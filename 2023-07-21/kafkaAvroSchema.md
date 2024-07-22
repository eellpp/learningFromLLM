### How kafka Schema Registry Works (AVRO)

Registering a Schema:

1) When a producer registers a schema, the Schema Registry assigns it a unique identifier (schema ID) and stores the schema under a subject.
If the schema already exists, the Schema Registry returns the existing schema ID.

**Producing Messages:**

The producer serializes the data using the registered schema.
Each serialized message includes a header with the schema ID, enabling consumers to retrieve the correct schema for deserialization.

**Retrieving a Schema:**

When a consumer receives a message, it extracts the schema ID from the message header.
The consumer fetches the schema from the Schema Registry using the schema ID, unless the schema is already cached locally.

**Enforcing Compatibility:**

When a new schema version is registered, the Schema Registry checks compatibility with the previous versions based on the configured compatibility level.

Notes:  
- the producer needs a schema file (avsc file). Needs to use the maven plugin to auto generate the class for schema. It then publishes the records
- The consumer may or may not use the schema file. But he needs to provide the schema registry url. For each incoming record, kafka checks the schema of the records against the schema registry (it keeps a local cache for quick lookup)
- On consumer side , by default, KafkaAvroDeserializerConfig.SPECIFIC_AVRO_READER_CONFIG is set to "false". Because of this by default kafka converts the record to GenericRecord. To convert to json, we can use the ByteArrayOutStream with the JsonEncoder to convert to json. This way consumer can generically convert avro message to json without converting to specific POJO

To convert to specific POJO, set SPECIFIC_AVRO_READER_CONFIG as "true" provide the class in deserializer 

** Example of specific producer **

```java
import io.confluent.kafka.serializers.KafkaAvroSerializer;
import org.apache.kafka.clients.producer.KafkaProducer;
import org.apache.kafka.clients.producer.ProducerRecord;
import org.apache.kafka.clients.producer.ProducerConfig;

import java.util.Properties;

public class AvroProducer {

    public static void main(String[] args) {
        Properties props = new Properties();
        props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringSerializer");
        props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, KafkaAvroSerializer.class.getName());
        props.put("schema.registry.url", "http://localhost:8081");

        KafkaProducer<String, User> producer = new KafkaProducer<>(props);

        User user = new User("Alice", 30);
        ProducerRecord<String, User> record = new ProducerRecord<>("user-topic", user);
        producer.send(record);
        producer.close();
    }
}


```


**Example of specific Consumer** 

```java
import io.confluent.kafka.serializers.KafkaAvroDeserializer;
import org.apache.kafka.clients.consumer.ConsumerConfig;
import org.apache.kafka.clients.consumer.ConsumerRecord;
import org.apache.kafka.clients.consumer.KafkaConsumer;

import java.util.Collections;
import java.util.Properties;

public class AvroConsumer {

    public static void main(String[] args) {
        Properties props = new Properties();
        props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        props.put(ConsumerConfig.GROUP_ID_CONFIG, "avro-consumer-group");
        props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
        props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, KafkaAvroDeserializer.class.getName());
        props.put("schema.registry.url", "http://localhost:8081");
        props.put("specific.avro.reader", "true");

        KafkaConsumer<String, User> consumer = new KafkaConsumer<>(props);
        consumer.subscribe(Collections.singletonList("user-topic"));

        while (true) {
            for (ConsumerRecord<String, User> record : consumer.poll(100)) {
                User user = record.value();
                System.out.println(user);
            }
        }
    }
}

```

### Maven Configuration for Avro Plugin (if using Maven)
This will auto generate the pojo class

```xml
<plugin>
    <groupId>org.apache.avro</groupId>
    <artifactId>avro-maven-plugin</artifactId>
    <version>1.10.2</version>
    <executions>
        <execution>
            <goals>
                <goal>schema</goal>
            </goals>
            <configuration>
                <sourceDirectory>${project.basedir}/src/main/avro</sourceDirectory>
                <outputDirectory>${project.basedir}/src/main/java</outputDirectory>
            </configuration>
        </execution>
    </executions>
</plugin>

```

### The data model of kafka message with schema  
The kafka record   
```bash
record.value() --> GenericData.Record   
((GenericData.Record)record.value()).schema ---> (Schema.RecordSchema)
(Schema.RecordSchema(((GenericData.Record)record.value()).schema).name ---> com.example.myproducermodule.model.MyAvroMessage
```


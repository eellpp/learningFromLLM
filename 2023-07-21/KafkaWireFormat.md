
Kafka Uses Byte Array . Producer writes byte array and Consumer reads byte array.

### Kafka Record Wire Format Breakdown
Each record in a batch includes a key, value, and other metadata, while the batch itself contains information about the collection of records. Understanding this format is essential for working with Kafka at a low level, especially when implementing custom serializers or interacting with Kafka directly over the network.

Below is a simplified example that shows the order and structure of a Kafka record as it appears on the wire. The example assumes a Kafka version 0.11 or later, which uses the record batch format.

### Wire Format Structure
`Record Batch`: The outer structure that contains metadata and multiple records.
`Record`: The individual messages within the batch.


***Record Batch Structure** 

BaseOffset`: The base offset of the batch.  
BatchLength: The total length of the batch, including all records.  
PartitionLeaderEpoch: The leader epoch of the partition.  
Magic: The version number (2 for record batch format).  
CRC: Checksum of the batch.  
Attributes: Metadata about the batch (e.g., compression type).  
LastOffsetDelta: The offset of the last record relative to the base offset.  
FirstTimestamp: Timestamp of the first record in the batch.  
MaxTimestamp: Maximum timestamp of records in the batch.  
ProducerId: ID of the producer.  
ProducerEpoch: Epoch of the producer.  
BaseSequence: Sequence number of the first record.  
RecordsCount: Number of records in the batch.  
Records: The actual records (messages).  


**Record Structure**

Length: Length of the record.  
Attributes: Metadata about the record.  
TimestampDelta: Timestamp difference from the first record's timestamp.  
OffsetDelta: Offset difference from the base offset.  
KeyLength: Length of the key.  
Key: The key of the record.  
ValueLength: Length of the value.  
Value: The actual value (payload) of the record.  
HeadersCount: Number of headers.  
Headers: Key-value pairs for additional metadata.  

Record Batch
```bash
+-------------------+------------------------------------------+
| Field             | Value                                    |
+-------------------+------------------------------------------+
| BaseOffset        | 42                                       |
| BatchLength       | 136                                      |
| PartitionLeaderEpoch | 0                                    |
| Magic             | 2                                        |
| CRC               | 123456789                                |
| Attributes        | 0                                        |
| LastOffsetDelta   | 2                                        |
| FirstTimestamp    | 1623456789000                            |
| MaxTimestamp      | 1623456789200                            |
| ProducerId        | -1                                       |
| ProducerEpoch     | -1                                       |
| BaseSequence      | -1                                       |
| RecordsCount      | 3                                        |
+-------------------+------------------------------------------+

```

Records
```bash
+-----------------+---------------------+------------------+-----------------+
| Record 1        | Record 2            | Record 3         |                 |
+-----------------+---------------------+------------------+-----------------+
| Length: 34      | Length: 36          | Length: 34       |                 |
| Attributes: 0   | Attributes: 0       | Attributes: 0    |                 |
| TimestampDelta: 0| TimestampDelta: 100| TimestampDelta: 200|               |
| OffsetDelta: 0  | OffsetDelta: 1      | OffsetDelta: 2   |                 |
| KeyLength: 3    | KeyLength: 3        | KeyLength: 3     |                 |
| Key: "key1"     | Key: "key2"         | Key: "key3"      |                 |
| ValueLength: 10 | ValueLength: 11     | ValueLength: 10  |                 |
| Value: "value_one"| Value: "value_two"| Value: "value_three"|              |
| HeadersCount: 0 | HeadersCount: 0     | HeadersCount: 0  |                 |
+-----------------+---------------------+------------------+-----------------+

```

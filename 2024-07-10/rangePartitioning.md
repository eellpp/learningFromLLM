### When to use range partitioning in oracle database ?

Range partitioning in an Oracle database is a useful technique when dealing with large tables where the data can be logically divided into ranges. Here are some scenarios when range partitioning should be considered:

Temporal Data:

Time-Based Data: If your table stores time-based data such as sales records, logs, or events, range partitioning can help. For example, partitioning by date (e.g., monthly or yearly) allows for efficient querying and maintenance of data.
Data Archiving:

Archiving Old Data: When old data needs to be archived or purged regularly, range partitioning makes it easier to drop or archive older partitions without affecting the rest of the table.
Query Performance:

Range Queries: If your queries frequently filter data based on a range of values (e.g., finding orders within a specific date range), range partitioning can significantly improve query performance by scanning only relevant partitions.
Data Distribution:

Even Distribution: If data can be evenly distributed across partitions based on a range of values, it can help with load balancing and parallel processing, leading to better overall performance.
Maintenance:

Maintenance Operations: Maintenance operations like index rebuilds, backups, or statistics gathering can be performed on individual partitions instead of the entire table, making these operations more manageable and less resource-intensive.
Partition Pruning:

Partition Pruning: Oracle can eliminate (prune) partitions that do not match the query predicates, which can significantly reduce the amount of data scanned during a query, thus speeding up query execution.
Data Loading:

Efficient Data Loading: Bulk loading data into a specific partition can be faster and more efficient than loading into an unpartitioned table.

example
```bash
CREATE TABLE sales (
  sale_id      NUMBER,
  sale_date    DATE,
  amount       NUMBER
)
PARTITION BY RANGE (sale_date) (
  PARTITION p2022 VALUES LESS THAN (TO_DATE('01-JAN-2023','DD-MON-YYYY')),
  PARTITION p2023 VALUES LESS THAN (TO_DATE('01-JAN-2024','DD-MON-YYYY')),
  PARTITION p2024 VALUES LESS THAN (TO_DATE('01-JAN-2025','DD-MON-YYYY'))
);
```

### Can you range partition to only last 30 days interval based on date column, without hardcoding in create table statement ?

Yes, you can implement range partitioning for a rolling 30-day interval without hardcoding each partition in the CREATE TABLE statement by using interval partitioning. Interval partitioning automatically creates partitions as data is inserted, based on a specified interval.

Here’s an example of how to set up a table with interval partitioning for a rolling 30-day interval based on a date column:

```bash
CREATE TABLE sales (
  sale_id    NUMBER,
  sale_date  DATE,
  amount     NUMBER
)
PARTITION BY RANGE (sale_date)
INTERVAL (NUMTODSINTERVAL(30, 'DAY'))
(
  PARTITION p_initial VALUES LESS THAN (TO_DATE('01-JAN-2024','DD-MON-YYYY'))
);

```
**Explanation**
PARTITION BY RANGE (sale_date): Specifies that the table is partitioned by the sale_date column.  
INTERVAL (NUMTODSINTERVAL(30, 'DAY')): Specifies that each partition will cover a 30-day interval.   
PARTITION p_initial VALUES LESS THAN (TO_DATE('01-JAN-2024','DD-MON-YYYY')): Defines an initial partition for dates before January 1, 2024. This is necessary to initialize the interval partitioning scheme.
With this setup, Oracle automatically creates new partitions as needed when data is inserted, ensuring that partitions are created for each 30-day interval. You don't need to manually manage the partitions for each specific 30-day period.  

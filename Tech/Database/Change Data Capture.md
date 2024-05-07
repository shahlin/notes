# Change Data Capture (CDC)

1. Process of recognizing when data has changed in a **source system** so that a **downstream system** can taken an action based on that change
2. In other words, change data capture is a method of ETL (Extract, Transform, Load) where data is extracted from a source, transformed, and then loaded to a target repository
3. Every change in the database, it will write that change to Kafka
4. Example of downstream systems: Another database, stream processing, etc
5. Downstream systems (users of this change event notification) are subscribed to the appropriate Kafka topic

## Basic flow
1. Source system pushes changes to Kafka
2. Target system subscribes and listens to topic and consumes messages
3. Target system applies changes

## Use cases
1. Replicate data into other databases like data warehouses or data lakes
2. Stream processing based on data changes. Example, sending an email if some value goes above threshold
3. Invalidate or update cache. Example, in case of deletes or updates, you can invalidate or update the cache

## CDC Methods
1. Log based CDC - The most popular way is to use a transaction log which records all the changes made to a database in a log file. 
2. Query-based CDC - Query the data in the source to pick up changes. This method requires that the source system (database table) has some kind of timestamp using which latest changes can be queried.
3. Trigger-based CDC - In this approach, you change the source application to trigger the write to a change table and then move it. This approach reduces database performance because it requires multiple writes each time a row is updated, inserted, or deleted.


## References
1. [Youtube - Change Data Capture (CDC) Explained (with examples)](youtube.com/watch?v=5KN_feUhtTM)
2. https://www.qlik.com/us/change-data-capture/cdc-change-data-capture

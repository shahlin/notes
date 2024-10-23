# Replication
- Replication is the process of copying the data over to multiple database servers
- Each replica contains the complete copy of the dataset
- This provides redundancy and high availability in case one of the database servers go down
- Replication can be achieved using CDC

![replication](https://github.com/user-attachments/assets/d5bec756-e590-445f-bb87-23303329b959)

## When to use Replication?
- When high availability and fault tolerance is required
- When reads are more than writes
- Global availability (like CDNs)

## Replication Strategies
- Full-table replication
- Key-based incremental replication
- Log-based replication

# Sharding
- Database partitioning technique wherein the data is divided into smaller chunks called Shards
- Each shard is stored on a different database server to distribute the load
- Fault tolerance is present within shards. If a shard goes down, other shards continue to live

![range-based sharding](https://github.com/user-attachments/assets/3296be35-cd19-46dd-a994-6fedb46af428)

## When to use Sharding?
- When you have large datasets that require horizontal scalability
- Good for high reads and writes requirements

## Sharding Strategies
- Vertical Sharding
- Hash Sharding
- Range Sharding
- Composite Sharding
- Horizontal Sharding

# References
- https://static.pingcap.com/files/2024/02/14133212/Range-Based-Sharding.png

# Distribution

 * Most NoSQL are specially desgined for running on clusters
 * But not all (e.g., graph databases)
 * Single server vs sharding and/or replication

## Sharding

 * Scales writes
 * How do users get all the data from a single server?
 * Aggregates are the unit of distribution
 * Can be handled in application logic
 * Many NoSQL databases offer auto-sharding

## Master-Slave Replication

 * Scales reads
 * Read resilience
 * Affects read consistency

## Peer-to-Peer Replication
 
 * Scales reads and writes
 * No single point of failure
 * Affects read and write consistency
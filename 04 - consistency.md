# Consistency

 * Relational databases exhibit strong consistency
 * NoSQL stores allow you to relax consistency
 * Consistency requirements per operation or per request

## Write consistency

 * Lost updates
 * Concurrency control
 * Pessimistic
 * Optimistic - conditional updates
 * Vector clocks and version vectors

## Read consistency

 * Inconsistent reads
 * Logical consistency 
 * Inconsistency window
 * Replication consistency
 * Eventually consistent
 * Read-your-writes consistency

## CAP Theorem

 * Consistency
 * Partition tolerance
 * Availability

   > Every request received by a nonfailing node in the system
   > must result in a response
   
 * Tradeoff between consistency and availability/latency

## Durability

 * Can also be sacrificed
 * Periodically flush writes to disk
 * What if the master node fails before replication?

## Quorums

 * The tradeoff is flexible
 * Replication factor - N
 * Write quorum - W > N/2
 * Read quorum - R + W > N

# Data Models

 * How we perceive the data
 * Not the storage model
 * Typically, the relational data model

## Aggregates

 * Unit for data manipulation
 * Unit of distribution
 * Transactional boundary
 * Relationships are not enforced
 * Denormalization
 * Typically, no atomic operations spaning multiple aggregates (exception: RavenDB)
 * Schemaless

## Key-value and document stores

 * Lookup by ID
 * The value can be opaque (key-value) or used by queries (document)
 * For key-value stores, you can integrate search tools for query support
 * Model for data access

## Column-family stores

 * Confusing model
 * Sparse table: columns can be added to any row, and rows can have different columns 
 * Two-level map: first key identifies the row, second one identifies the column
 * Column families (super-columns in Cassandra)
 * Storage model more suited for reading
 * Columns are ordered (name, timestamp, etc) 
 * Model for data access

## Materialized views

 * Cached queries
 * Can be stale
 * Can be included in a document
 * Map-reduce

## Graphs

 * Small records with complex interconnections
 * Nodes connected by edges
 * More performant than relational databases
 * Not suitable for clusters
 * Transactions need to span multiple nodes

## Facts

 * Immutability
 * Time
 * Storage is cheap
 * Event sourcing
 * Datomic (Memory Image)

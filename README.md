# 🏎 metaData-filtering-engine
The goal of this project is to create an engine that efficiently navigates accurate data through complex distributed systems and bloom filters, ultimately improving hard-ware partitioning within databricks. Solving a small file problem and data skew, impact of performance and budget in big data environments. 

# Storage Layer Optimization (Foundation)
Structure data so the engine can skip unnecessary files in order to optimize I/O (Input/Output). Implement Delta Lake, first ensure that data is stored in a Delta format. Delta enables ACID transactions and meta data handling for advanced skipping. Apply z-ordering on high cardinality columns as it allows mapping of multi dimensional data to one dimension while preserving locality (when querying one of these columns, databricks skips files that fall outside of z-bound). Create bloom filter indexes for columns that are frequently used in point look ups, this is the probabilistic data structure that communicates with the engine if a value might exist or does not exist at all. 

# Building the Meta-Data Driven Engine 
Rather than hard coding filters, building the layer that dynamically generates Spark SQL based configuration file for “metadata table.” Create a schema definition that maps data to their physical storage location and filter type. Write a python or pyspark wrapper that read the meta data and builds the filter or where clauses. Predicate pushdown to ensure that the engine pushes these filters down to the level so spark does not load the entire data into memory before filtering. 

# Performance of Hardware Aware Partitioning 
Analyze data distribution by identifying if certain keys have significantly more data than others. If partitioning is too large for a single executor RAM, implement salting which adds a random prefix to the key to break it into smaller manageable chunks. Match partition sizes to core/RAM ratio of databricks workers through cluster matching. 

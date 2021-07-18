Best Practices for designing and architecting with Dynamodb

## RDBMS vs NoSQL:

1. RDBMS can be queried flexibly, but queries are relatively expensive and dont scale in high traffic situations
2. In NoSQL queries can only be made in limited number of ways in which they are effective, outside which queries are expensive and slow

Database design for each of the system thus has to be different.

## Normalization of data model

Database normalization is the process of decomposing tables to eliminate data redundancy and undesired characteristics like insertion anamolies

ELI5: structure tables and attributes such that there is no/less redundant data and the dependencies make sense.

## Differences in design approach of RDBMS vs DynamoDB

-   In DynamoDB, you design your schema specifically to make the most common and important queries as fast and as inexpensive as possible. Your data structures are tailored to the specific requirements of your business use cases.
-   In RDBMS we are focused on creating a normalized data model without worrying about the implementation details or performance

## Approach NoSQL design

1. The first step in design is to identify the specific query patterns that the system must satisfy.
2. fundamental properties:
	1. data size: knowing how much data will be stored and requested at one time will help determine the most effective way to partition data
	2. shape of data stored should correspond to shape of data returned from a query. This increases speed and scalability, Unlike RDBMS where the data is reshaped. 
	3. Data velocity: By increasing the number of physical partitions, dynamodb scales.
3. General governing principles:
	1. **Keep related data together** - As a general rule, maintain as few tables as possible. 
	2. Use sort order - related items should have a sortable
	3. Distribute queries - Design data keys to distribute traffic evenly across partitions avoiding "hot spots"
	4. Use GSIs


## Primary key

Primary key is made of partition key only or composite meaning partition and sort key. A partition key identifies a logical partition in which table data is stored. The provisioned I/O resources are (generally) equally distributed across partitions, so partition key must be designed such that, no one partition would get "hot" all the time.

For example in a users table, `User ID` is a good partition key, in a devices table `Device ID` could be a good key provided no one device is more popular than others. `Status Code` is a Bad key if there are only few codes.

## Sharding

Process of breaking up large tables into smaller chunks called shards

If `date` is the partition key, we can add a random number from 1 to say 200 to create more partitions.

`22-07-2021.34` here for each write a random rumber in this case 34 is added to make the key and stored. This improves write efficiency. 

This strategy called random suffixes, is great for write, but makes querying hell.

We can make use of hash functions to come up with more predictable suffixes, this strategy is called calculated suffixes.


## Sort key

well designed sort keys have benifits like:

1. Gather related information in one place which can lead to efficient queries. We can make use of sort key operations like `>, <, >=, <=, ==, begins_with, between`
2. Define hierarchical relationships and query at any level of the hierarchy

example: 

```
[country]#[region]#[state]#[county]#[city]#[neighborhood]
```

We can query for 

`Key('sort_key').begins_with('india')`
`Key('sort_key').begins_with('india#north')`
`Key('sort_key').begins_with('india#north#jammu')` etc

3. sort keys can be used for version control

## Indexes

1. GSI - Index with partition key and sort key different from that of base table. Its considered global because a query can span across partitions in base table. GSI has no size limitations and has its own provisoned throughput settings seperate from base table's read and write units
2. LSI - same partition key as the base table, but different sort key. The total size of indexed items for any one partition key cannot exceed 10GB. shared proviosioned read write units with base table.

Default quota: 20GSIs, 5LSIs

General rule: Use GSIs rather than LSIs


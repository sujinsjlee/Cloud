# Storage in the Cloud

## Cloud Storage

> Cloud Storage, which is a service that offers developers and IT organizations durable and highly available **object storage**.

- These objects are stored in a packaged format which contains the binary form of the actual data itself, as well as relevant associated meta-data (such as date created, author, resource type, and permissions), and a globally unique identifier.

- **BLOB**
    - Cloud Storage’s primary use is whenever binary large-object storage (also known as a “BLOB”) is needed for online content such as videos and photos, for backup and archived data and for storage of intermediate results in processing workflows. 

- **bucket**
    - A bucket needs a globally unique name and a specific geographic location for where it should be stored, and an ideal location for a bucket is where latency is minimized.

- Because storing and retrieving large amounts of object data can quickly become expensive, Cloud Storage also offers **lifecycle management policies**. 

## Cloud SQL

> Cloud SQL offers fully managed relational databases, including MySQL, PostgreSQL, and SQL Server as a service.


- A benefit of Cloud SQL instances is that they're accessible by other google Cloud services and even external services. 

## Cloud Spanner

> Cloud Spanner is a fully managed relational database service that scales horizontally, is strongly consistent, and speaks SQL. 


- Cloud Spanner is especially suited for applications that require: A SQL relational database management system with joins and secondary indexes Built-in high availability Strong global consistency And high numbers of input and output operations per second. We’re talking tens of thousands of reads and writes per second or more.

## Firestore

> Firestore is a flexible, horizontally scalable, NoSQL cloud database for mobile, web, and server development.


- Firestore synchronizes any local changes back to Firestore. Firestore leverages Google Cloud’s powerful infrastructure: automatic multi-region data replication, strong consistency guarantees, atomic batch operations, and real transaction support. 

## Cloud Bigtable

> Cloud Bigtable is Google's NoSQL Big data database service.


- It's the same database that powers many core Google services including search, analytics, maps, and Gmail. Bigtable is designed to handle massive workloads at consistent low latency and high throughput. So it's a great choice for both operational and analytical applications including internet of things, user analytics, and financial data analysis.

- they're running machine learning algorithms on the data.

## Comparing storage options

- Consider using Cloud Storage if you need to store immutable blobs larger than 10 megabytes, such as large images or movies. This storage service provides petabytes of capacity with a maximum unit size of 5 terabytes per object. 

- Consider using Cloud SQL or Cloud Spanner if you need full SQL support for an online transaction processing system. Cloud SQL provides up to 64 terabytes, depending on machine type, and Cloud Spanner provides petabytes. Cloud SQL is best for web frameworks and existing applications, like storing user credentials and customer orders. If Cloud SQL doesn’t fit your requirements because you need horizontal scalability, not just through read replicas, consider using Cloud Spanner. 

- Consider Firestore if you need massive scaling and predictability together with real time query results and offline query support. This storage service provides terabytes of capacity with a maximum unit size of 1 megabyte per entity. Firestore is best for storing, syncing, and querying data for mobile and web apps. 

- Finally, consider using Cloud Bigtable if you need to store a large number of structured objects. Cloud Bigtable doesn’t support SQL queries, nor does it support multi-row transactions. This storage service provides petabytes of capacity with  a maximum unit size of 10 megabytes per cell and 100 megabytes per row. Bigtable is best for analytical data with heavy read and write events, like AdTech, financial, or IoT data. 

## BigQuery

- The usual reason to store data in BigQuery is so you can use its big data analysis and interactive querying capabilities, but it’s not purely a data storage product.
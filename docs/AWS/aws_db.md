# AWS Database
> [RDS](#RDS)  
> [DynamoDB](#DynamoDB)  
> [Aurora](#Aurora)  
> [Database Types](#types)  
> [Redshift](#Redshift)  
   

## RDS
- [RDS snapshot](https://aws.amazon.com/ko/premiumsupport/knowledge-center/encrypt-rds-snapshots/)
    - Relational Database Service (Amazon RDS) 
    - **You can't take an encrypted snapshot of an unencrypted DB instance.** However, you can perform a workaround that achieves the same results. The following steps are applicable to Amazon RDS for MySQL, Oracle, SQL Server, PostgreSQL, or MariaDB.

    - Important: If you use Amazon Aurora, you can restore an unencrypted Aurora DB cluster snapshot to an encrypted Aurora DB cluster. However, you must specify an AWS Key Management Service (AWS KMS) encryption key when you restore from the unencrypted DB cluster snapshot. 
        1.    Open the Amazon RDS console, and then choose Snapshots from the navigation pane.
        2.    Select the snapshot that you want to encrypt.
        3.    Under Snapshot Actions, choose Copy Snapshot.
        4.    Choose your Destination Region, and then enter your New DB Snapshot Identifier.
        5.    Change Enable Encryption to Yes.
        6.    Select your AWS KMS Key from the list.
        7.    Choose Copy Snapshot.

- [RDS Read Replica](https://aws.amazon.com/rds/features/read-replicas)
    - You can reduce the load on your source DB instance by routing read queries from your applications to the read replica. Read replicas allow you to elastically scale out beyond the capacity constraints of a single DB instance for read-heavy database workloads. Because read replicas can be promoted to master status, they are useful as part of a sharding implementation.
    - To further maximize read performance, Amazon RDS for MySQL allows you to add table indexes directly to Read Replicas, without those indexes being present on the master.

### RDS Security - Encryption
> At rest encryption  


-  Possibility to encrypt the master & read replicas with AWS KMS - AES-256 encryption
- Encryption has to be defined at launch time
- If the master is not encrypted, the read replicas cannot be encrypted
- Transparent Data Encryption (TDE) available for Oracle and SQL Server

> In-flight encryption  


- SSL certificates to encrypt data to RDS in flight
- Provide SSL options with trust certificate when connecting to database
- To enforce SSL:
    - PostgreSQL: rds.force_ssl=1 in the AWS RDS Console (Parameter Groups)
    - MySQL: Within the DB: GRANT USAGE ON *.* TO 'mysqluser'@'%' REQUIRE SSL;


## DynamoDB
- Dynamo DB does not perform joins
- ~JSON
### DynamoDB – Time To Live (TTL)
- Automatically delete items after an expiry timestamp
### DynamoDB - Usecase
- [Deliver seamless retail experiences](https://aws.amazon.com/dynamodb/)
    - Use design patterns for deploying shopping carts, workflow engines, inventory tracking, and customer profiles. DynamoDB supports high-traffic, extreme-scaled events and can handle millions of queries per second.

## Aurora
- **Aurora Serverless** – for unpredictable / intermittent workloads
- **Aurora Multi-Master** – for continuous writes failover
- **Aurora – Custom Endpoints** : 
    - Define a subset of Aurora Instances as a Custom Endpoint
    - The Reader Endpoint is generally not used after defining Custom Endpoints
    - Aurora uses single reader endpoint for all replica nodes.
    - We can create new custom endpoint for the workload.
- Aurora **storage automatically grows** in increments of 10GB, up to 128 TB.
- Aurora can have 15 replicas while MySQL has 5, and **the replication process is faster** (sub 10 ms replica lag)
- **Failover in Aurora is instantaneous**. It’s HA (High Availability) native.

## types
- **RDBMS (= SQL / OLTP)**: RDS, Aurora – great for joins
- **NoSQL database**: DynamoDB (~JSON), ElastiCache (key / value pairs), Neptune (graphs) – no joins, no SQL
- **Object Store**: S3 (for big objects) / Glacier (for backups / archives)
- **Data Warehouse (= SQL Analytics / BI)**: Redshift (OLAP), Athena
- **Search**: ElasticSearch (JSON) – free text, unstructured searches
- **Graphs**: Neptune – displays relationships between data

## Redshift
> It’s OLAP – online **analytical processing** (analytics and data warehousing)  

- Q Does Amazon Redshift support Multi-AZ Deployments?
    - **Redshift has no “Multi-AZ” mode**
    - Currently, Amazon Redshift only supports Single-Region deployments
- [Amazon Redshift Improves Performance of Inter-Region Snapshot Transfers : Enable cross-Region snapshots](https://aws.amazon.com/about-aws/whats-new/2019/10/amazon-redshift-improves-performance-of-inter-region-snapshot-transfers/)

> Redshift Spectrum: perform queries directly against S3 (no need to load)  
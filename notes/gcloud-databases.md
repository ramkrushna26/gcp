# Cloud SQL
Fully **managed** relational database service
  * configure what you want & NOT worry about managing the database
  * support MySQL, PostgreSQL, SQL Server
  * regional service providing high availability (99.95%)
  * use SSDs (recommended) / HDDs
  * upto 416GB of RAM & 30TB of storage
    
use cloud SQL for simple relational use cases
  * to migrate local MySQL, PostgreSQL, SQL Server databases
  * to reduce maintenance costs of simple relational databases
  * use **cloud spanner** instead of cloud SQL if :
    * have huge volume of relational data (TBs) or
    * need infinite scaling for growing application or
    * need globle (distributed accross multiple region) database or
    * need high availability (99.99%)

```
$ gcloud config set project glowing-furnace-304608  

# for connecting to cloud database  
$ gcloud sql connect my-cloud-sql-instance --user=root --quiet
$ gcloud sql connect my-cloud-postgres-instance --user=postgres --quiet
```
### MySQL Commands
```
> use todos
> create table user (id integer, username varchar(30) );
> describe user;
> insert into user values (1, 'Ranga');
> select * from user;
```

## Cloud SQL - Features
Automatic encryption (tables/backups), maintenance & updates  
High availibility & failover  
  * create standby with automatic failover
  * pre-requisites - automated backups & binary logging  
Read replicas for read workloads  
  * options: cross-zone, cross-region & external (non cloud sql db)
  * pre-requisites - automated backups & binary logging  
Automatic storage increase without downtime (for new versions)
Point-in-time recovery: enable binary logging
Backupes (automated & on demand)
Supports migration from other sources (use database migration service - DMS)
You can export data from UI or gcloud with formats


## Cloud SQL - High Availability
Create a high availibility configurations  
  * choose primary & secondary zones within a region
  * 2 instances -- primary & secondary  
Changes from primary are replicated synchronunsly to secondary  
In case of zonal failure, automatic failover to secondary instances  
  * if primary zone available, failover does not revert automatically
High availibility setup can not be used as read replica  

![image](https://github.com/ramkrushna26/gcp-notes/assets/45620457/18ac1478-2aa2-47ba-9b45-6fbb7bc01933)

## Cloud SQL - Commands
```
$ gcloud sql instances create/clone/describe/delete/patch
OPTIONS:
  patch --backup-start-time
```
```
$ gcloud sql databases create/delete/describe/list/patch
```
```
$ gcloud sql connect <instance>
OPTIONS:
  --database
  --user
```
```
$ gcloud sql backups create/describe/list
```

# Cloud Spanner

**Fully Managed**, mission critical, relational(SQL), glabaly distributed database with very high availibility (99.999%)  
  * strong transactional consistency at global scale
  * **scales to PBs** of data with automatic sharding  
Cloud spanner scales horizontally for reads & writes  
  * configure number of nodes
  * in camparision to cloud sql, which provides read replica but can not horizontally scale write operations  
Regional & Multi-regional configurations
Expensive compared to cloud sql: pay for nodes & storage
Data export: use cloud console & automated export using data flow  

## Cloud Spanner - Queries  
```
CREATE TABLE Users (
  UserId   INT64 NOT NULL,
  UserName  STRING(1024)
) PRIMARY KEY(UserId);
```

# Cloud Datastore (Firestore)
**Datastore** - Highly scalable NoSQL document database  
  * automatically scales partitions data as it grows
  * recommended for few TBs of data
  * supports transactions, indexes, sql like queries (GQL) (does NOT support joins & aggregates)
  * for use cases where flexible schema needed with transactions (user profile & product catalog)
  * structure: kind > enrity (use namespaces to group entities)
  * export can be onlt from gcloud (not from console)
    * export contains metadata & folder with data  

**Firestore** - datastore++ - optimized for multi-device access
  * offline mode & data synchronization across multiple devices - mobile, IOT, etc
  * provides client side libraries - Web, android, iOS etc
  * offers datastore & native nodes

# Cloud BigTable

**Petabyte scale, wide column** NoSQL DB (HBase API compatibility)  
  * designed for huge columes of analytical & operational data (IoT streams, analytics, time series data)
  * handles millions of read/write TPS at very low latency
  * single row transaction (multi row transactions NOT supported)  
**NOT Serverless** - need to create server instances (use SSD or HDD)  
  * Scale horizontally with multiple nodes (no downtown for cluster re-sizing)  
can NOT export data using cloud console or gcloud (use either java -jar JAR export/import or HBase commands)
Use **cbt** command line tool to work with BigTable (ex: cbt createtable <table>)
At the most basic level, each table is a sorted key/value map  
  * each value in row is indexed using a key - row key
  * related columns are grouped into column families  
    * column identified by column-family:column-identifier  
This structure supports high read & write throuput at low latency
Scalable to pytabytes of data with millisecond response upto millions of TPS
**Use case**: IOT streams, graph data, real-time analytics (time-series, stock)
**Cloud Dataflow**: used to export data from bigtable to cloudstorage  

## Cloud BigTable (cbt) - Commands
* **cbt** - CLI for Cloud BigTable
```
gcloud --version
cbt listinstances -project=glowing-furnace-304608
```
* Recommanded approach: to set project & verify installation
```
echo project = glowing-furnace-304608 > ~/.cbtrc
cat ~/.cbtrc
cbt listinstances
```
Configure project with cbt in **.cbtrc** file  
```
$ cbt createinstance
$ cbt createcluster - create cluster configured instances
$ cbt createtable/deleteinstance/deletecluster/deletetable
$ cbt ls -list tables and column families
```

# MemoryStore
**In-memory datastore service**: reduce access time  
**Fully managed** (provisioning, failover, replication & patching)  
  * highly available (99.9%)
  * monitoring can be easily setup using cloud monitoring  
Support for **Redis & Memcached**
  * Use memcached for caching (reference data, database query cache, session store)
  * Use redis for low latency access with presistence & high availability (gaming leader board, palyer profile, in-memory streaming)  

# BigQuery - Datawarehousing
**Exabyte scale modern datawarehousing** solution from GCP  
  * relational database (SQL, schema, consistency, etc)
  * use sql like commands to query large datasets
  * traditional (storage+compute) + modern (realtime + serverless)  

Import & Export
  * load data from variety of source, incl. streaming data (variety of import formats)
  * export to cloud storage & data studio  
Automatically expires data (configure table expiration)
**QUery external data sources without storing data in bigquery**
  * cloud storage, cloud sql, bigtable, google drive
  * use permanent & temporary external tables  

## BigQuery - Accessing & Querrying data
Access database using :
  * cloud console
  * bq command line tool
  * BigQuery rest API
  * HBase API based libraries  
BigQuery queries can be **expensive** as you are running them on large datasets
**Estimate big query** before running:
  * use UI/bq(--dry-run) -- get estimated scanned data volume
  * use pricing calculator - find price of scanning 1MB of data  

## BigQuery (bq) - Commands
```
$ bq show bigquery-public-data:samples.shakespeare
$ bq query <query>
  --dry-run
$ bq extract
$ bq load
```

# Relational Databases - Import & Export
**Cloud SQL**: to/from cloud storage from console/gcloud/rest API
  * for large datasets, use serverless mode  
```
$ gcloud sql export/import csv/sql
```

**Cloud Spanner**: to/from cloud storage from console (use cloud data flow)  

**BigQuery**: to/from cloud storage & others from bq/console
```
$ bq extract/load
```
  * variety of options to import data
    * load data from cloud storage (Data store -> cloud storage -> bigquery)
    * batch loading using bigquery data transfer service
    * use dataflow to setup streamline pipeline


# NoSQL Databases - Import & Export
**Cloud Datastore/Firestore**: to/from cloud storage from console/gcloud/rest API
```
$ gcloud datastore/firestore export/import  --kinds --namespaces
```
**Cloud BigTable**: to/from cloud storage - create dataflow jobs
  * formats: Avro/Parquet/SequenceFiles
Ensure that service accounts have access to cloud storage buckets
  * ACL
```
$ gsutil acl ch -U <service account> :W <bucket>
```
  * OR roles **Storage Admin** or **Storage Object Admin** or **Storage Object Owner**

# Summary
* BigQuery, Datastore, Firebase does NOT need VM configurations
* Cloud SQL, BigTable need VM configuration  

* Relational databases
  * small local database - Cloud SQL
  * Highly scalable global database - Cloud Spanner
  * Datawarehouse - BigQuery  

* NoSQL databases
  * transactional database for few TB of data  - Cloud Datastore
  * Huge volume of IoT / streaming data - Cloud BigTable





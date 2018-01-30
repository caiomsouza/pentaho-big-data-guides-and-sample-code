# Guides

Pentaho Big Data Guides <BR>

### HDFS Commands - List files and folders
```
hdfs dfs -ls hdfs://demouser@pentahobdvm.localdomain:8020/
```

### HDFS Commands - Create a folder
```
hdfs dfs -mkdir hdfs://demouser@pentahobdvm.localdomain:8020/demo-twitter
```

More HDFS commands:
https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html

All commands were tested with Impala<BR>

### SHOW DATABASES
```
show databases;
```

### USE DATABASE
```
use database_name;
```

### SHOW TABLES
```
show tables;
```

### DESCRIBE TABLE
```
describe test_table;
```

### COMPUTE TABLE STATS
```
compute stats test_table;
```

### SHOW TABLE STATS
```
show table stats test_table;
```

### SHOW COLUMN STATS
```
show column stats test_table;
```

### CREATE TABLE
```
CREATE TABLE test_table ( 
key    String, 
call_date    String, 
source_number    String, 
call_month    String, 
call_year    String, 
day_of_week    String, 
area_code    String,
state    String, 
country    String, 
time_zone    String, 
weekday    String, 
num_calls    String);
```

### CREATE A NEW TABLE BASED ON ANOTHER TABLE
```
create table your_new_table like your_old_table;
```

### INSERT DATA TO YOUR NEW TABLE FROM YOUR OLD TABLE
```
insert overwrite table your_new_table select * from your_old_table;
```

### CREATE A NEW TABLE (PARQUET FORMAT) BASED ON ANOTHER TABLE
```
create table your_new_table like your_old_table stored as parquetfile;
```

### INSERT DATA TO YOUR NEW TABLE (PARQUET FORMAT) FROM YOUR OLD TABLE
```
insert overwrite table your_new_table select * from your_old_table;
```

### DROP TABLE
```
drop table test_table;
```

### Example of Selects 
```
select * from test_table;
```

```
select count(*) from test_table;
```
```
select distinct country from test_table;
```
```
select * from test_table
where country = 'SPAIN';
```
```
select * from test_table
where country = 'SPAIN'
order by num_calls;
```

### EXPLAIN SELECT
```
explain select * from test_table;
```

### Refresh and Invalidate

Marks the metadata for one or all tables as stale. Required after a table is created through the Hive shell, before the table is available for Impala queries. The next time the current Impala node performs a query against a table whose metadata is invalidated, Impala reloads the associated metadata before the query proceeds. This is a relatively expensive operation compared to the incremental metadata update done by the REFRESH statement, so in the common scenario of adding new data files to an existing table, prefer REFRESH rather than INVALIDATE METADATA. <BR><BR>

Source:<BR>
https://impala.apache.org/docs/build/html/topics/impala_invalidate_metadata.html<BR>

### Invalidate Table
```
invalidate metadata test_table;
```

### Refresh Table
```
refresh test_table;
```

### Leave Hadoop Cluster from Safe Mode
```
hdfs dfsadmin -safemode leave
```

Error log:
```
2018/01/16 18:41:22 - Hadoop Copy Files - Resources are low on NN. Please add or free up more resources then turn off safe mode manually. NOTE:  If you turn off safe mode before adding resources, the NN will immediately return to safe mode. Use "hdfs dfsadmin -safemode leave" to turn safe mode off.
```

* Clean Hadoop Trash to free up disk space

### Check disk space
```
df -h
```

### Remove HDFS Folders:
```
hadoop fs -rm -r -f hdfs://pentahovm:8020/airline_demo
hadoop fs -rm -r -f hdfs://pentahovm:8020/poc-uk-price-paid-data
hadoop fs -rm -r -f hdfs://pentahovm:8020/poc-cs
```

### Check running services (Pentaho VM)
```
sudo service --status-all
```



## PARTITIONING TABLES
Before you continue please read the documentations below:

* [Tuning Impala for Performance](https://www.cloudera.com/documentation/enterprise/5-8-x/topics/impala_performance.html)
* [Impala Frequently Asked Questions](https://www.cloudera.com/documentation/enterprise/5-6-x/topics/impala_faq.html)


### Partitioning an internal Impala Table (TEXT FORMAT)

First create a partitioned table (in this case we are partitioning by year)

```
create table census_partitioned_example (name string, census_year int) partitioned by (year int);
```

### Create the partitions (TEXT FORMAT)
```
alter table census_partitioned_example add partition (year=2010);
alter table census_partitioned_example add partition (year=2011);
alter table census_partitioned_example add partition (year=2012);
alter table census_partitioned_example add partition (year=2013);
```

### Check if the partitiones were created 
```
show table stats census_partitioned_example
```

Output:
![Image show table stats census_partitioned_example](https://github.com/caiomsouza/pentaho-big-data-guides-and-sample-code/blob/master/guides/images/show_table_stats_from_a_partitioned_table.PNG)

### Insert data into your partitioned table (TEXT FORMAT)
```
insert into census_partitioned_example partition (year=2013) values ('Smith',2010),('Jones',2010);
insert into census_partitioned_example partition (year=2010) values ('Smith',2010),('Jones',2010);
insert into census_partitioned_example partition (year=2011) values ('Smith',2020),('Jones',2020),('Doe',2020);
insert into census_partitioned_example partition (year=2012) values ('Smith',2020),('Doe',2020);
```

### Query your table
```
select * from census_partitioned_example
```

### Querying a partition
```
select * from census_partitioned_example
where year = 2013 
```

### Creating a new partition using PARQUET FORMAT
```
alter table census_partitioned_example add partition (year=2014);
alter table census_partitioned_example partition (year=2014) set fileformat parquet;
```

### Creating a new partition using AVRO FORMAT
```
alter table census_partitioned_example add partition (year=2015);
alter table census_partitioned_example partition (year=2015) set fileformat avro;
```

### Check if the partitiones were created (TEXT, PARQUET and AVRO FORMAT)
```
show table stats census_partitioned_example
```

### Inserting data to the table
```
insert into census_partitioned_example partition (year=2014) values ('Caio PARQUET',2014),('Jones',2010);
insert into census_partitioned_example partition (year=2014) values ('Caio PARQUET 2',2014),('Jones',2010);
```

### Querying the year=2014 partition
```
select * from census_partitioned_example
where year = 2014
```

### Partitions inside HDFS

![Partitions inside HDFS](https://github.com/caiomsouza/pentaho-big-data-guides-and-sample-code/blob/master/guides/images/physical_files_for_partitions_impala_internal_table.PNG)



### LOAD DATA Statement
```
LOAD DATA INPATH 'hdfs_file_or_directory_path' [OVERWRITE] INTO TABLE tablename
  [PARTITION (partcol1=val1, partcol2=val2 ...)]
```

Source:<BR>
https://www.cloudera.com/documentation/enterprise/5-4-x/topics/impala_load_data.html<BR>

### Sample Code - Impala Internal Table with Partitions

Job:
*	Create a partition table on Impala;
*	Create the partitions;
*	Insert test data inside different partitions;


![Main Job](https://github.com/caiomsouza/pentaho-big-data-guides-and-sample-code/blob/master/sample-code/impala_internal_table_with_partitions/images/Main_Job_Impala_Internal_Table_With_Partitions.PNG)

More details: <BR>
https://github.com/caiomsouza/pentaho-big-data-guides-and-sample-code/blob/master/sample-code/impala_internal_table_with_partitions/README.md




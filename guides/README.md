# Guides

Pentaho Big Data Guides

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



## PARTITION TABLES
Before you continue please read the documentations below:

* Tuning Impala for Performance
[Tuning Impala for Performance](https://www.cloudera.com/documentation/enterprise/5-8-x/topics/impala_performance.html)
https://www.cloudera.com/documentation/enterprise/5-8-x/topics/impala_performance.html
* Impala Frequently Asked Questions
https://www.cloudera.com/documentation/enterprise/5-6-x/topics/impala_faq.html
* Partitioning for Impala Tables
https://www.cloudera.com/documentation/enterprise/5-8-x/topics/impala_partitioning.html#partitioning


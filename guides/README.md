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

### Select 
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



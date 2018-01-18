# Guides - Not Finished / DO NOT USE IT

Impala does not support Index.<BR>
Hive support indexes.<BR>

### Show Indexes 
```
show indexes on test_table;
```

### Create Index
```
create index index01 on table census (year) as 'COMPACT' with deferred rebuild;
```

### Links about Creating Indexes with Hive
https://acadgild.com/blog/indexing-in-hive/
https://www.slideshare.net/BenjaminLeonhardi/hive-loading-data
http://maheshwaranm.blogspot.com.es/2013/09/hive-indexing.html


### Tuning Impala for Performance
https://www.cloudera.com/documentation/enterprise/5-8-x/topics/impala_performance.html

### Impala Frequently Asked Questions
https://www.cloudera.com/documentation/enterprise/5-6-x/topics/impala_faq.html

### Partitioning for Impala Tables
https://www.cloudera.com/documentation/enterprise/5-8-x/topics/impala_partitioning.html#partitioning



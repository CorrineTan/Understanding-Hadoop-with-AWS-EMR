## Basics

#### HDFS Basic Concepts
- HDFS - distributed, fault tolerant, high throughput file system.
- Hardware failure is a norm not an exception (on average, harddrive last for 3 years - 1/1000).
- Streaming data access rather than interactive access by user.
- Large Data Sets (hundred of terabytes and perabytes).
- Simple Coherency Model - write-once-read-many access model, and read is always sequencial.
- Moving computation is cheaper than moving data.

#### HDFS Basic Commands
$ hdfs dfs -ls / 

![hdfs_ls_command](https://github.com/CorrineTan/Understanding-Hadoop-with-AWS-EMR/blob/main/Image/hdfs_ls_command.png)

$ touch test.txt
$ hdfs dfs -mkdir /user/dataset
$ hdfs dfs -cp file://'pwd'/test.txt /user/dataset
$ hdfs dfs -ls /user/dataset

$ hdfs --loglevel DEBUG dfs -ls /user/dataset/text.txt

## HDFS Basics


## HDFS Component - NameNode
## HDFS Component - DataNode
## Checkpointing
## HDFS Journal
## HDFS replication
## Read/write path in HDFS


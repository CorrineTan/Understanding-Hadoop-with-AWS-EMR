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

Usage: hdfs dfs -ls <args> <br/>
For a file returns stat on the file with the following format: <br/>
```permissions number_of_replicas userid groupid filesize modification_date modification_time filename ```

For a directory it returns list of its direct children as in Unix. A directory is listed as: <br/>
```permissions userid groupid modification_date modification_time dirname ```

![hdfs_ls_command](https://github.com/CorrineTan/Understanding-Hadoop-with-AWS-EMR/blob/main/Image/hdfs_ls_command.png)

![hdfs_list_details](https://github.com/CorrineTan/Understanding-Hadoop-with-AWS-EMR/blob/main/Image/hdfs_list_details.png)

$ touch test.txt<br/>
$ hdfs dfs -mkdir /user/dataset<br/>
$ hdfs dfs -cp file://'pwd'/test.txt /user/dataset<br/>
$ hdfs dfs -ls /user/dataset<br/>
$ hdfs --loglevel DEBUG dfs -ls /user/dataset/text.txt<br/>

## HDFS Basics


## HDFS Component - NameNode
## HDFS Component - DataNode
## Checkpointing
## HDFS Journal
## HDFS replication
## Read/write path in HDFS


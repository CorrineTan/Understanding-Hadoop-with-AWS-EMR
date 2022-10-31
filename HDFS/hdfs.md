## Basics

#### HDFS Basic Concepts
- HDFS - distributed, fault tolerant, high throughput file system.
- Hardware failure is a norm not an exception (on average, harddrive last for 3 years - 1/1000).
- Streaming data access rather than interactive access by user.
- Large Data Sets (hundred of terabytes and perabytes).
- Simple Coherency Model - write-once-read-many access model, and read is always sequencial.
- Moving computation is cheaper than moving data.

#### HDFS Basic Commands
$ hdfs dfs -ls / <br/>
Usage: 
For a file returns stat on the file with the following format: <br/>
```permissions number_of_replicas userid groupid filesize modification_date modification_time filename ```<br/>
For a directory it returns list of its direct children as in Unix. A directory is listed as: <br/>
```permissions userid groupid modification_date modification_time dirname ```

![hdfs_ls_command](https://github.com/CorrineTan/Understanding-Hadoop-with-AWS-EMR/blob/main/Image/hdfs_ls_command.png)

![hdfs_list_details](https://github.com/CorrineTan/Understanding-Hadoop-with-AWS-EMR/blob/main/Image/hdfs_list_details.png)

$ touch test.txt<br/>
$ hdfs dfs -mkdir /user/dataset<br/>
$ hdfs dfs -cp file://`pwd`/test.txt /user/dataset<br/>
$ hdfs dfs -ls /user/dataset<br/>
$ hdfs --loglevel DEBUG dfs -ls /user/dataset/text.txt<br/>
![hdfs_cp_debug](https://github.com/CorrineTan/Understanding-Hadoop-with-AWS-EMR/blob/main/Image/hdfs_cp_debug.png)

$ hdfs dfs -df -h <br/>
Usage: shows the capacity, free, and used space of the filesystem<br/>
<img src="https://github.com/CorrineTan/Understanding-Hadoop-with-AWS-EMR/blob/main/Image/hdfs_df_h.png">

$ hdfs dfsadmin -report <br/>
Usage: outputs a brief report on the overall HDFS filesystem. It’s a useful command to quickly view how much disk is available, how many DataNodes are running, corrupted blocks etc. <br/>
<img src="https://github.com/CorrineTan/Understanding-Hadoop-with-AWS-EMR/blob/main/Image/hdfs_dfsadmin_report.png">

Or use HDFS NameNode Web UI to understand more details.

## HDFS Component - NameNode
It is the primary and it stores the namespace and block metadata to data node mappings.

#### HDFS Metadata
List of files, list of blocks of each file, list of DataNode for each block, File attributes(access time, replication factor). <br/>
Metadata in Memory (no need for I/O). No demand paging of FS metadata.
<img src="https://github.com/CorrineTan/Understanding-Hadoop-with-AWS-EMR/blob/main/Image/hdfs_architecture.png">

## HDFS Component - DataNode
It actually stores the data in blocks.

## Checkpointing
## HDFS Journal
## HDFS replication
## Read/write path in HDFS


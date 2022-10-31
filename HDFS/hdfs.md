## HDFS Basics

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

Ohter Keypoints:
1. Only the file system metadata is persisted to disk<br/>
2. The mapping between the Blocks and DataNodes is never persisted to Disk<br/>
3. Everytime a DataNodes starts, it advertised the blocks it has<br/>
For example, if we have a DataNode with a 1TB data disk it will advertise: <br/>
```hdfs Block Size = 64MB      1TB/64MB = 16384 blocks```

## HDFS Component - DataNode
It actually stores the data in blocks.

File Splits - blocks
<img src="https://github.com/CorrineTan/Understanding-Hadoop-with-AWS-EMR/blob/main/Image/hdfs_blocks.png">

## Checkpointing and HDFS Journal
NameNode keeps the entries namespace image in RAM<br/>
NameNode record changes to HDFS in a write-ahead log called the journal in its local native file system (EditLogs) <br/>
The location of block replicas are not part of the persistent checkpoint<br/>
The two fiels which maintain this metadata on Disk: FSImage, EditLogs(Journal)<br/>
The inodes and the list of blocks that define the metadata of the name system are called "image"<br/>
The FSImage is never changed by NameNode.

NameNode Metadata layout: note that the "seen_txid" keeps the last transaction id of the last checkpoint:<br/>
<img src="https://github.com/CorrineTan/Understanding-Hadoop-with-AWS-EMR/blob/main/Image/hdfs_nn_metadata.png">

## HDFS replication
Each block replica on DataNode has two files in local native file system: <br/>
1st file: contains data itself <br/>
2nd file: records the block's metadata<br/>
HAR files - Hadoop-Archive is to mitigate the small file problem

Replication: default: 1 - < 4 nodes, 2 - < 10 nodes, 3 for other cases: <br/>
<img src="https://github.com/CorrineTan/Understanding-Hadoop-with-AWS-EMR/blob/main/Image/hdfs_blocks_replication.png">

Changing dfs.replicaiton doesn't gonna change the files already in hdfs. Change replication at file level: "-Ddfs.replication". To change replication of an exsiting file: use "setrep" command

## Read/write path in HDFS
#### HDFS Write 
Sequence of Communication<br/>
<img src="https://github.com/CorrineTan/Understanding-Hadoop-with-AWS-EMR/blob/main/Image/hdfs_write.png">

<img src="https://github.com/CorrineTan/Understanding-Hadoop-with-AWS-EMR/blob/main/Image/hdfs_write_dn.png">

Write Process:<br/>
 - HDFS client write data into cache, when it reaches HDFS block size, the HDFS Client contacts the NameNode
 - NameNode inserts the filenames into its hierarchy, allocate a DataNode for the block and replicas
 - DataNode form a pipeline in the order to minimize the total network distance 
 - Client put data from its memory to DataNodes, data is sent in 4KB packets. Each data packet is acknowledged, but client can continue to send data without waiitng for ACKs
 - After finish pushing all pakcets to DataNodes and obtained ACKs for all packets, the client tells the NameNode that block is written and closed
 - NameNode commits the file creation and log in the journal. If the NameNode dies before the file is close, the file is lost

#### HDFS Read
Sequence of Communication<br/>
<img src="https://github.com/CorrineTan/Understanding-Hadoop-with-AWS-EMR/blob/main/Image/hdfs_read.png">

Read Process:<br/>
 - Client program requests for data from the NameNode using filename
 - NameNode returns the block location on the DataNodes
 - Client then accesses each block individually
 - Can have parallel access if on different nodes
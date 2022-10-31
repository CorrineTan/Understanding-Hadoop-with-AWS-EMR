## Basic functionality of YARN
YARN - Yet Another Resource Negotiator<br/>
Provides a unified framework that many engines access the resource of the cluster
<img src="https://github.com/CorrineTan/Understanding-Hadoop-with-AWS-EMR/blob/main/Image/yarn_overall.png">
YARN features:
- Multi-tenancy<br/>
YARN allows multiple access engines to use Hadoop as the command standard for batch, interactive, real-time engines that can simulataneously access the same dataset.<br/>
- Scalability<br/>
ResourceManager focuses exclusively on scheduling, keeping pace as cluster expand to thousands of nodes managing petabytes of data.<br/>
- Comaptibility<br/>
Hadoop1,2,3: exisiting mapreduce application can run on different version of hadoops, without disruption to exsiting process that already work. <br/>
- Cluster Utilization <br/>
YARN's dynamic allocation of cluster resources improves utlization over more static MapReduce rules used in early version of Hadoop.

Run example MapReduce job:<br/>
$ yarn jar /usr/lib/hadoop-mapreduce/hadoop-mapreduce-examples.jar pi 5 10
<img src="https://github.com/CorrineTan/Understanding-Hadoop-with-AWS-EMR/blob/main/Image/example_mapreduce_job.png">
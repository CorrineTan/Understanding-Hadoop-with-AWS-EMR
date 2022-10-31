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

### Applications
Notice the applicaiton id "application_1667175198378_0001" here:<br/>
The middle number is "Epoch time of when the ResourceManager was started"<br/>
The lastly number is the "Applicaiton sequence number which increases"<br/>
If we restart ResourceManager, we will get a different epoch time and sequesce starts from "0001" again.

#### Application commands:
List application the finished applicaiton: <br/>
```$ yarn application -list -appStates ALL 2>&1 | sed "s/\ \+/\ /g"```

<img src="https://github.com/CorrineTan/Understanding-Hadoop-with-AWS-EMR/blob/main/Image/mapreduce_list_application.png">

List the running application: <br/>
```$ yarn application -list"```

Kill the running application: <br/>
```$ yarn application -kill application_1667175198378_0001```

List Nodes and Resrouces: <br/>
```$ yarn node -list``` <br/>
```$ yarn node -list -showDetails``` 

<img src="https://github.com/CorrineTan/Understanding-Hadoop-with-AWS-EMR/blob/main/Image/yarn_node_list.png">

Viewing the YARN Application logs: <br/>
```$ yarn logs -applicationID application_1667175198378_0001``` <br/>

#### Resource Manager WebUI
Yarn Web UI: http://hadooptutorial.info/yarn-web-ui/ <br/>
It's running on port 8088 on the EMR primary node.

Main components in YARN: ResourceManager and NodeManager


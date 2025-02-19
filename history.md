[JStorm 0.9.0 ����](http://wenku.baidu.com/view/59e81017dd36a32d7375818b.html)

#Release 0.9.3.1

## Enhancement
1. switch apache thrift7 to storm thrift7
2. set defatult acker number is 1
3. add "spout.single.thread" setting

#Release 0.9.3
## New feature
1. Support Aliyun Apsara/Hadoop Yarn

## Enhancement
1. Redesign Logview
2. Kill old worker under the same port when worker is starting
3. Add zk information/version information on UI
4. Add nodeport information for dead task in nimbus
5. Add interface to get values when spout doing ack
6. Add timeout statics in bolt
7. jstorm script return status
8. Add logs when fail to deserialize tuple 
9. Skip sleep operation when max_pending is 1 and waiting ack
10. Remove useless dependency
11. Longer task timeout setting
12. Add supervisor.use.ip setting
13. Redirect supervisor out/err to /dev/null, redirect worker out/err to one file


## Bug Fix
1. Fix kryo fail to deserialize object when enable classloader
2. Fix fail to reassign dead task when worker number is less than topology apply
3. Set samller jvm heap memory for jstorm-client 
4. Fix fail to set topology status as active when  do rebalance operation twice at one time,
5. Fix local mode bug under linux
6. Fix average latency isn't accurate
7. GC tuning.
8. Add default kill function for AysncLoopRunnable
 


#Release 0.9.2
## New feature
1. Support LocalCluster/LocalDrpc mode, support debugging topology under local mode
2. Support CGroups, assigning CPU in hardware level.
3. Support simple logview

## Bug fix or enhancement
1. Change SpoutExecutor's RotatingMap to TimeCacheMap, when putting too much timeout tuple is easy to cause deadlock in spout acker thread
2. Tunning gc parameter, improve performance and avoid full GC
3. Improve Topology's own gc priority, make it higher than JStorm system setting.
4. Tuning Nimbus HA, switch nimbus faster, when occur nimbus failure.
5. Fix bugs found by FindBugs tool.
6. Revert Trident interface to 0.8.1, due to 0.8.1's trident interface's performance is better.
7. Setting nimbus.task.timeout.secs as 60 to avoid nimbus doing assignment when task is under full gc.
8. Setting default rpc framework as netty
9. Tunning nimbus shutdown flow
10. Tunning worker shutdown flow
11. Add task heartbeat log
12. Optimize Drpc/LocalDrpc source code.
13. Move classloader to client jar.
14  Fix classloader fail to load  anonymous class
15. Web Ui display slave nimbus
16. Add thrift max read buffer size
17. Setting CPU slot base double
18. Move Zk utility to jstorm-client-extension.jar
19. Fix localOrShuffle null pointer
20. Redirecting worker's System.out/System.err to file is configurable.
21. Add new RPC frameworker JeroMq
22. Fix Zk watcher miss problem
23. Update sl4j 1.5.6 to 1.7.5
24. Shutdown worker when occur exception in Smart thread
25. Skip downloading useless topology in Supervisor
26. Redownload the topology when failed to deserialize topology in Supervisor.
27. Fix topology codeDir as resourceDir
28. Catch error when normalize topology
29. Add log when found one task is dead
30. Add maven repository, JStorm is able to build outside of Alibaba
31. Fix localOrShuffle null pointer exception
32. Add statics counting for internal tuples in one worker
33. Add thrift.close after download topology binary in Supervisor


# Release 0.9.1

## new features
1. Application classloader. when Application jar is conflict with jstorm jar, 
   please enable application classloader.
2. Group Quato, Different group with different resource quato.

## Bug fix or enhancement
1. Fix Rotation Map competition issue.
2. Set default acker number as 0
3. Set default spout/bolt number as 1
4. Add log directory in log4j configuration file
5. Add transaction example
6. Fix UI showing wrong worker numbe in topology page
7. Fix UI showing wrong latency in topology page
8. Replace hardcode Integer convert with JStormUtils.parseInt
9. Support string parse in Utils.getInt
10. Remove useless dependency in pom.xml
11. Support supervisor using IP or special hostname
12. Add more details when no resource has been assigned to one new topology
13. Replace normal thread with Smart thread
14. Add gc details 
15. Code format
16. Unify stormId and topologyId as topologyId
17. Every nimbus will regist ip to ZK



# Release 0.9.0
In this version, it will follow storm 0.9.0 interface, so the application running
on storm 0.9.0 can run in jstorm 0.9.0 without any change.

## Stability
1. provide nimbus HA. when the master nimbus shuts down, it will select another
 online nimbus to be the master. There is only one master nimbus online 
 any time and the slave nimbuses just synchronouse the master's data.
2. RPC through netty is stable, the sending speed is match with receiving speed. 


## Powerful scheduler
1. Assigning resource on four dimensions:cpu, mem, disk, net
2. Application can use old assignment.
3. Application can use user-define resource.
4. Task can apply extra cpu slot or memory slot.
4. Application can force tasks run on different supervisor or the same supervisor








# Release 0.7.1
In this version, it will follow storm 0.7.1 interface, so the topology running
in storm 0.7.1 can run in jstorm without any change.

## Stability
* Assign workers in balance
* add setting "zmq.max.queue.msg" for zeromq
* communication between worker and tasks without zeromq
* Add catch exception operation
  * in supervisor SyncProcess/SyncSupervisor
  * add catch exception and report_error in spout's open and bolt's prepare
  * in all IO operation
  * in all serialize/deserialize
  * in all ZK operation
  *  in topology upload/download function
  *  during initialization zeromq
* do assignmen/reassignment operation in one thread to avoid competition
* redesign nimbus 's topology assign algorithm, make the logic simple much.
* redesign supervisor's sync assignment algorithm, make the logic simple much
* reduce zookeeper load
  * redesign nimbus monitor logic, it will just scan tasks' hearbeat, frequency is 10s
  * nimbus cancel watch on supervisor
  * supervisor heartbeat frequence change to 10s
  * supervisor syncSupervisor/syncProcess frequence change to 10s
  * supervisor scan /$(ZKROOT)/assignment only once in one monitor loop
  * task hearbeat change to 10s
* create task pid file before connection zk, this is very import when zk is unstable.


## Performance tuning
* reduce once memory copy when deserialize tuple, improve performance huge.
* split executor thread as two thread, one handing receive tuples, one sending tuples, improve performance much
* redeisign sample code, it will sampling every 5 seconds, not every 20 tuple once, improve performance much
* simplify the ack's logic, make acker more effeciency
* Communication between worker and tasks won't use zeromq, just memory share in process
* in worker's Drainer/virtualportdispatch thread, spout/bolt recv/send thread, 
   the thread will sleep 1 ms when there is not tuple in one loop
* communication between worker and tasks without zeromq
* sampling frequence change to 5s, not every 20 tuple once.

## Enhancement:
* add IFailValueSpout interface
* Redesign sampling code, collection statics model become more common.
  *  Add sending/recving tps statics, statics is more precise.
* Atomatically do deactivate action when kill/rebalance topology, and the wait time is 2 * MSG_TIMEOUT
* fix nongrouping bug, random.nextInt will generate value less than 0.
* Sleep one setting time(default is 1 minute) after finish spout open, 
   which is used to wait other task finish initialization.
* Add check component name when submit topology, forbidding the component 
   which name start with "__"
* change the zk's node /$(ZKROOT)/storm to /$(ZKROOT)/topology
* abstract topology check logic from generating real topology function
* when supervisor is down and topology do rebalance, the alive task under down 
   supervisor is unavailable.
* add close connection operation after finish download topology binary
* automatically create all local dirtorie, such as 
   /$(LOCALDIR)/supervisor/localstate
* when killing worker, add "kill and sleep " operation before "kill -9" operation
* when generate real topology binary,
  * configuration priority different.   
      component configuration > topology configuration > system configuration
  * skip the output stream which target component doesn't exist.
  * skip the component whose parallism is 0.
  * component's parallism is less than 0, throw exception.
* skip ack/fail when inputstream setting is empty
* add topology name to the log
* fix ui select option error, default is 10 minutes
* supervisor can display all worker's status
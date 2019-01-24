# oozie-scheduler-workflow

Oozie combines multiple jobs sequentially into one logical unit of work. It is integrated with the Hadoop ecosystems, with MR1, YARN as its architectural center, and supports Hadoop jobs for  MapReduce, Spark, Pig, Hive, and Sqoop. Oozie can also schedule jobs specific to a system, like Java programs, python  or shell scripts.
And Oozie is like Cron of unix world as well. It helps to schedule the jobs and tracking the status.


1.Install/configure the hdfs. let's create the single node cluster to test the setup.

2.Install/configure oozie and replace the default derby database with mysql or postgresql. here we use mysql.

3.Integrates the oozie library packages to hdfs and get the appropriate permission/ownership in place. Otherwise, it will be difficult to troubleshoot the jobs when it fails.


4.Run mapreduce jobs through Oozie. job/workflow details are in folder mapreduce-jobs.


$oozie job -oozie http://hostname:11000/oozie -config examples/apps/map-reduce/job.properties -run 
job: 0000014-190123083458183-oozie-oozi-W

$oozie job -oozie http://localhost:11000/oozie -info 0000014-190123083458183-oozie-oozi-W
Job ID : 0000014-190123083458183-oozie-oozi-W
Workflow Name : map-reduce-wf
App Path      : hdfs://hostname/user/oozie/wc1
Status        : SUCCEEDED
Run           : 0
User          : hadoop
Group         : -
Created       : 2019-01-23 00:31 GMT
Started       : 2019-01-23 00:31 GMT
Last Modified : 2014-01-23 00:32 GMT
Ended         : 2014-01-23 00:32 GMT
CoordAction ID: -

Actions
ID                                                                            Status    Ext ID                 Ext Status Err Code
0000014-190123083458183-oozie-oozi-W@:start:                                  OK        -                      OK         -
0000014-190123083458183-oozie-oozi-W@mr-node                                  OK        job_1401405229971_0022 SUCCEEDED  -
0000014-190123083458183-oozie-oozi-W@end                                      OK        -                      OK

$



5.Run spark jobs using Oozie workflow.  Details are in the folder spark-jobs. follow the same steps as what follwed at running mapreduce job previously. 

6.Scheduling Oozie jobs. Once the jobs are functioning fine, we can use the cron like scheduling jobs through "frequency" parameter. Oozie offers a cron like syntax structure to schedule/run the jon at particulat time or in regular interval.

<coordinator-app name="MY_APP" frequency="30 14 * *
        *" start="20019-01-23T05:00Z" end="2019-01-23T06:00Z" timezone="UTC" xmlns="uri:oozie:coordinator:0.5">
   <action>
      <workflow>
         <app-path>hdfs://hostname:8020/user/oozie</app-path>
      </workflow>
   </action>
</coordinator-app>


---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
starred: false
keywords: []
description: How to setup Hortonworks cluster using EC2
datePublished: '2016-01-15T20:45:34.017Z'
dateModified: '2016-01-15T20:45:19.223Z'
title: 'Hortonworks Data Platform on AWS EC2 - yet another instruction!'
author: []
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
sourcePath: _posts/2016-01-15-hortonworks-data-platform-on-aws-ec2-yet-another-instructi.md
published: true
url: hortonworks-data-platform-on-aws-ec2-yet-another-instructi/index.html
_type: Article

---
# Hortonworks Data Platform on AWS EC2 - yet another instruction!

From time to time I have a possibility to run training about Hadoop ecosystem.
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/dcc48202-cbb8-40ea-921f-89f0d18ed7ee.jpg)

## Sandbox is great? 

Main requirement is good linux/mac laptop to run: 

* MRUnit tests 
* Map Reduce in local mode 
* PigUnit

For windows machines it's more difficult as it requires Hadoop binaries.
But most important is that it should run smoothly Hortonworks Sandbox machine which quite often updated and requires more and more resources. At this moment (2015) 6GB RAM is a minimum and 8GB is recommended to run only VM and still it will work slow. 

On trainings students are using Eclipse/Idea and use browsers and it looks like that 16GB laptop should be used instead of 8GB. It is a problem for students because most of the people don't have brand new hardware with those parameters and can't attend courses. 

For training and private use I have created using Amazon EC2 very small cluster consisting of two machines. 

Below there is short instruction how to install HDP 2.2.4.2 with Ambari 1.7\. 

## Reference
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/92157037-9e67-4105-8972-e5a3b39fc12f.png)

This instruction is a mix of many instructions, but most of the credit goes to two below: 

* http://hortonworks.com/blog/deploying-hadoop-cluster-amazon-ec2-hortonworks/ 
* http://hortonworks.com/kb/ambari-on-ec2/ 

EC2 setup
Cluster consits of two machines: 

* m3.xlarge hdpmaster1 
* m3.xlarge hdpmaster2 

I have created first instance based on Centos 6 with 100GB SSD.
During the instance creation process there is possibility to create security group - below there is short table with some port exceptions. If anything is missing, it's easy to fix it later: 

ICMP:

* open all ALL 

TCP rules: 

* 0 -- 65535 your subnet 
* 22 (SSH) 0.0.0.0/0 
* 7180 0.0.0.0/0 
* 8080 - 8100 0.0.0.0/0 
* 50000 -- 50100 0.0.0.0/0 

UDP rules: 

* 0 -- 65535 your-subnet 

I've downloaded the training-cluster.pem and saved it securely (.ssh directory is great for that). 

Now I can login using this key: 
> 
> ssh -i .ssh/training-cluster.pem root@52.6.33.48 

There are packages to install and services turn off: 

* vi /etc/sysconfig/selinux (set SELINUX=disabled) 
* yum -y install ntp 
* chkconfig ntpd on 
* chkconfig iptables off 
* chkconfig ip6tables off 
* /etc/init.d/iptables stop 2 /dev/null /dev/null 

We can save our image (without reboot) as our private AMI and run second instance (hdpmaster2).

To succeed with keyless login from Ambari (running on hdpmaster1) to second node we have to upload our key. 

* scp -i .ssh/training-cluster.pem .ssh/training-cluster.pem root@ip-of-hdpmaster1: 
* ssh -i .ssh/training-cluster.pem root@ip-of-hdpmaster1 
* mv training-cluster.pem .ssh/id\_rsa 

To make it more easy I modify etc/hosts: 

* 127.0.0.1 localhost localhost.localdomain localhost4 localhost4.localdomain4 
* ::1 localhost localhost.localdomain localhost6 localhost6.localdomain6
* \[private ip of hdpmaster1\] hdpmaster1
* \[private ip of hdpmaster2\] hdpmaster2

I distribute hosts file to second node. Notice it doesn't ask for key or password:

> scp /etc/hosts root@hdpmaster2:/etc/hosts

At this moment I have all machines ready to install HDP.

## Ambari setup

I setup ambari repo, install it and start everything with default values. 

* yum install wget 
* wget http://public-repo-1.hortonworks.com/ambari/centos6/1.x/updates/1.7.0/ambari.repo 
* cp ambari.repo /etc/yum.repos.d 
* yum install epel-release 
* yum repolist 
* yum install ambari-server 
* ambari-server setup 
* ambari-server start 

Right now I can login (admin/admin) to hdpmaster1-public-ip:8080\. I change password! 

## Cluster setup

I choose HDP 2.2 stack, paste hostnames of my huge cluster: 

* hdpmaster1 
* hdpmaster2 

Initial setup finishes quite quickly. I install all the services I want, assign DataNode and NodeManager to both machines and again.
There are some issues that I will fix later.

## User creation

I add sample user - it's good snippet to add more users: 

* groupadd hadoopusers 
* useradd -g hadoopusers app\_user 
* passwd app\_user 
* sudo -u hdfs hdfs dfs -mkdir /user/app\_user/ 
* sudo -u hdfs hdfs dfs -chown -R app\_user:hadoopusers /user/app\_user/ 

## Smoketests

I test all the functionalities - Hive, Tez, MapReduce, HBase and fix problems. 

## Problems

1) During mapreduce I get this Error : 
> 
> File does not exist: hdfs://...../hdp/apps/2.2.4.2--2/mapreduce/mapreduce.tar.gz 

From hdpmaster1 do: 

* sudo -u hdfs hdfs dfs -mkdir -p /hdp/apps/2.2.4.2-2/mapreduce 
* sudo -u hdfs hdfs dfs -put /usr/hdp/current/hadoop-client/mapreduce.tar.gz 

2) During mapreduce we get this Error : 
> 
> File does not exist: hdfs://...../hdp/apps/2.2.4.2--2/tez/tez.tar.gz 

From hdpmaster1 do: 

* sudo -u hdfs hdfs dfs -mkdir -p /hdp/apps/2.2.4.2-2/tez 
* sudo -u hdfs hdfs dfs -put /usr/hdp/2.2.4.2-2/tez/lib/tez.tar.gz /hdp/apps/2.2.4.2-2/tez/ 

3) Despite assigning 100GB SSD to each machine, Centos reports 8GB disk size. [Here][0] is great fix for that - I just follow instructions. 

## Previus day struggles 

On previous day I tried to install cluster on RHL7 - it doesn't work at this moment, some issues worth to note: 

* Ambari-agent has some issues with rpms: https://issues.apache.org/jira/browse/AMBARI--10201 which of course I've encountered :) There is fix for that but is not yet released 
* "Cannot register host with not supported os type, hostname={host private DNS}, serverOsType=redhat7, agentOsType=redhat7" http://hortonworks.com/community/forums/topic/failure-registering-hosts/ 
* Ambari requires root access at default. To enable it on RHL there is nice instruction: http://stackoverflow.com/a/18047873/4368212 

## Cludbreak
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/5373ffca-a191-475e-b032-2bbaed8fa36b.png)

One could ask - why not to use SequenceIQ Cloudbreak - I tried it yesterday - it creates cluster very nicely. But when I logged in I didn't know how to use it - how to run Hive, run Pig script, HBase or MapReduce application. I need to read some articles about and return to it, because it automates everything I have written above. 

## Costs

This setup isn't expensive as long as I remember to shutdown cluster after use. Amazon Calculator estimates it should costs 18$ for storage and 50 cents per hour for using cluster.
I hope this instruction will help someone.
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/3e2a1ecd-b66e-47ef-bd28-3b1a1654dfc4.jpg)

[0]: http://stackoverflow.com/a/24030938/4368212
---
layout: post
title: gentoo hadoop
---
It is very easy to install Hadoop into Funtoo/Gentoo, as I've added hadoop ebuild to my [overlay](https://github.com/longlene/clx).

1. add overlay
Add the overlay to portage:
```shell
layman -f -a clx -o https://raw.github.com/longlene/clx/master/repo.xml
```

2. installation
emerge the **hadoop-bin**:
```shell
emerge -v hadoop-bin
```

3. start
As the ebuild has setting the hadoop, I can start hadoop at once.

At first, format the HDFS:
```shell
/etc/init.d/hadoop-namenode format
```

Then, start the hadoop:
```shell
/etc/init.d/hadoop-namenode start
/etc/init.d/hadoop-datanode start
/etc/init.d/hadoop-secondarynamenode start
/etc/init.d/yarn-resourcemanager start
/etc/init.d/yarn-nodemanager start
```

4. usage
setup user directory:
```shell
hdfs dfs -mkdir /user
hdfs dfs -mkdir /user/$(whoami)
```

upload data file:
```shell
hdfs dfs -put /etc/hadoop input
```

execute the task:
```shell
hadoop jar /opt/hadoop-3.1.0/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.1.0.jar grep input output 'dfs[a-z.]+'
```

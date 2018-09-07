---
layout: post
title: "Notes on Seven Databases in Seven Weeks"
date: 2014-07-01 00:22:17 +0800
comments: true
categories: 
---
## 《七周七数据库》笔记
***
- Postgres RDBMS  
- Riak 键值存储  
- Redis 键值存储  
- Hbase 列存储  
- Mongodb 文档型数据库  
- CouchDB 文档型数据库  
- neo4j 图数据库  


RDBMS:以集合论为基础，实现为具有行和列的二维表。MySQL, H2，SQLite  
K-V:对资源要求比较少，Memcached，memcachedb，Membase  
面向列的数据库：一个给定列的数据放在一起，HBase，Cassandra，Hypertable  
文档型数据库：存储文档，  
图数据库：擅长处理高度互联的数据，存储关系，常用于社交网络应用

***

+ Postgres：RDBMS  
+ Riak：支持HTTP和REST Web方式，根据亚马逊Dynamo实现，值可以是任何内容，通过mapreduce支持高级查询  
+ Redis：写入磁盘前先写入内存缓存，适合用于缓存非关键数据  
+ HBase：以Google的BigTable论文为蓝本，建立在hadoop引擎之上，  
+ MongoDB：支持巨大的数据，使用javascript作为查询语言  
+ CouchDB：用Erlang编写，具有高可用性，使用javascript作为查询语言  
+ Neo4j：图数据库

### Postgres：  
内置支持Unicode  
可处理TB级数据  
用户：Skype，法国储蓄银行（CNAF），美国联邦航空局（FAA）  
CRUD create, read, udpate, delete  
$ createdb xxx // 创建数据库  
数据类型：  
text任意长度  
varchar(n) 长度可达n字节的字符串  
char(n) 长度正好是n的字符串  
存储过程在服务端执行  
字符串匹配：LIKE和ILIKE，正则表达式，相似比较算法levenshtein，三连词，全文检索  

优点：结构化良好的数据，自定义语言扩展，自定义索引，自定义数据类型，开源协议友好，BSD协议  
缺点：不擅长水平扩展，不适合数据要求过于灵活
### Riak
值可以是任何类型的数据，如普通文本、JSON、XML、图片、甚至视频片段；可通过http访问  
Riak的灵感来自于Amazon Dynamo的论文  
CRUD post, get, put, delete  
键值存储，可将键分类，放入bucket中  
支持mapreduce  
便于集群处理  
使用向量时钟解决冲突  

优点：大型订货系统，有高可用性，避免单点故障，便于横向扩展  
缺点：不够简单与健壮，mapreduce执行较慢  

### HBase
面向列的数据库  
可伸缩性好，用于在线分析处理系统，容错性好  
用户：Fackbook, Twitter, eBay, Meetup, Ning, Yahoo

优点：健壮的可伸缩架构，内置版本和压缩功能，社区友好  
缺点：不能缩小，最小配置5个节点，大集群胶南管理，一般不单独部署，而是和hadoop一起部署；除行键外，无任何索引功能；无数据类型

### MongoDB
文档型数据库，允许数据嵌套保存，  
用户：Foursquare，bit.ly，CERN  
JSON文档数据库，以BSON存储  
使用javascript查询  
支持B树索引  
节点宕机时，每个服务有张选票，拥有最新数据的服务将提升为主节点；服务器总是应该有单数个  
不允许有个多个主节点  
优点：能通过复制和横向伸缩，处理大量的数据，有非常灵活的数据模型  
缺点：没有任何模式，建立集群需要预先设计好  

### CouchDB
支持各种场景，面向文档，使用JSON作为存储和通信的语言，  
没有事务和锁的概念，使用_id和_rev来控制修改，  
所有的通信都基于REST的  
优点：可胜任各种部署场景，健壮且稳定  
缺点：不能对数据分片，  

### Neo4j
数据存为图，适合社交网络  
重点是数据间的关系，而非数据集合间的共性  
支持REST接口  
支持事务  
优点：支持大规模集群，  
缺点：无法从一个顶点指回自身  

### Redis
键值对存储库，读取写入速度快，  
支持事务  
支持强大的数据结构  
到期策略，数据持久化  
优点：速度快，  
缺点：驻留在内存，存在持久性问题  


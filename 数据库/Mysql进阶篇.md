# 1. 存储引擎



## 1.1 MySQL体系结构



![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660371977861-0f0c92c9-0613-4bf3-bf88-be6507d314b6.png)

### 1). 连接层



最上层是一些客户端和链接服务，包含本地sock 通信和大多数基于客户端/服务端工具实现的类似于TCP/IP的通信。主要完成一些类似于连接处理、授权认证、及相关的安全方案。在该层上引入了线程池的概念，为通过认证安全接入的客户端提供线程。同样在该层上可以实现基于SSL的安全链接。服务器也会为安全接入的每个客户端验证它所具有的操作权限。



### 2). 服务层



第二层架构主要完成大多数的核心服务功能，如SQL接口，并完成缓存的查询，SQL的分析和优化，部分内置函数的执行。所有跨存储引擎的功能也在这一层实现，如 过程、函数等。在该层，服务器会解析查询并创建相应的内部解析树，并对其完成相应的优化如确定表的查询的顺序，是否利用索引等，最后生成相应的执行操作。如果是select语句，服务器还会查询内部的缓存，如果缓存空间足够大，这样在解决大量读操作的环境中能够很好的提升系统的性能。



### 3). 引擎层 



存储引擎层， 存储引擎真正的负责了MySQL中数据的存储和提取，服务器通过API和存储引擎进行通信。不同的存储引擎具有不同的功能，这样我们可以根据自己的需要，来选取合适的存储引擎。数据库中的索引是在存储引擎层实现的。



### 4). 存储层



数据存储层， 主要是将数据(如:  redo log 、 undo log 、数据、索引、二进制日志、错误日志、查询日志、慢查询日志等)存储在文件系统之上，并完成与存储引擎的交互。



和其他数据库相比，MySQL有点与众不同，它的架构可以在多种不同场景中应用并发挥良好作用。主要体现在存储引擎上，插件式的存储引擎架构，将查询处理和其他的系统任务以及数据的存储提取分离。这种架构可以根据业务的需求和实际需要选择合适的存储引擎



## 1.2 存储引擎介绍



![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660372065481-18ddbac3-28df-4969-99c4-bc6b5207b11f.png)

大家可能没有听说过存储引擎，但是一定听过引擎这个词，引擎就是发动机，是一个机器的核心组件。 

比如，对于舰载机、直升机、火箭来说，他们都有各自的引擎，是他们最为核心的组件。而我们在选择引擎的时候，需要在合适的场景，选择合适的存储引擎，就像在直升机上，我们不能选择舰载机的引擎一样。

而对于存储引擎，也是一样，它是MySQL数据库的核心，我们也需要在合适的场景选择合适的存储引擎。接下来就来介绍一下存储引擎。

存储引擎就是存储数据、建立索引、更新/查询数据等技术的实现方式 。**存储引擎是基于表的**，而不是基于库的，所以存储引擎也可被称为表类型。我们可以在创建表的时候，来指定选择的存储引擎，如果没有指定将自动选择默认的存储引擎。

### 1). 建表时指定存储引擎

```sql
CREATE TABLE 表名( 
    字段1 字段1类型 [ COMMENT 字段1注释 ] , 
    ...... 
    字段n 字段n类型 [COMMENT 字段n注释 ] 
) ENGINE = INNODB [ COMMENT 表注释 ] ; 

//ENGINE 引擎
```



### 2). 查询当前数据库支持的存储引擎

```shell
show engines;
```



**示例演示:** 

A. 查询建表语句 --- 默认存储引擎: InnoDB

```sql
show create table account;
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660372166458-375b753f-b4d8-49e9-804f-0a45e71020ee.png)

我们可以看到，创建表时，即使我们没有指定存储引擎，MySQL数据库也会自动选择默认的存储引擎。



B. 查询当前数据库支持的存储引擎

```sql
show engines ; 
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652153585112-c4a8c599-a746-45c0-b8ae-3d120b798fd5.png)



C. 创建表 my_myisam , 并指定MyISAM存储引擎

```sql
create table my_myisam( 
    id int, 
    name varchar(10) 
) engine = MyISAM ; 
```



D. 创建表 my_memory , 指定Memory存储引擎

```sql
create table my_memory( 
    id int, 
    name varchar(10) 
) engine = Memory ; 
```



## 1.3 存储引擎特点



上面我们介绍了什么是存储引擎，以及如何在建表时如何指定存储引擎，接下来我们就来介绍下来上面 

重点提到的三种存储引擎 InnoDB、MyISAM、Memory的特点



### 1.3.1 InnoDB



#### 1). 介绍

InnoDB是一种兼顾高可靠性和高性能的通用存储引擎，在 MySQL 5.5 之后，InnoDB是默认的MySQL 存储引擎。



#### 2). 特点

- DML操作遵循ACID模型，支持事务；    DML（增删改）  ACID事务四大特征
- 行级锁，提高并发访问性能； 
- 支持外键FOREIGN KEY约束，保证数据的完整性和正确性；



#### 3). 文件

xxx.ibd：xxx代表的是表名，innoDB引擎的每张表都会对应这样一个表空间文件，ibd文件存储该表的表结构（xxx.frm-早期的 、xxx.sdi-新版的）、数据和索引信息。

参数：innodb_file_per_table

```sql
show variables like 'innodb_file_per_table';
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652153842046-501dee42-12d4-4870-9c25-11d4e13a4d08.png)

如果该参数开启，代表对于InnoDB引擎的表，每一张表都对应着一个ibd文件。 我们直接打开MySQL的 

数据存放目录： C:\ProgramData\MySQL\MySQL Server 8.0\Data ， 这个目录下有很多文件夹，不同的文件夹代表不同的数据库，我们直接打开itcast文件夹。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652154037625-db1d2078-1c2f-420b-9cb8-ab8add85c2d5.png)

可以看到里面有很多的ibd文件，每一个ibd文件就对应一张表，比如：我们有一张表 account，就有这样的一个account.ibd文件，而在这个ibd文件中不仅存放表结构、数据，还会存放该表对应的索引信息。 而该文件是基于二进制存储的，不能直接基于记事本打开，我们可以使用mysql提供的一个指令 ibd2sdi ，通过该指令就可以从ibd文件中提取sdi信息，而sdi数据字典信息中就包含该表的表结构。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652154178457-d32152b1-8f26-4481-8db7-6d9bc2fe2ee4.png)



#### 4). 逻辑存储结构

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660372256863-8121e30b-3422-4f4d-81e5-6acd0d4c910c.png)

- 表空间 : InnoDB存储引擎逻辑结构的最高层，.ibd文件其实就是表空间文件，在表空间中可以包含多个Segment段。 
- 段 : 表空间是由各个段组成的， 常见的段有数据段、索引段、回滚段等。InnoDB中对于段的管理，都是引擎自身完成，不需要人为对其控制，一个段中包含多个区。 
- 区 : 区是表空间的单元结构，每个区的大小为1M。 默认情况下， InnoDB存储引擎页大小为16K， 即一个区中一共有64个连续的页。 
- 页 : 页是组成区的最小单元，**页也是InnoDB 存储引擎磁盘管理的最小单元**，每个页的大小默认为 16KB。为了保证页的连续性，InnoDB 存储引擎每次从磁盘申请 4-5 个区。 
- 行 : InnoDB 存储引擎是面向行的，也就是说数据是按行进行存放的，在每一行中除了定义表时所指定的字段以外，还包含两个隐藏字段(后面会详细介绍)。 



### 1.3.2 MyISAM



#### 1). 介绍

MyISAM是MySQL早期的默认存储引擎。



#### 2). 特点

不支持事务，不支持外键 

支持表锁，不支持行锁 

访问速度快



#### 3). 文件

xxx.sdi：存储表结构信息 

xxx.MYD: 存储数据 

xxx.MYI: 存储索引

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652164763535-6974f0a4-f5f6-43cd-b545-52b90aafe8fe.png)



### 1.3.3 Memory



#### 1). 介绍

Memory引擎的表数据时存储在内存中的，由于受到硬件问题、或断电问题的影响，只能将这些表作为临时表或缓存使用。



#### 2). 特点

内存存放 

 hash索引（默认）



#### 3).文件 

xxx.sdi：存储表结构信息 



### 1.3.4 区别及特点



![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652165031303-495ec63c-ef69-4fee-a594-07165f93fc05.png)



### 面试题: 

InnoDB引擎与MyISAM引擎的区别 ? 

①. InnoDB引擎, 支持事务, 而MyISAM不支持。 

②. InnoDB引擎, 支持行锁和表锁, 而MyISAM仅支持表锁, 不支持行锁。 

③. InnoDB引擎, 支持外键, 而MyISAM是不支持的。

主要是上述三点区别，当然也可以从索引结构、存储限制等方面，更加深入的回答，具体参 

考如下官方文档：

**https://dev.mysql.com/doc/refman/8.0/en/innodb-introduction.html** 

**https://dev.mysql.com/doc/refman/8.0/en/myisam-storage-engine.html**



## 1.4 存储引擎选择



在选择存储引擎时，应该根据应用系统的特点选择合适的存储引擎。对于复杂的应用系统，还可以根据实际情况选择多种存储引擎进行组合。



- InnoDB: 是Mysql的默认存储引擎，支持事务、外键。如果应用对事务的完整性有比较高的要求，在并发条件下要求数据的一致性，数据操作除了插入和查询之外，还包含很多的更新、删除操作，那么InnoDB存储引擎是比较合适的选择。 
- MyISAM ： 如果应用是以读操作和插入操作为主，只有很少的更新和删除操作，并且对事务的完整性、并发性要求不是很高，那么选择这个存储引擎是非常合适的。 
- MEMORY：将所有数据保存在内存中，访问速度快，通常用于临时表及缓存。MEMORY的缺陷就是对表的大小有限制，太大的表无法缓存在内存中，而且无法保障数据的安全性。



# 2. 索引



## 2.1 索引概述



### 2.1.1 介绍



索引（index）是帮助MySQL高效获取数据的数据结构(有序)。在数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用（指向）数据， 这样就可以在这些数据结构上实现高级查找算法，这种数据结构就是索引。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652165298804-4e7d86d0-7b13-4d7e-ad0a-9a4a3352b47d.png)

一提到数据结构，大家都会有所担心，担心自己不能理解，跟不上节奏。不过在这里大家完全不用担心，我们后面在讲解时，会详细介绍。



### 2.1.2 演示



表结构及其数据如下：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652165346843-398b000b-940b-4b76-ae04-609601c6121b.png)

假如我们要执行的SQL语句为 ： select * from user where age = 45;



1). 无索引情况

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652165391106-d3e803b3-4b7f-4be9-9511-545b4e79effc.png)

在无索引情况下，就需要从第一行开始扫描，一直扫描到最后一行，我们称之为 全表扫描，性能很低。



2). 有索引情况

如果我们针对于这张表建立了索引，假设索引结构就是二叉树，那么也就意味着，会对age这个字段下的值建立一个二叉树的索引结构。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652165459940-f05cfdec-410e-404e-a329-fd2cbe6e43e4.png)

此时我们在进行查询时，只需要扫描三次就可以找到数据了，极大的提高的查询的效率。

备注： 这里我们只是假设索引的结构是二叉树，介绍一下索引的大概原理，只是一个示意图，并 

不是索引的真实结构，索引的真实结构，后面会详细介绍。



### 2.1.3 特点-索引优缺点



![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652172119674-f3a5fb0f-206d-4e67-a1f5-6af377feccf0.png)

因为更新表时需要更新索引，调整索引的结构（这点结合更新二叉树理解）

## 2.2 索引结构 



### 2.2.1 概述



MySQL的索引是在存储引擎层实现的，不同的存储引擎有不同的索引结构，主要包含以下几种：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652172168151-6b15b6f1-fec9-4f0b-9613-7d1f5d51f3ab.png)

上述是MySQL中所支持的所有的索引结构，接下来，我们再来看看不同的存储引擎对于索引结构的支持情况。 

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652172182405-dbf72e29-c30b-40ec-b59c-c4396d5e7d0e.png)

注意： 我们平常所说的索引，如果没有特别指明，都是指B+树结构组织的索引

索引的本质：一种有序的数据结构，一般常用的是多路平衡搜索树，即BTREE

索引的作用：帮助MySQL高效获取数据

### 2.2.2 二叉树



假如说MySQL的索引结构采用二叉树的数据结构，比较理想的结构如下：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652165759248-57aa01e5-1b0d-43b4-9de3-d18f92d5e817.png)

如果主键是顺序插入的话，则会形成一个单向链表，结构如下：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652165781033-041503a1-80f3-40f2-a9e9-7655285489ec.png)

所以，如果选择二叉树作为索引结构，会存在以下缺点：

- 顺序插入时，会形成一个链表，查询性能大大降低。 
- 大数据量情况下，层级较深，检索速度慢。



此时大家可能会想到，我们可以选择红黑树，红黑树是一颗自平衡二叉树，那这样即使是主键顺序插入数据，最终形成的数据结构也是一颗平衡的二叉树,结构如下:

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652165846566-63296c09-2e31-4284-a33f-c80f4d795b11.png)

但是，即使如此，由于红黑树也是一颗二叉树，所以也会存在一个缺点：

- 大数据量情况下，层级较深，检索速度慢。

所以，在MySQL的索引结构中，并没有选择二叉树或者红黑树的索引结构，而选择的是B+Tree索引结构，那么什么是 B+Tree呢？在详解B+Tree之前，先来介绍一个B-Tree。 



### 2.2.3 B-Tree



B-Tree，B树是一种多叉平衡查找树，相对于二叉树，B树每个节点可以有多个分支，即多叉。

以一颗最大度数（max-degree）为5(5阶)的B-tree为例，那这个B树每个节点最多存储4个key，5个指针： 

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652166035789-4979cad6-4498-472a-9bed-39c180ee4b79.png)

知识小贴士: 树的度数（degree）指的是一个节点的子节点个数。



我们可以通过一个数据结构可视化的网站来简单演示一下。 

**https://www.cs.usfca.edu/~galles/visualization/BTree.html**

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652166169207-8d5f310d-bd88-485e-923e-c9dd2e72d1dd.png)

插入一组数据： 100 65 169 368 900 556 780 35 215 1200 234 888 158 90 1000 88 120 268 250 121 。

然后观察一些数据插入过程中，节点的变化情况。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652247829524-28fa4dea-9123-4d2c-be71-172daf7209ae.png)

特点： 

- 度数（max-degree）为5(5阶)的B树，每一个节点最多存储4个key，对应5个指针。 
- 一旦节点存储的key数量到达5，就会裂变，中间元素向上分裂。 
- 在B树中，非叶子节点和叶子节点都会存放数据。



度数（max-degree）为4(4阶)的B树，每一个节点最多存储3个key，对应4个指针

一旦节点存储的key数量到达3，就会裂变，中间左侧的元素向上分裂。 

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652247676333-c3732efe-cd16-4290-8f16-fcd9535f8ea4.png)



### 2.2.4 B+Tree索引



B+Tree是B-Tree的变种，我们以一颗最大度数（max-degree）为4（4阶）的B+tree为例，来看一下其结构示意图：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652167386592-2c9a4505-cd47-42da-8668-4be9e870457c.png)

我们可以看到，两部分：

- 绿色虚线框框起来的部分，是索引部分，仅仅起到索引数据的作用，不存储数据。
- 红色虚线框框起来的部分，是数据存储部分，在其叶子节点中要存储具体的数据。

我们可以通过一个数据结构可视化的网站来简单演示一下。

**https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html**

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652167549841-595a5d7c-112f-4287-8733-4c2534488bbe.png)

插入一组数据： 100 65 169 368 900 556 780 35 215 1200 234 888 158 90 1000 88 120 268 250 121 。

然后观察一些数据插入过程中，节点的变化情况。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652170322598-557c75fc-2f1c-4d77-9b2e-2f64c40a512e.png)

最终我们看到，B+Tree 与 B-Tree相比，主要有以下三点区别： 

- 所有的数据都会出现在叶子节点。 
- 叶子节点形成一个单向链表。 
- 非叶子节点仅仅起到索引数据作用，具体的数据都是在叶子节点存放的。

上述我们所看到的结构是标准的B+Tree的数据结构。

接下来，我们再来看看MySQL中优化之后的B+Tree。MySQL索引数据结构对经典的B+Tree进行了优化。在原B+Tree的基础上，增加一个指向相邻叶子节点的链表指针，就形成了带有顺序指针的B+Tree，提高区间访问的性能，利于排序。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652171241389-1f2ea26f-3673-46c9-937f-0cef5deb22d7.png)



### 2.2.5 Hash索引



MySQL中除了支持B+Tree索引，还支持一种索引类型---Hash索引。



1). 结构 

哈希表：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652171428453-9dd3f366-a0c6-4168-b9c8-9bf1f98481d8.png)

哈希索引就是采用一定的hash算法，将键值换算成新的hash值，映射到对应的槽位上，然后存储在hash表中。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652171568654-d1decd62-3874-40af-b3c0-f2c6945e21fb.png)

如果两个(或多个)键值，映射到一个相同的槽位上，他们就产生了hash冲突（也称为hash碰撞），可以通过链表来解决。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652171681016-b4720dc0-826f-4a57-97c4-f551a6c7180e.png)



2). 特点

A. Hash索引只能用于对等比较(=，in)，不支持范围查询（between，>，< ，...） 

B. 无法利用索引完成排序操作 

C. 查询效率高，通常(不存在hash冲突的情况)只需要一次检索就可以了，效率通常要高于B+tree索引



3). 存储引擎支持 

在MySQL中，支持hash索引的是Memory存储引擎。 而InnoDB中具有自适应hash功能，hash索引是InnoDB存储引擎根据B+Tree索引在指定条件下自动构建的。



### 思考题： 

为什么InnoDB存储引擎选择使用B+tree索引结构?

A. 相对于二叉树，层级更少，搜索效率高； 

B. 对于B-tree，无论是叶子节点还是非叶子节点，都会保存数据，这样导致一页中存储的键值减少，指针跟着减少，要同样保存大量数据，只能增加树的高度，导致性能降低； 

C. 相对Hash索引，B+tree支持范围匹配及排序操作；



## 2.3 索引分类 



### 2.3.1 索引分类 



在MySQL数据库，将索引的具体类型主要分为以下几类：**主键索引**、**唯一索引**、**常规索引**、**全文索引**。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652172520434-72b49443-01b4-45eb-a318-bfd91e76c024.png)



### 2.3.2 聚集索引&二级索引



而在InnoDB存储引擎中，根据索引的存储形式，又可以分为以下两种：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652172573387-b0dfc03e-a3c8-4f4c-b871-34caf135daba.png)

聚集索引选取规则: 

- 如果存在主键，主键索引就是聚集索引。
- 如果不存在主键，将使用第一个唯一（UNIQUE）索引作为聚集索引。
- 如果表没有主键，或没有合适的唯一索引，则InnoDB会自动生成一个rowid作为隐藏的聚集索引

聚集索引和二级索引的具体结构如下：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652173055929-c8a7834e-afab-44dd-87e2-810bbe6fdbe6.png)

- 聚集索引的叶子节点下挂的是这一行的数据 。 
- 二级索引的叶子节点下挂的是该字段值对应的主键值（主键字段的值）。

接下来，我们来分析一下，当我们执行如下的SQL语句时，具体的查找过程是什么样子的。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652172909369-89435ce5-f787-49a4-b56c-d7b97f293a25.png)

具体过程如下: 

①. 由于是根据name字段进行查询（给name字段设置了普通索引），所以先根据name='Arm'到name字段的二级索引中进行匹配查找。但是在二级索引中只能查找到 Arm 对应的主键值 10。

②. 由于查询返回的数据是select  *，所以此时，还需要根据主键值10，到聚集索引中查找10对应的记录，最终找到主键值10对应的行row这行数据。

③. 最终拿到这一行的数据，直接返回即可。

回表查询： 这种先到二级索引中查找数据，找到对应的主键值，然后再到聚集索引中根据主键值，获取 

行数据的方式，就称之为回表查询。



#### 思考题：

以下两条SQL语句，那个执行效率高? 为什么? 

A. select * from user where id = 10 ; 

B. select * from user where name = 'Arm' ; 

备注: id为主键，name字段创建的有索引；

解答： 

A 语句的执行性能要高于B 语句。 

因为A语句直接走聚集索引结构，直接返回数据。 而B语句需要先查询name字段的二级索引结构，然后再查询聚集索引结构，也就是需要进行回表查询。



#### 思考题：

InnoDB主键索引的B+tree高度为多高呢?

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652171241389-1f2ea26f-3673-46c9-937f-0cef5deb22d7.png)

假设:

如果一行数据大小为1KB，则一页（InnoDBB存储引擎page16KB）中可以存储16行这样的数据。

InnoDB的指针占用6个Byte 的空间，主键即使为bigint，占用Byte数为8。

计算高度为2：

①由题已知：

n为key  bigint类型的主键值占用8Byte，

(n + 1)为指针  占用6Byte，

Page是由Row组成的，1Page默认占用16KB=16 * 1024Byte。

②可以计算推导：

计算第一层的那一页/块：16 * 1024Byte = n * 8Byte + (n + 1) * 6Byte  ,

算出n约为1170，那么会有1170+1个指针，

1171个指针指向的叶子节点挂着具体的行数据，1171* 16KB(1个Page默认占用16KB) = 18736KB

计算出记录数量为：18736KB / 1KB = 18000

③也就是说，如果B+tree的高度为2，则可以存储 18000 多条记录。

计算高度为3： 

上面的每个叶子节点分别可以在往下生成1171个指针（叶子节点）

所以[1171 * (1171 * 16 KB)]/1KB = 21939856 

也就是说，如果树的高度为3，则可以存储 2200w 左右的记录。



## 2.4 索引语法



### 1). 创建索引 

```sql
CREATE [ UNIQUE | FULLTEXT ] INDEX index_name ON table_name (index_col_name,... ) ; -- index_name简写idx_表名
```



### 2). 查看索引

```sql
SHOW INDEX FROM table_name ;
```



### 3). 删除索引

```sql
DROP INDEX index_name ON table_name ;
```



### 案例演示: 

先来创建一张表 tb_user，并且查询测试数据。

```sql
CREATE TABLE `tb_user` (
  `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '主键',
  `name` varchar(50) NOT NULL COMMENT '用户名',
  `phone` varchar(11) NOT NULL COMMENT '手机号',
  `email` varchar(100) DEFAULT NULL COMMENT '邮箱',
  `profession` varchar(100) DEFAULT NULL COMMENT '专业',
  `age` tinyint(3) unsigned DEFAULT NULL COMMENT '年龄',
  `gender` char(1) DEFAULT NULL COMMENT '性别 , 1: 男, 2: 女',
  `status` char(1) DEFAULT NULL COMMENT '状态',
  `createtime` datetime DEFAULT NULL COMMENT '创建时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=19 DEFAULT CHARSET=utf8mb4 COMMENT='系统用户表';

INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (19, '吕布', '17799990000', 'lvbu666@163.com', '软件工程', 23, '1', '6', '2001-02-02 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (20, '曹操', '17799990001', 'caocao666@qq.com', '通讯工程', 33, '1', '0', '2001-03-05 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (21, '赵云', '17799990002', '17799990@139.com', '英语', 34, '1', '2', '2002-03-02 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (22, '孙悟空', '17799990003', '17799990@sina.com', '工程造价', 54, '1', '0', '2001-07-02 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (23, '花木兰', '17799990004', '19980729@sina.com', '软件工程', 23, '2', '1', '2001-04-22 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (24, '大乔', '17799990005', 'daqiao666@sina.com', '舞蹈', 22, '2', '0', '2001-02-07 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (25, '露娜', '17799990006', 'luna_love@sina.com', '应用数学', 24, '2', '0', '2001-02-08 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (26, '程咬金', '17799990007', 'chengyaojin@163.com', '化工', 38, '1', '5', '2001-05-23 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (27, '项羽', '17799990008', 'xiaoyu666@qq.com', '金属材料', 43, '1', '0', '2001-09-18 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (28, '白起', '17799990009', 'baiqi666@sina.com', '机械工程及其自动化', 27, '1', '2', '2001-08-16 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (29, '韩信', '17799990010', 'hanxin520@163.com', '无机非金属材料工程', 27, '1', '0', '2001-06-12 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (30, '荆轲', '17799990011', 'jingke123@163.com', '会计', 29, '1', '0', '2001-05-11 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (31, '兰陵王', '17799990012', 'lanlinwang666@126.com', '工程造价', 44, '1', '1', '2001-04-09 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (32, '狂铁', '17799990013', 'kuangtie@sina.com', '应用数学', 43, '1', '2', '2001-04-10 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (33, '貂蝉', '17799990014', '84958948374@qq.com', '软件工程', 40, '2', '3', '2001-02-12 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (34, '妲己', '17799990015', '2783238293@qq.com', '软件工程', 31, '2', '0', '2001-01-30 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (35, '芈月', '17799990016', 'xiaomin2001@sina.com', '工业经济', 35, '2', '0', '2000-05-03 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (36, '嬴政', '17799990017', '8839434342@qq.com', '化工', 38, '1', '1', '2001-08-08 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (37, '狄仁杰', '17799990018', 'jujiamlm8166@163.com', '国际贸易', 30, '1', '0', '2007-03-12 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (38, '安琪拉', '17799990019', 'jdodm1h@126.com', '城市规划', 51, '2', '0', '2001-08-15 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (39, '典韦', '17799990020', 'ycaunanjian@163.com', '城市规划', 52, '1', '2', '2000-04-12 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (40, '廉颇', '17799990021', 'lianpo321@126.com', '土木工程', 19, '1', '3', '2002-07-18 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (41, '后羿', '17799990022', 'altycj2000@139.com', '城市园林', 20, '1', '0', '2002-03-10 00:00:00');
INSERT INTO `tb_user`(`id`, `name`, `phone`, `email`, `profession`, `age`, `gender`, `status`, `createtime`) VALUES (42, '姜子牙', '17799990023', '37483844@qq.com', '工程造价', 29, '1', '4', '2003-05-26 00:00:00');
```

表结构中插入的数据如下：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652176026456-b21b26ab-8bb6-4666-8ba8-85d5d6d95eb9.png)

数据准备好了之后，接下来，我们就来完成如下需求：



A. name字段为姓名字段，该字段的值可能会重复，为该字段创建索引。

```sql
CREATE INDEX idx_user_name ON tb_user(name);
```



B. phone手机号字段的值，是非空，且唯一的，为该字段创建唯一索引。

```sql
CREATE UNIQUE INDEX idx_user_phone ON tb_user(phone);
```



C. 为profession、age、status创建联合索引。

```sql
CREATE INDEX idx_user_pro_age_sta ON tb_user(profession,age,status);
```



D. 为email建立合适的索引来提升查询效率。

```sql
CREATE INDEX idx_email ON tb_user(email);
```



完成上述的需求之后，我们再查看tb_user表的所有的索引数据。

```sql
show index from tb_user;
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652176438774-08271df1-f641-428c-a3ef-8c7eddb8d163.png)



## 2.5 SQL性能分析 



### 2.5.1 SQL执行频率 -定位低效SQL



MySQL 客户端连接成功后，通过 show [session|global] status 命令可以提供服务器状态信息。通过如下指令，可以查看当前数据库的INSERT、UPDATE、DELETE、SELECT的访问频次：

```sql
-- session 是查看当前会话 ; 
-- global 是查询全局数据 ; 
-- 这里7个下划线 对应 _select、_update、_insert、_delete

SHOW GLOBAL STATUS LIKE 'Com_______'; 
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652176742784-42330af0-93c9-44f4-a1f0-9758f546af1e.png)

Com_delete: 删除次数 

Com_insert: 插入次数 

Com_select: 查询次数 

Com_update: 更新次数

我们可以在当前数据库再执行几次查询操作，然后再次查看执行频次，看看 Com_select 参数会不会变化。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652176905526-c15c8ca1-aa98-4a29-bea0-fb8d83173548.png)

通过上述指令，我们可以查看到当前数据库到底是以查询为主，还是以增删改为主，从而为数据库优化提供参考依据。 如果是以增删改为主，我们可以考虑不对其进行索引的优化。 如果是以查询为主，那么就要考虑对数据库的索引进行优化了。

那么通过查询SQL的执行频次，我们就能够知道当前数据库到底是增删改为主，还是查询为主。 那假如说是以查询为主，我们又该如何定位针对于那些查询语句进行优化呢？ 其实我们可以借助于慢查询日志。

接下来，我们就来介绍一下MySQL中的慢查询日志。 



### 2.5.2 慢查询日志-定位低效SQL



慢查询日志记录了所有执行时间超过指定参数（long_query_time，单位：秒，默认10秒）的所有SQL语句的日志。

MySQL的慢查询日志默认没有开启，我们可以查看一下系统变量 slow_query_log。

```sql
-- 查看mysql系统的变量 看慢查询日志开启状态
show variables like 'slow_query_log';

-- 开启慢日志查询开关
set global slow_query_log='ON';
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652177282521-544e66c3-61e8-45c0-a3a2-4f15dd9d588c.png)

如果要开启慢查询日志，需要在MySQL的配置文件（/etc/my.cnf）中配置如下信息：

```sql
# 开启MySQL慢日志查询开关 
slow_query_log=1 

# 设置慢日志的时间为2秒，SQL语句执行时间超过2秒，就会视为慢查询，记录慢查询日志 
long_query_time=2
```

配置完毕之后，通过以下指令重新启动MySQL服务器进行测试，查看慢日志文件中记录的信息

/var/lib/mysql/localhost-slow.log。

```plain
systemctl restart mysqld
```

然后，再次查看开关情况，慢查询日志就已经打开了。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652178066343-ba49239d-bf30-49e4-8b9e-162e0c71fb27.png)



#### 测试：

A. 执行如下SQL语句 ：

```sql
-- 这条SQL执行效率比较高, 执行耗时 0.00sec 
select * from tb_user; 
-- 由于tb_sku表中, 预先存入了1000w的记录, count一次,耗时 13.35sec 
select count(*) from tb_sku; 
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652234456618-325c5bf4-ee08-4156-a32a-2b626b17e172.png)



B. 检查慢查询日志 ： 

最终我们发现，在慢查询日志中，只会记录执行时间超多我们预设时间（2s）的SQL，执行较快的SQL 

是不会记录的。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652234613421-b968be17-669b-472f-8a4b-1d633b858ce0.png)

那这样，通过慢查询日志，就可以定位出执行效率比较低的SQL，从而有针对性的进行优化。



### 2.5.3 profile耗时详情-定位之后进行分析



show profiles语句，能够在做SQL优化时帮助我们了解SQL执行时间都耗费到哪里去了。

通过have_profiling参数，能够看到当前MySQL是否支持profile操作：

```sql
-- 查看当前MySQL是否支持profile
SELECT @@have_profiling ; 

-- 查看当前MySQL的profile是否开启了
select @@profiling
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660372430527-d7ed2a1a-538f-4b4c-90e0-3d745fa47892.png)

可以看到，当前MySQL是支持 profile操作的，但是开关是关闭的。可以通过set语句在session/global级别开启profiling：

```sql
SET profiling = 1;
```

profile开关已经打开了，接下来，我们所执行的SQL语句，都会被MySQL记录，并记录执行时间消耗到哪儿去了。 我们直接执行如下的SQL语句：

```sql
select * from tb_user; 
select * from tb_user where id = 1; 
select * from tb_user where name = '白起'; 
select count(*) from tb_sku; 
```

执行一系列的业务SQL语句的操作，然后通过如下指令查看SQL详细的执行耗时：

```sql
-- 查看每一条SQL的耗时基本情况 
show profiles; 

-- 查看指定query_id的SQL语句各个阶段的耗时情况 
show profile for query         query_id; 

-- 查看指定query_id的SQL语句CPU的使用情况 
show profile cpu for query     query_id; 
```

查看每一条SQL的耗时情况: 

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660372459165-29c86989-c897-4ba7-825f-8ef7e9a27e1c.png)

查看指定SQL各个阶段的耗时情况 : 

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660372502964-5ca5a712-3744-4249-958a-994ca60de832.png)



### 2.5.4 explain执行计划-定位之后进行分析



EXPLAIN 或者 DESC命令获取 MySQL 如何执行 SELECT 语句的信息，包括在 SELECT 语句执行过程中表如何连接和连接的顺序。

语法: 

```sql
-- 直接在select语句之前加上关键字 explain / desc 
EXPLAIN SELECT 字段列表 FROM 表名 WHERE 条件 ; 
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652237574611-6265a2b7-bd68-4b83-849d-efd179d2dd37.png)

Explain 执行计划中各个字段的含义: 

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652237602989-3e77e173-91ec-4341-acb7-798a30e7df5d.png)



## 2.6 索引使用



#### 2.6.1 验证索引效率



在讲解索引的使用原则之前，先通过一个简单的案例，来验证一下索引效率，看看是否能够通过索引来提升数据查询性能。在演示的时候，我们还是使用之前准备的一张表 tb_sku , 在这张表中准备了1000w的记录。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652237871936-474da082-88c2-4df7-baac-6a64ec5b3a59.png)

这张表中id为主键，有主键索引，而其他字段是没有建立索引的。 我们先来查询其中的一条记录，看看里面的字段情况，执行如下SQL： 

```sql
select * from tb_sku where id = 1\G;
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652238169496-f530785e-f377-46f4-b6bb-4caf6a5e217c.png)

可以看到即使有1000w的数据,根据id进行数据查询,性能依然很快，因为主键id是有索引的。 

那么接下来，我们再来根据 sn 字段进行查询，执行如下SQL：

```sql
SELECT * FROM tb_sku WHERE sn = '100000003145001';
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652238134066-66596521-fa11-4515-b786-d44c48a9d251.png)

我们可以看到根据sn字段进行查询，查询返回了一条数据，结果耗时 20.78sec，就是因为sn没有索引，而造成查询效率很低。

那么我们可以针对于sn字段，建立一个索引，建立了索引之后，我们再次根据sn进行查询，再来看一下查询耗时情况。

创建索引：

```sql
-- 针对sn字段，建立一个普通索引
create index idx_sku_sn on tb_sku(sn) ;
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652238070498-babd2093-6592-493d-a43c-0fd48d5209e7.png)

查看索引：

```sql
-- 查看tb_sku表中的索引数据
show index from tb_sku;
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652238614782-c99c3a40-7cf5-4ab8-8f69-ca6dd3c3f2d1.png)

然后我们再次执行相同的SQL语句，再次查看SQL的耗时。

```sql
SELECT * FROM tb_sku WHERE sn = '100000003145001';
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652238103209-b5652384-d8ab-4142-9f4a-0d33744c204a.png)

我们明显会看到，sn字段建立了索引之后，查询性能大大提升。建立索引前后，查询耗时都不是一个数量级的。



#### 2.6.2 最左前缀法则



如果索引了多列（联合索引），要遵守最左前缀法则。最左前缀法则指的是查询从索引的最左列开始，并且不建议跳过索引中的列。如果跳跃某一列，索引将会部分失效（后面的字段索引失效）。 



##### 案例说明：

以 tb_user 表为例，我们先来查看一下之前 tb_user 表所创建的索引。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652238753744-982382e2-67de-407f-b584-dd413fc68447.png)

在 tb_user 表中，有一个联合索引idx_user_pro_age_sta，这个联合索引涉及到三个字段，顺序分别为：profession， age，status。

**白话：对于最左前缀法则指的是，查询时，最左边的列必须存在，也就是字段profession必须存在，否则索引全部失效。而且中间不能跳过某一列，否则该列后面的字段索引将失效。**

 接下来，我们来演示几组案例，看一下具体的执行计划：

```sql
explain select * from tb_user where profession = '软件工程' and age = 31 and status = '0';
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652239019163-e379213e-27d9-4bc2-9cf8-f3b717a0dbdb.png)



```sql
explain select * from tb_user where profession = '软件工程' and age = 31;
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652239062920-28fe3f3d-57d8-4d1d-9dd8-79c190afd2e6.png)



```sql
explain select * from tb_user where profession = '软件工程';
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652239107041-8f3f979b-b823-46eb-9a96-3413fbf29c3d.png)

以上的这三组测试中，我们发现只要联合索引最左边的字段 profession不存在，索引就会生效，只不过索引的长度不同。 而且由以上三组测试，我们也可以推测出profession字段索引长度为47、age 字段索引长度为2、status字段索引长度为5。





```sql
explain select * from tb_user where age = 31 and status = '0';
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652239245065-817fce28-754e-4643-ae56-760ee994cceb.png)



```sql
explain select * from tb_user where status = '0';
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652239271148-84758524-26b5-4fab-bcf6-c1ae52c3bbb0.png)

而通过上面的这两组测试，我们也可以看到索引并未生效，原因是因为不满足最左前缀法则，联合索引最左边的列profession字段不存在。





```sql
explain select * from tb_user where profession = '软件工程' and status = '0';
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652239352767-0af97e1d-7e22-4a16-b58f-8c807b414e67.png)

上述的SQL查询时，存在profession字段，最左边的列是存在的，即该SQL语句满足最左前缀法则的基本条件。但是查询时，跳过了age这个列，所以后面的列索引是不会使用的，也就是索引部分生效，所以索引的长度就是47。



##### 思考题：

当执行SQL语句: 

explain select * from tb_user where age = 31 and status = '0' and profession = '软件工程'; 时，是否满足最左前缀法则，走不走上述的联合索引，索引长度？

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652239625143-422d980c-02cf-45bb-a91e-8351623ed55d.png)

可以看到，是完全满足最左前缀法则的，索引长度54，联合索引是生效的。



注意 ： 最左前缀法则中指的最左边的列，是指在查询时，联合索引的最左边的列(即是**第一个字段)必须存在**，与我们编写SQL时，条件编写的**先后顺序无关。**





#### 2.6.3 范围查询



联合索引中，**出现范围查询(>,<)，范围查询右边的列索引失效**。



```sql
explain select * from tb_user where profession = '软件工程' and age > 30 and status = '0';
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652240100428-c2ae1d73-30ba-408e-b2dc-2016f32077e6.png)

当范围查询使用> 或 < 时，走联合索引了，但是索引的长度为49，就说明范围查询右边的status字段是没有走索引的。 



```sql
explain select * from tb_user where profession = '软件工程' and age >= 30 and status = '0';
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652240270031-8ee47e09-4aad-4421-adfe-ae829a88992a.png)

当范围查询使用>= 或 <= 时，走联合索引了，但是索引的长度为54，就说明所有的字段都是走索引的。



所以，在业务允许的情况下，**尽可能的使用类似于 >= 或 <= 这类的范围查询，而避免使用 > 或 < 。 **



#### 2.6.4 索引失效情况



1. like 以%开头，索引无效；当like前缀没有%，后缀有%时，索引有效；
2. or语句前后没有同时使用索引；
3. 数据类型出现隐式转化。如varchar不加单引号的话可能会自动转换为int型，使索引无效，产生全表扫描；
4. 违背最左前缀法则；
5. 在索引列上使用 IS NULL 或 IS NOT NULL有时会导致索引失效，这需要结合第8点分析，当索引列里的值为NULL的情况占多数时，IS NULL就不会走索引，因为这种情况全表扫描比走索引更快；反过来，索引列不为NULL的情况占多数时，IS NOT NULL不会走索引；
6. 在索引字段上使用not，<>，!=。不等于操作符是永远不会用到索引的，因此对它的处理只会产生全表扫描。 优化方法： key<>0 改为 key>0 or key<0;
7. 对索引字段进行计算操作、字段上使用函数。（索引为 emp(ename,empno,sal)）;
8. 当全表扫描速度比索引速度快时，mysql会使用全表扫描，此时索引失效；
9. 在索引字段上使用in会走索引，not in不走索引。

##### 2.6.4.1 索引列上运算



不要在索引列上进行运算操作， 否则索引将失效。



在tb_user表中，除了前面介绍的联合索引之外，还有一个索引，是phone字段的单列索引。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652240495012-b66229fe-6a17-4113-8cb0-21542a80c429.png)



A. 当根据phone字段进行等值匹配查询时, 索引生效。

```sql
explain select * from tb_user where phone = '17799990015';
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652240555653-53bfd9a8-4ca3-46a8-b074-48ae0af3b0c4.png)



B. 当根据phone字段进行函数运算操作之后，索引失效。

```sql
explain select * from tb_user where substring(phone,10,2) = '15';
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652240589092-02354161-3e14-4432-80ed-dff80eb8c061.png)



##### 2.6.4.2 字符串不加引号



字符串类型的字段使用时，不加引号，此时索引将失效。



接下来，我们通过两组示例，来看看对于字符串类型的字段，加单引号与不加单引号的区别：

```sql
explain select * from tb_user where profession = '软件工程' and age = 31 and status = '0'; 

explain select * from tb_user where profession = '软件工程' and age = 31 and status = 0; 
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652240720911-a0b198d3-e44f-4516-b1eb-4dd7a0f33544.png)



```sql
explain select * from tb_user where phone = '17799990015'; 

explain select * from tb_user where phone = 17799990015; 
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652240756259-72359ff4-2d28-4b6c-875b-9e0c12d42671.png)

经过上面两组示例，我们会明显的发现，如果字符串类型字段使用时不加单引号，对于查询结果，没什么影响，但是数据库存在隐式类型转换，索引将失效。

对于字符串类型的字段使用时，不加引号，MySQL底层会隐式转换为字符串类型。



##### 2.6.4.3 模糊查询



如果仅仅是尾部模糊匹配，索引不会失效。如果是头部模糊匹配，索引失效。包含模糊匹配，索引失效。



接下来，我们来看一下这三条SQL语句的执行效果，查看一下其执行计划：

由于下面查询语句中，都是根据profession字段查询，符合最左前缀法则，联合索引是可以生效的， 

我们主要看一下，模糊查询时，%加在关键字之前，和加在关键字之后对索引的影响。

```sql
-- 尾部模糊匹配
explain select * from tb_user where profession like '软件%'; 

-- 头部模糊匹配
explain select * from tb_user where profession like '%工程'; 

-- 包含模糊匹配
explain select * from tb_user where profession like '%工%'; 
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652248913760-53a862ad-71fb-46fa-8b36-8f872af53d0f.png)

经过上述的测试，我们发现，在like模糊查询中，在关键字后面加%，索引可以生效。而如果在关键字前面加了%，索引将会失效。包含模糊匹配，索引失效。



##### 2.6.4.4 or连接条件



用or分割开的条件， 如果or前面的条件中的列有索引，而or后面的列中没有索引，那么涉及的索引都不会被用到。

```sql
-- id列有索引，age列没有索引
explain select * from tb_user where id = 10 or age = 23; 

-- phone列有索引，age列没有索引
explain select * from tb_user where phone = '17799990017' or age = 23;
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652249268105-f6417ea3-feaf-4471-976b-38dd5cb596dd.png)

由于age没有索引，所以即使id、phone有索引，索引也会失效。所以需要针对于age也要建立索引。

然后，我们可以对age字段建立索引。

```sql
create index idx_user_age on tb_user(age);
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652249387297-698a7b2c-bd7a-4464-9191-fd5b26c800f6.png)

建立了索引之后，我们再次执行上述的SQL语句，看看前后执行计划的变化。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652249445543-7e83ac7d-6615-4460-a32f-ebc460484029.png)

最终，我们发现，当or连接的条件，左右两侧字段都有索引时，索引才会生效。



##### 2.6.4.5 数据分布对是否走索引的影响



（查询优化器）

如果MySQL评估使用索引的效率比全表扫描更慢，则不使用索引。

------

查看tb_user表中的所有索引：

```sql
show index from tb_user;
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652250039169-1f28d2fd-2dfa-43ed-92e8-9dcc6fa71ce0.png)

查看tb_user表中的所有数据：

```sql
select * from tb_user;
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652250224462-1de1e249-cf3d-4651-a558-f8d8c456083c.png)

执行如下两条语句 ：

```sql
select * from tb_user where phone >= '17799990005'; 

select * from tb_user where phone >= '17799990015';
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652249551086-d0539b0a-2ea3-4e12-a07f-a1652774f96f.png)

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652249570195-b48b7208-ee91-45c2-8c4b-69d9a886b1de.png)

经过测试我们发现，相同的SQL语句，只是传入的字段值不同，最终的执行计划也完全不一样，这是为什么呢？

就是因为MySQL在查询时，会评估使用索引的效率与走全表扫描的效率，如果走全表扫描更快，则放弃索引，它去走全表扫描。 因为索引是用来索引少量数据的，如果通过索引查询返回大批量的数据，MySQL评估还不如走全表扫描来的快，此时索引就会失效。



------

接下来，我们再来看看 is null 与 is not null 操作是否走索引。

执行如下两条语句 ：

```sql
explain select * from tb_user where profession is null; 

explain select * from tb_user where profession is not null; 
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652250342680-935b51c8-a20a-41c4-8476-8b019c33d189.png)

接下来，我们做一个操作将profession字段值全部更新为null。 

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652250362322-3d8b908e-338e-4297-b2b8-c4ba9cff1b74.png)

然后，再次执行上述的两条SQL，查看SQL语句的执行计划。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652250410605-f18104ce-932c-46e6-b812-6cd7b9681ed2.png)

最终我们看到，一模一样的SQL语句，先后执行了两次，结果查询计划是不一样的，为什么会出现这种现象？这是和数据库的数据分布有关系。查询时MySQL会评估，走索引快，还是全表扫描快，如果全表扫描更快，则放弃索引走全表扫描。 因此，is null 、is not null是否走索引，得具体情况具体分析，并不是固定的。



#### 2.6.5 SQL提示



目前tb_user表的数据情况如下:

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652252493406-8db340c6-7d96-4354-9cd6-64fe54cd1b00.png)

索引情况如下:

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652252519910-4c2ea92e-829a-4844-b0e1-8ed8fe2c168a.png)

把上述的 idx_user_age, idx_email 这两个之前测试使用过的索引直接删除。

```sql
drop index idx_user_age on tb_user; 
drop index idx_email on tb_user;
```



A. 执行SQL : explain select * from tb_user where profession = '软件工程';

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652252578253-149d7e40-8118-44dc-9d8a-a1070edfd173.png)

查询走了联合索引。



B. 执行SQL创建profession的单列索引：create index idx_user_pro on tb_user(profession);

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652252612105-fd8d3877-fc90-4972-9070-8ce62597b08e.png)



C. 创建单列索引后，再次执行A中的SQL语句，查看执行计划，看看到底走哪个索引。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652252634657-01215348-4548-4984-a794-33c183f77968.png)

测试结果，我们可以看到，possible_keys中 idx_user_pro_age_sta,idx_user_pro 这两个索引都可能用到，最终MySQL选择了使用idx_user_pro_age_sta索引。这是MySQL自动选择的结果。

那么，我们能不能在查询的时候，自己来指定使用哪个索引呢？ 答案是肯定的，此时就可以借助于MySQL的SQL提示来完成。 接下来，介绍一下SQL提示。



SQL提示，是优化数据库的一个重要手段，简单来说，就是在SQL语句中加入一些人为的提示来达到优化操作的目的。

1). use index ： 建议MySQL使用哪一个索引完成此次查询（仅仅是建议，mysql内部还会再次进行评估）。

```sql
explain select * from tb_user use index(idx_user_pro) where profession = '软件工程';
```



2). ignore index ： 忽略指定的索引。

```sql
explain select * from tb_user ignore index(idx_user_pro) where profession = '软件工程';
```



3). force index ： 强制使用索引。

```sql
explain select * from tb_user force index(idx_user_pro) where profession = '软件工程';
```



示例演示： 

A. use index

```sql
explain select * from tb_user use index(idx_user_pro) where profession = '软件工程';
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652252866397-4998150d-6428-4ed5-8724-6e4a14e3def4.png)



B. ignore index

```sql
explain select * from tb_user ignore index(idx_user_pro) where profession = '软件工程';
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652252928877-dd660c3b-074b-44e4-9d83-c8f0e7059026.png)



C. force index

```sql
explain select * from tb_user force index(idx_user_pro_age_sta) where profession = '软件工程';
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652252959503-cccf8d90-c134-4b51-b8d3-16a19ae980ab.png)



#### 2.6.6 覆盖索引



尽量使用覆盖索引，减少select *。 那么什么是覆盖索引呢？ 覆盖索引是指 查询语句走了索引，并且查询语句需要返回的列，在该索引列中已经全部能够找到 。 



接下来，我们来看一组SQL的执行计划，看看执行计划的差别，然后再来具体做一个解析。

```sql
explain select id, profession from tb_user where profession = '软件工程' and age = 31 and status = '0' ; 

explain select id,profession,age, status from tb_user where profession = '软件工程' and age = 31 and status = '0' ; 

explain select id,profession,age, status, name from tb_user where profession = '软件工程' and age = 31 and status = '0' ;

explain select * from tb_user where profession = '软件工程' and age = 31 and status = '0'; 
```

上述这几条SQL的执行结果为:

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652253213176-9133f6d1-10bb-4e92-97b5-0f8aad57510f.png)

从上述的执行计划我们可以看到，这四条SQL语句的执行计划前面所有的指标都是一样的，看不出来差异。但是此时，我们主要关注的是后面的Extra(额外的)，前面两条SQL的结果为 Using where; Using Index ; 而后面两条SQL的结果为: Using index condition 。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652253287435-5476a430-faa7-46bb-b61a-ab38081acc39.png)

因为，在tb_user表中有一个联合索引 idx_user_pro_age_sta，该索引关联了三个字段 

profession、age、status，而这个索引也是一个二级索引结构，所以叶子节点下面挂的是这一行的主 

键id。 所以当我们查询返回的数据列在 id、profession、age、status 之中，则直接走二级索引 

直接返回数据了。 如果超出这个范围，就需要拿到主键id，再去扫描聚集索引，再获取额外的数据列

了，这个过程就是回表。 而我们如果一直使用select * 查询返回所有字段值，则很容易就会造成回表 

查询（除非是根据主键查询，此时只会扫描聚集索引 。select *  from tb_user where id = 1 ）。



为了大家更清楚的理解，什么是覆盖索引，什么是回表查询，我们一起再来看下面案例的这组SQL的执行过程。

A. 表结构及索引结构示意图:

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652253766191-004ee930-2e55-4d8f-ada3-661a34e81bf5.png)

id是主键，是一个聚集索引。 name字段建立了普通索引，是一个二级索引（辅助索引）。



B. 执行SQL : select * from tb_user where id = 2;

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652253860027-f4371631-77db-4943-9579-f781add9b99d.png)

根据id查询，直接走聚集索引查询，一次索引扫描，直接返回数据（行Row数据），性能高。



C. 执行SQL：selet id,name from tb_user where name = 'Arm';

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652253995238-77176503-0de0-4323-beb7-44c459ecc51a.png)

虽然是根据name字段查询，查询二级索引，但是由于查询返回在字段为 id，name，在name的二级索引中，这两个值都是可以直接获取到的，因为覆盖索引，所以不需要回表查询，性能高。



D. 执行SQL：selet id,name,gender from tb_user where name = 'Arm';

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652255264469-6a06dbef-ba27-43f7-9f9b-daab9e3e13bd.png)

由于在name列的二级索引中，不包含gender列，所以，需要两次索引扫描（拿着name列对应的主键值），也就是需要回表查询，性能相对较差一点。



##### 思考题：

一张表, 有四个字段(id, username, password, status), 由于数据量大, 需要对以下SQL语句进行优化, 该如何进行才是最优方案:

select id,username,password from tb_user where username = 'itcast';



答案: 

针对于 username, password建立联合索引, 

创建索引sql为: create index idx_user_name_pass on tb_user(username,password); 

这样可以避免上述的SQL语句，在查询的过程中，出现回表查询



#### 2.6.7 前缀索引



当字段类型为字符串（varchar，text，longtext等）时，有时候需要索引很长的字符串，这会让索引结构变得很大（索引结构占用很大的磁盘空间），查询时，浪费大量的磁盘IO， 影响查询效率。此时可以只将字符串的一部分前缀，建立索引（称之为前缀索引），这样可以大大节约索引结构空间，从而提高索引效率。



##### 1). 创建前缀索引的语法

```sql
create index idx_xxxx on table_name(xxxcolumn(n)) ;
```

示例: 

为tb_user表的email字段，建立长度为5的前缀索引。

```sql
create index idx_email_5 on tb_user(email(5));
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652259586978-beb0aec5-dc00-4043-99bb-62919a0e44bf.png)



##### 2). 前缀长度

前缀长度可以根据索引的选择性来决定，而选择性是指统计不重复的索引裂字段值（作为基数）和统计数据表中的记录总数（作为分母）的比值， 索引选择性（比值）越高则查询效率越高， 唯一索引的选择性是1，这是最好的索引选择性，性能也是最好的。

```sql
-- 选择性 = 统计不重复的索引列字段值（基数）和数据表的记录总数的比值
select count(distinct email) / count(*) from tb_user ;  //1.0000

-- email列字段值再截取短一点，看看比值（索引选择性）
select count(distinct substring(email,1,5)) / count(*) from tb_user ; //0.9583 [综合一下这个前缀长度，查询效率是高一些的]

-- email列字段值再再截取短一点，看看比值（索引选择性）
select count(distinct substring(email,1,4)) / count(*) from tb_user ;//0.9167
```



##### 3). 前缀索引的查询流程 

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652260408874-783b30ce-3764-461c-92cd-44a1fc1dfaa4.png)



#### 2.6.8 单列索引与联合索引



单列索引：即一个索引只包含单个列。 

联合索引：即一个索引包含了多个列。



我们先来看看 tb_user 表中目前的索引情况:

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652260628291-6c74aebf-1360-4e5e-aa77-b39e6e302c6e.png)可以看到tb_user表中既有单列索引，又有联合索引。

接下来，我们来执行一条SQL语句，看看其执行计划：

```sql
explain select id,phone,name from tb_user where phone = '17799990010' and name = '韩信';
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652260755196-8590a6c3-d060-4e46-a549-8596322f7ce2.png)

通过上述执行计划我们可以看出来，在and连接的两个字段 phone、name上都是有各自的单列索引的，但是最终mysql只会选择一个索引，也就是说，上述SQL语句只能走一个字段的索引结构，此时是会回表查询的。



紧接着，我们再来创建一个phone和name字段的联合索引:

```sql
create unique index idx_user_phone_name on tb_user(phone,name);
```

来查询一下SQL语句执行计划。

```sql
explain select id,phone,name from tb_user where phone = '17799990010' and name = '韩信';
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652261841736-a7ce2ec3-3ee7-43e5-8eda-b1ec643230bd.png)

此时，查询时，就走了联合索引，而在联合索引的二级索引结构中包含 phone列、name列的信息，在叶子节点下挂的是对应的主键id，所以查询是无需回表查询的。



在业务场景中，如果存在多个and查询条件，考虑针对于查询字段建立索引时，建议建立联合索引，而非单列索引。



如果查询使用的是联合索引，具体的结构示意图如下：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652262148159-80297868-2853-4b63-ad68-cd7a08240f5f.png)



## 2.7 索引设计原则



1). 针对于数据量较大，且查询比较频繁的表建立索引。 

2). 针对于常作为查询条件（where）、排序（order by）、分组（group by）操作的字段建立索引。 

3). 尽量选择区分度高的列作为索引，尽量建立唯一索引，区分度越高，使用索引的效率越高。 

区分度高的列，如身份证号；区分度不高的列，如性别。

4). 如果是字符串类型的字段，字段的长度较长，可以针对于字段的特点，建立前缀索引。 

5). 尽量使用联合索引，减少单列索引，查询时，联合索引很多时候可以覆盖索引，节省存储空间，避免回

表查询，提高查询效率。 

6). 要控制索引的数量，索引并不是多多益善，索引越多，维护索引结构的代价也就越大，会影响增删改的

效率。

7). 如果确定索引列不能存储NULL值的话，请在创建表时使用NOT NULL约束这个列。当优化器知道每列是否包含 NULL值时，优化器可以更好地确定哪个索引最有效地用于查询。



# 3. SQL优化

SQL优化步骤：

1. 定位SQL性能瓶颈位置，见 2.5 SQL性能分析-定位低效SQL
2. 分析SQL，见 2.5 SQL性能分析-分析SQL
3. 优化SQL，（本3. SQL优化要讲如何优化SQL语句）



### 3.1 插入数据 



#### 3.1.1 insert

如果我们需要一次性往数据库表中插入多条记录，可以从以下三个方面进行优化。

```sql
insert into tb_test values(1,'tom'); 
insert into tb_test values(2,'cat'); 
insert into tb_test values(3,'jerry'); 
..... 
```



##### 1). 优化

批量插入数据，应该尽量使用多个值的一条insert语句，减少客户端与数据库之间的连接、关闭操作；

```sql
Insert into tb_test values(1,'Tom'),(2,'Cat'),(3,'Jerry');
```



##### 2). 再优化

设置手动提交事务，原因参考海量数据导入优化。

```sql
-- 开启事务
start transaction; 
-- 批量插入
insert into tb_test values(1,'Tom'),(2,'Cat'),(3,'Jerry'); 
insert into tb_test values(4,'Tom'),(5,'Cat'),(6,'Jerry'); 
insert into tb_test values(7,'Tom'),(8,'Cat'),(9,'Jerry'); 
-- 提交事务
commit; 
```



##### 3). 再再优化

主键顺序插入，性能要高于乱序插入，原因参考海量数据导入优化。

```sql
主键乱序插入 : 8 1 9 21 88 2 4 15 89 5 7 3  -- 还要来回比对值
主键顺序插入 : 1 2 3 4 5 7 8 9 15 21 88 89  -- B+Tree顺序指针，顺序排好的一颗树结构。
```



#### 3.1.2 海量插入数据

如果一次性需要插入大批量数据(比如: 几百万的记录)，使用insert语句插入性能较低，此时可以使用MySQL数据库提供的load指令进行插入。操作如下：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652263049438-a7428f5f-b3c2-4f4e-9456-7b619145bf6d.png)

可以执行如下指令，将数据脚本文件中的数据加载到表结构中：

```sql
-- 客户端连接服务端时，加上参数 -–local-infile 
mysql –-local-infile -u root -p 

-- 设置全局参数local_infile为1，开启从本地加载文件导入数据的开关 
set global local_infile = 1; 

-- 执行load指令将准备好的数据，加载到表结构中 
load data local infile '/root/sql1.log' into table tb_user fields terminated by ',' lines terminated by '\n' ;

//terminated by 终止于
//fields terminated by ',' 每个字段以','分隔
//lines terminated by '\n' 每个以 换行符 分隔
```

主键顺序插入性能高于乱序插入



示例演示: 

A. 创建表结构

```sql
CREATE TABLE `tb_user` ( 
    `id` INT(11) NOT NULL AUTO_INCREMENT, 
    `username` VARCHAR(50) NOT NULL, 
    `password` VARCHAR(50) NOT NULL, 
    `name` VARCHAR(20) NOT NULL, 
    `birthday` DATE DEFAULT NULL, 
    `sex` CHAR(1) DEFAULT NULL, 
    PRIMARY KEY (`id`), 
    UNIQUE KEY `unique_user_username` (`username`) 
) ENGINE=INNODB DEFAULT CHARSET=utf8 ; 
```



B. 设置参数

```sql
-- 客户端连接服务端时，加上参数 -–local-infile 
mysql –-local-infile -u root -p 

-- 设置全局参数local_infile为1，开启从本地加载文件导入数据的开关 
set global local_infile = 1; 
```



C. load加载数据

```sql
# 导入指定文件的数据到指定表
# 文件中的数据格式："1,wy,123123\n2,xy,123123\n3,zy,123123"

load data local infile '/root/load_user_100w_sort.sql' into table tb_user fields terminated by ',' lines terminated by '\n' ; 
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652264391423-26e43769-494b-45cc-8364-f11e87c5b571.png)

我们看到，插入100w的记录，17s就完成了，性能很好。

在load时，主键顺序插入性能高于乱序插入

| **优化方法**         | **原因**                                                     |
| -------------------- | ------------------------------------------------------------ |
| 建议按照主键顺序导入 | 因为存在主键，所以必定要维护至少一个索引，而InnoDB类型的表创建的索引是B+数结构，所以按照顺序方式导入会更快。因此这里建议将数据按照主键顺序排序后再导入。 |
| 关闭唯一性校验       | 如果数据表中某个列是唯一属性的，那么每导入一条数据时，都会对该列进行唯一性校验，即判断是否重复，如果重复，则不导入。所以在导入数据时可以先执行"SET UNIQUE_CHECKS=0;"关闭唯一性校验，导入完成后再"SET UNIQUE_CHECKS=1;"开启。注意：这么做的前提是你一定要保证你导入的数据中，该列的所有值都是唯一的。 |
| 设置手动提交事务模式 | 如果不设置手动提交事务，那么默认每条插入语句都是一个事务，每次都要提交事务。设置手动提交事务的话，可以在循环前开启事务，循环结束后再提交事务，只需要提交一次事务。所以可以通过"SET AUTOCOMMIT=0"关闭自动提交，导入数据后再"SET AUTOCOMMIT=1"开启。最后还是要提醒一点，手动提交事务模式确实能加快效率，但是不能说手动提交事务就比自动提交事务要好。比如，使用手动提交模式时，如果有一个数据插入异常导致失败，那么前面插入的数据就都失败了。而自动提交模式就能保证”失败的数据“之前的所有数据都已经插入完成，不会丢失 |



### 3.2 主键优化



在上一小节，我们提到，主键顺序插入的性能是要高于乱序插入的。 这一小节，就来介绍一下具体的原因，然后再分析一下主键又该如何设计。



#### 1). 数据组织方式-IOT

在InnoDB存储引擎中，表中的数据都是根据主键顺序组织存放的，如下这种存储方式的表称为索引组织表 

(index organized table IOT)。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652265176385-f6ed7939-22dd-4161-887a-4e67044fcd3d.png)

行数据，都是存储在聚集索引的叶子节点上的。而我们之前也讲解过InnoDB的逻辑结构图：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652163156378-eec36f8c-fc9e-4f94-973a-49af93bb4c6a.png)

在InnoDB引擎中，数据行是记录在逻辑结构 page 页中的，而每一个页的大小是固定的，默认16KB。 

那也就意味着， 一个页中所存储的行也是有限的，如果插入的数据行row在该页存储不了，将会存储 

到下一个页中，页与页之间会通过指针连接。



#### 2). 页分裂

页可以为空，也可以填充一半，也可以填充100%。每个页包含了2~N条行数据(如果一行数据占用空间过大，会行溢出，既当前页没有足够的空间存这条行数据了)，根据主键排列。



##### A. 主键顺序插入效果 

①. 从磁盘中申请页， 主键顺序插入

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652265809330-13b4633c-59c7-4372-8b85-22e6cdea2c5e.png)

②. 第一个页没有满，继续往第一页插入

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652265901582-dfc02383-1b70-4038-992a-69c9929a0b66.png)

③. 当第一个也写满之后，再写入第二个页，页与页之间会通过指针连接

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652265926104-609820d4-9ad9-4ef9-a9f3-af604ba342a9.png)

④. 当第二页写满了，再往第三页写入

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652265957296-92e2e634-2d11-4016-8f5a-7b4c4e4ae90d.png)



##### B. 主键乱序插入效果

①. 假如1#,2#页都已经写满了，存放了如图所示的数据

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652266025896-54571dd1-65cb-4a4e-b4f3-c5e2c6b48a6f.png)

②. 此时再插入id为50的记录，我们来看看会发生什么现象

会再次开启一个页，写入新的页中吗？

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652266055605-59d6cef3-e34a-40e0-95f1-f3cbe95a9454.png)

不会。因为，B+Tree索引结构的叶子节点是有顺序的。按照顺序，应该存储在47之后。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652266098538-e932f874-8e88-4bc3-9845-4d4fdf722f89.png)

但是47所在的1#页，已经写满了，存储不了50对应的数据了。 那么此时会开辟一个新的页 3#。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652266133584-01b4b2a1-b243-4f51-8a1f-9c311b1a2769.png)

但是并不会直接将50存入3#页，而是会将1#页后一半的数据，移动到3#页，然后在3#页，插入50。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652266177887-527832a3-db9e-4301-9602-a3f25246618e.png)

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652266230086-c73b219c-b89f-4189-9d7d-d6b5c05b7172.png)

移动数据，并插入id为50的数据之后，那么此时，这三个页之间的数据顺序是有问题的。 1#的下一个页，应该是3#， 3#的下一个页是2#。 所以，此时，需要重新设置链表指针。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652266303395-6ae93ad5-c312-491b-8836-1150f7107250.png)

上述的这种现象，称之为 "页分裂"，是比较耗费性能的操作。



#### 3). 页合并

假如目前表中已有数据的索引结构(叶子节点)如下：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652266366339-bca0ee05-8665-4f92-b975-b2e29cd02b2b.png)

当我们对已有数据进行删除时，具体的效果如下:

当删除一行记录时，实际上记录并没有被物理删除，只是记录被标记（flaged）为删除并且它的空间变得允许被其他记录声明使用。 

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652266414144-28cc3641-d4eb-474d-b113-5ce3075ce122.png)

当我们继续删除2#的数据记录

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652266457029-6420981c-a445-4446-bdf2-234bd3b95540.png)

当页中删除的记录达到 MERGE_THRESHOLD（默认为页的50%），InnoDB会开始寻找最靠近的页（前或后）看看是否可以将两个页合并以优化空间使用。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652266564266-fad0dfdd-baa0-46aa-9fe0-00b8e85fdf9e.png)

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652266619173-3c1e498f-0fdc-4b3c-a0bc-32d8aa365840.png)

删除数据，并将页合并之后，再次插入新的数据20，则直接插入3#页

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652266658970-1bd3e4be-45b2-430f-b738-5c9d04b57a0c.png)

这个里面所发生的合并页的这个现象，就称之为 "页合并"。



##### 知识小贴士：  

MERGE_THRESHOLD：合并页的阈值，可以自己设置，在创建表或者创建索引时指定。



#### 4). 索引设计原则

- 满足业务需求的情况下，尽量降低主键的长度。 
- 插入数据时，尽量选择顺序插入，选择使用AUTO_INCREMENT自增主键。
- 尽量不要使用UUID做主键或者是其他自然主键，如身份证号。
- 业务操作时，避免对主键的修改。 

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652266882157-c9630aba-fec1-4ada-bd07-b524659bbd01.png)

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652266959034-e42c40a1-77dd-4cd7-9111-57e150a17bb5.png)



### 3.3 order by优化



MySQL的排序，有两种方式：

- Using filesort : 通过表的索引或全表扫描，读取满足条件的数据行，然后在排序缓冲区sort buffer中完成排序操作，所有不是通过索引直接返回排序结果的排序都叫 FileSort 排序。 
- Using index : 通过有序索引顺序扫描直接返回有序数据，这种情况即为 using index，不需要额外排序，操作效率高。 

对于以上的两种排序方式，Using index的性能高，而Using filesort的性能低，我们在优化排序操作时，尽量要优化为 Using index。

order by的优化主要在于减少FileSort的出现



接下来，我们来做一个测试： 

A.  数据准备 

​      把之前测试时，为tb_user表所建立的部分索引直删除掉

```sql
drop index idx_user_phone on tb_user; 
drop index idx_user_phone_name on tb_user; 
drop index idx_user_name on tb_user; 
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652267189787-5cb43522-9b19-417b-9e24-41aeb10c4931.png)



B. 执行排序SQL

```sql
explain select id,age,phone from tb_user order by age ;
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652267250574-d48cac24-0d6e-48f2-8405-715dc385165b.png)



```sql
explain select id,age,phone from tb_user order by age, phone ;
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652267270354-86bb8fba-eda6-4d35-9f7b-4b9fa6f258d3.png)

由于 age, phone 都没有索引，所以此时再排序时，出现Using filesort， 排序性能较低。



C. 创建索引

```sql
-- 创建联合索引 age,phone
create index idx_user_age_phone_aa on tb_user(age,phone);
```



D. 创建索引后，根据age, phone进行升序排序

```sql
explain select id,age,phone from tb_user order by age; 
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652267344487-8e1d1c64-1ed7-4c3c-a64f-d7885f550e0e.png)



```sql
explain select id,age,phone from tb_user order by age , phone;
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652267371160-39325fd2-cef7-442f-ac34-ab94be71aa5b.png)

建立索引之后，再次进行排序查询，就由原来的Using filesort， 变为了 Using index，性能就是比较高的了。



E. 创建索引后，根据age, phone进行降序排序

```sql
explain select id,age,phone from tb_user order by age desc , phone desc ;
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652354164896-6e415970-94f7-4cb5-b6b2-93284598bff1.png)

也出现 Using index， 但是此时Extra中出现了 Backward index scan，这个代表反向扫描索引，因为在MySQL中我们创建的索引，默认索引的叶子节点是从小到大排序的，而此时我们查询排序时，是从大到小，所以，在扫描时，就是反向扫描，就会出现 Backward index scan。 在MySQL8版本中，支持降序索引，我们也可以创建降序索引。



F. 根据phone，age进行升序排序，phone在前，age在后。

```sql
explain select id,age,phone from tb_user order by phone , age;
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652354260517-9eb88ca1-149c-42be-8275-2c0c5acb8981.png)

排序时,也需要满足最左前缀法则,否则也会出现 filesort。因为在创建索引的时候， age是第一个字段，phone是第二个字段，所以排序时，也就该按照这个顺序来，否则就会出现 Using filesort。 



F. 根据age, phone进行降序一个升序，一个降序

```sql
explain select id,age,phone from tb_user order by age asc , phone desc ;
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652354343424-a2c44326-2e18-428f-8e46-e09471050516.png)

因为创建索引时，如果未指定顺序，默认都是按照升序排序的，而查询时，一个升序，一个降序，此时就会出现Using filesort。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652354412763-2f5fc9d6-b221-4aa1-ae45-612de4fa984b.png)

为了解决上述的问题，我们可以创建一个索引，这个联合索引中 age 升序排序，phone 倒序排序。



G. 创建联合索引(age 升序排序，phone 倒序排序)

```sql
create index idx_user_age_phone_ad on tb_user(age asc ,phone desc);
//ad  asc desc
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652354702368-6c4e1f09-e7a2-4067-8439-91de270495db.png)



H. 然后再次执行如下SQL

```sql
explain select id,age,phone from tb_user order by age asc , phone desc ;
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652354760714-d563c519-e5d5-4a5b-b342-ea4ec9755138.png)



#### 升序/降序联合索引结构图示: 

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652354842988-c5c586b0-d77e-49ab-9c33-47735eec6518.png)

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652354862275-0b768d89-bd29-4086-a1d5-251d5e672849.png)

#### 由上述的测试,我们得出order by优化原则:

A. 根据排序字段建立合适的索引，多字段排序时，也遵循最左前缀法则。 

B. 尽量使用覆盖索引。 

C. 多字段排序, 一个升序一个降序，此时需要注意联合索引在创建时的规则（ASC/DESC）。 

D. 如果不可避免的出现filesort，大数据量排序时，可以适当增大排序缓冲区大小sort_buffer_size(默认256k)。 



### 3.4 group by优化



分组操作，我们主要来看看索引对于分组操作的影响。



首先我们先将 tb_user 表的索引全部删除掉 。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652355489653-615fc7b3-7840-47b8-89fb-f13f5753e201.png)

接下来，在没有索引的情况下，执行如下SQL，查询执行计划：

```sql
explain select profession , count(*) from tb_user group by profession ;
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652355627317-2a097984-80ef-4beb-b1c9-6541817bbb84.png)

然后，我们在针对于 profession ，age，status 创建一个联合索引。

```sql
create index idx_user_pro_age_sta on tb_user(profession , age , status);
```

紧接着，再执行前面相同的SQL查看执行计划。 

```sql
explain select profession , count(*) from tb_user group by profession ;
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652355697039-db62527d-17c9-4552-baef-37e25de3b0c9.png)

再执行如下的分组查询SQL，查看执行计划：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652355789342-d1edbfe7-4a86-4f8c-a00f-e8349489c27c.png)

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652355797908-b3c291b4-9bf1-4e09-97e3-3145f93d634b.png)

我们发现，如果仅仅根据age分组，就会出现 Using temporary ；而如果是 根据 profession,age两个字段同时分组，则不会出现 Using temporary。原因是因为对于分组操作，在联合索引中，也是符合最左前缀法则的。 



所以，

#### 在分组操作中，我们需要通过以下两点进行优化，以提升性能： 

A. 在分组操作时，可以通过索引来提高效率。 

B. 分组操作时，索引的使用也是满足最左前缀法则的。



### 3.5 limit优化



在数据量比较大时，如果进行limit分页查询，在查询时，越往后，分页查询效率越低。

我们一起来看看执行limit分页查询耗时对比：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652355912619-3a77da0d-9ab4-4e97-9f88-2a4d5b2df0bd.png)

通过测试我们会看到，越往后，分页查询效率越低，这就是分页查询的问题所在。

因为，当在进行分页查询时，如果执行 limit 2000000,10 ，此时需要MySQL排序前2000010条记录，仅仅返回 2000000 ~ 2000010 的记录，其他记录丢弃，查询排序的代价非常大 。



#### 优化思路: 

一般分页查询时，通过创建 覆盖索引 能够比较好地提高性能，可以通过覆盖索引加子查询形式进行优化。

```sql
explain select * from tb_sku t , (select id from tb_sku order by id limit 2000000,10) a where t.id = a.id;

-- (select id from tb_sku order by id limit 2000000,10) 走聚集索引，覆盖索引
```



### 3.6 count优化 



#### 3.6.1 概述



```sql
select count(*) from tb_user ;
```

在之前的测试中，我们发现，如果数据量很大，在执行count操作时，是非常耗时的。



- MyISAM 引擎把一个表的总行数存在了磁盘上，因此执行 count(*) 的时候会直接返回这个数，效率很高； 但是如果是带条件的count，MyISAM也慢。 
- InnoDB 引擎就麻烦了，它执行 count(*) 的时候，需要把数据一行一行地从引擎里面读出来，然后累积计数。

如果说要大幅度提升InnoDB表的count聚合函数的效率，主要的优化思路：自己计数(可以借助于redis这样的数据库进行,但是如果是带条件的count又比较麻烦了)。



#### 3.6.2 count用法

 

count() 是一个聚合函数，对于返回的结果集，一行行地判断，如果 count 函数的参数不是NULL，累计值就加 1，否则不加，最后返回累计值。

用法：count（*）、count（主键）、count（字段）、count（数字）

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652356256559-a7c1069e-65ca-4780-824b-d1f0e6a1c532.png)

按照效率排序的话，count(字段) < count(主键 id) < count(1) ≈ count(*)，所以尽量使用 count(*)。 



### 3.7 update优化



我们主要需要注意一下update语句执行时的注意事项。

```sql
update course set name = 'javaEE' where id = 1 ;

//id列 创建了主键索引，上面的SQL更新语句走了聚集索引
```

当我们在执行删除的SQL语句时，会锁定id为1这一行的数据，然后事务提交之后，行锁释放。



但是当我们在执行如下SQL时。

```sql
update course set name = 'SpringBoot' where name = 'PHP' ;

//name列 没有创建索引，上面的SQL更新语句索引失效
```

当我们开启多个事务，在执行上述的SQL时，我们发现行锁升级为了表锁。 导致该update语句的性能大大降低。（这种更新语句没有走索引，锁表，影响表的并发性能）



update更新数据的时候，

InnoDB的行锁是针对索引加的锁，不是针对记录加的锁 ，并且该索引不能失效，否则会从行锁升级为表锁。



# 4. 视图/存储过程/触发器



## 4.1 视图



### 4.1.1 介绍



视图（View）是一种虚拟存在的表（它是一条SELECT语句查询基表执行后返回的结果集）。视图（View）中的数据并不在数据库中实际物理存在，行和列数据来自定义视图的查询中使用的表（基表），并且（视图（View）中的行和列数据）是在使用视图时动态生成的（基表中的数据改变改变的话，视图中的数据也会动态跟着变化，并且视图的查询也会走基表中的索引）。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652358329514-505db2b5-a417-417f-805a-00fa51a25e74.png)

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652358368523-6ea9dde7-5aa5-4e31-822d-68db671553a7.png)

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652358464096-922836fb-a29c-433b-859e-046654d2d0d2.png)



通俗的讲，视图只是保存了查询的SQL逻辑，不保存查询的数据结果集（原因是基表中的数据改变的话，视图中的数据也会动态跟着变化）。所以我们在创建视图（View）的时候，主要的工作就落在创建这条SQL查询语句上。



### 4.1.2 语法



#### 1). 创建

```sql
CREATE [OR REPLACE] VIEW 视图名称[(列名列表)] 
AS 
SELECT语句 [ WITH [ CASCADED | LOCAL ] CHECK OPTION ]
```



#### 2). 查询

```sql
查看创建视图语句：SHOW CREATE VIEW 视图名称; 
查看视图数据：SELECT * FROM 视图名称 ...... ; 
```



#### 3). 修改

```sql
方式一：
CREATE [OR REPLACE] VIEW 视图名称[(列名列表)] 
AS 
SELECT语句 [ WITH [ CASCADED | LOCAL ] CHECK OPTION ] 


方式二：
ALTER VIEW 视图名称[(列名列表)] 
AS 
SELECT语句 [ WITH [ CASCADED | LOCAL ] CHECK OPTION ] 
```



#### 4). 删除

```sql
DROP VIEW [IF EXISTS] 视图名称 [,视图名称] ...
```



#### 演示示例：

```sql
-- 创建视图 
create or replace view stu_v_1 
as 
select id,name from student where id <= 10; 

-- 查询视图 
show create view stu_v_1; 
select * from stu_v_1; 
select * from stu_v_1 where id < 3; 

-- 修改视图 
create or replace view stu_v_1 
as 
select id,name,no from student where id <= 10; 

alter view stu_v_1 
as 
select id,name from student where id <= 10; 

-- 删除视图 
drop view if exists stu_v_1; 
```

上述我们演示了，视图应该如何创建、查询、修改、删除，那么我们能不能通过视图来插入、更新数据呢？ 接下来，做一个测试。

```sql
create or replace view stu_v_1 
as 
select id,name from student where id <= 10 ;

select * from stu_v_1; 

-- 往视图中插入数据
insert into stu_v_1 values(6,'Tom'); 
insert into stu_v_1 values(17,'Tom22'); 
```

执行上述的SQL，我们会发现，id为6和17的数据都是可以成功插入的。 但是我们执行查询，查询出来的数据，却没有id为17的记录。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652356995208-3dbd2e7f-dc91-42c0-8b4c-e3285153ee03.png)

因为我们在创建视图的时候，指定的条件为 id<=10, id为17的数据，是不符合条件的，所以没有查询出来，但是这条数据确实是已经成功的插入到了基表中。

如果我们定义视图的时候，如果指定了条件，然后我们在插入、修改、删除数据时，是否可以做到必须满足条件才能操作（插入、修改、删除数据），否则不能够操作（插入、修改、删除数据）呢？ 

答案是可以的，这就需要借助于视图的检查选项了。



### 4.1.3 检查选项



当使用WITH CHECK OPTION子句创建视图时，MySQL会通过视图检查正在更改的每个行，例如 插入，更新，删除，以使其符合视图的定义。 MySQL允许基于另一个视图创建视图，它还会检查依赖视图中的规则以保持一致性。为了确定检查的范围，mysql提供了两个选项： CASCADED 和 LOCAL ，默认值为CASCADED（级联） 。（套娃）



#### 1). CASCADED（级联）

级联。

比如，v2视图是基于v1视图的，如果在v2视图创建的时候指定了检查选项为 cascaded，但是v1视图创建时未指定检查选项。 则MySQL在执行检查时，不仅会检查v2，还会级联检查v2的关联视图v1。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652357200571-cead888e-0262-4737-8155-68c1b3b5da47.png)



#### 2). LOCAL（本地）

本地。

比如，v2视图是基于v1视图的，如果在v2视图创建的时候指定了检查选项为 local ，但是v1视图创建时未指定检查选项。 则MySQL在执行检查时，只会检查v2，不会检查v2的关联视图v1。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652357294110-dc2eddf0-7a29-49e4-9544-67353b853e95.png)



### 4.1.4 视图的更新/插入



要使视图可更新，视图中的行与基础表中的行之间必须存在一对一的关系。如果视图包含以下任何一项，则该视图不可更新：

A. 聚合函数或窗口函数（SUM()、 MIN()、 MAX()、 COUNT()等） 

B. DISTINCT 

C. GROUP BY 

D. HAVING 

E. UNION 或者 UNION ALL  联合查询



示例演示:

```sql
create view stu_v_count 
as 
select count(*) from student;
```

上述的视图中，就只有一个单行单列的数据，如果我们对这个视图进行更新或插入的，将会报错。

```sql
 insert into stu_v_count values(10); 
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652357440103-14654a1e-e6c2-4d0f-ba70-6d7c75c97643.png)



### 4.1.5 视图作用



#### 1). 简单

视图不仅可以简化用户（开发人员）对数据的理解，也可以简化他们的操作。那些被经常使用的查询可以被定义为视图，从而使得用户（开发人员）不必为以后的操作每次指定全部的条件。



#### 2). 安全

我们知道数据库可以授权，但不能授权到数据库特定行和特定的列上。通过视图用户（开发人员）只能查询和修改他们所能见到的数据。



#### 3). 数据独立

视图可帮助用户（开发人员）屏蔽真实表结构变化带来的影响。



### 4.1.6 案例



1). 为了保证数据库表的安全性，开发人员在操作tb_user表时，只能看到的User的基本字段，屏蔽手机号和邮箱两个字段。

```sql
-- 创建视图
create view tb_user_view 
as 
select id,name,profession,age,gender,status,createtime from tb_user; 

-- 查询视图数据
select * from tb_user_view; 
```



2). 查询每个学生所选修的课程（三张表联查），这个功能在很多的业务中都有使用到，为了简化操作，定义一个视图。

```sql
-- 创建视图
create view tb_stu_course_view 
as 
select s.name student_name , s.no student_no , c.name course_name 
from student s, student_course sc , course c 
where s.id = sc.studentid and sc.courseid = c.id; 


select * from tb_stu_course_view; 
```

视图的查询也会走基表中的索引

视图的查询也会走基表中的索引

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652358074853-c4fbbe9d-f952-4a54-93bf-0b7e2cb8478f.png)



## 4.2 存储过程 （不推荐使用）

### 为什么不推荐使用存储过程和触发器

1. 存储过程和触发器二者是有很大的联系的，我的一般理解就是触发器是一个隐藏的存储过程，因为它不需要参数，不需要显示调用，往往在你不知情的情况下已经做了很多操作。从这个角度来说，由于是隐藏的，无形中增加了系统的复杂性，非DBA人员理解起来数据库就会有困难，因为它不执行根本感觉不到它的存在。
2. 再有，涉及到复杂的逻辑的时候，触发器的嵌套是避免不了的，如果再涉及几个存储过程，再加上事务等等，很容易出现死锁现象，再调试的时候也会经常性的从一个触发器转到另外一个，级联关系的不断追溯，很容易使人头大。其实，从性能上，触发器并没有提升多少性能，只是从代码上来说，可能在coding的时候很容易实现业务，所以我的观点是：摒弃触发器！触发器的功能基本都可以用存储过程来实现。
3. 在编码中存储过程显示调用很容易阅读代码，触发器隐式调用容易被忽略。
4. 存储过程也有他的致命伤；
5. 存储过程的致命伤在于移植性，存储过程不能跨库移植，比如事先是在mysql数据库的存储过程，考虑性能要移植到oracle上面那么所有的存储过程都需要被重写一遍。

总结：只有在并发不高的项目，管理系统中用。如果是面向用户的高并发应用，都不要使用。触发器和存储过程本身难以开发和维护，不能高效移植。触发器完全可以用事务替代。存储过程可以用后端脚本替代。



### 4.2.1 介绍



存储过程是事先经过编译并存储在数据库中的一段 SQL 语句的集合，调用存储过程可以简化应用开发人员的很多工作，减少数据在数据库和应用服务器之间的传输，对于提高数据处理的效率是有好处的。存储过程思想上很简单，就是数据库 SQL 语言层面的代码封装与重用。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652358624995-5be14945-9b8c-4c64-95e0-c2cca03d1030.png)

特点: 

- 封装，复用 -----------------------> 可以把某一业务SQL封装在存储过程中，需要用到的时候直接调用即可。 
- 可以接收参数，也可以返回数据 --------> 在存储过程中，可以传递参数，也可以接收返回值。 
- 减少网络交互，效率提升 -------------> 如果涉及到多条SQL，每执行一次都是一次网络传输。 而如果多条SQL封装在存储过程中，我们只需要网络交互一次可能就可以了。



### 4.2.2 基本语法



#### 1). 创建

```sql
CREATE PROCEDURE 存储过程名称 ([ 参数列表 ]) 
BEGIN
-- SQL语句 
END ; 

//PROCEDURE procedure 程序
```



#### 2). 调用

```sql
CALL 名称 ([ 参数 ]); 
```



#### 3). 查看

```sql
-- 查询指定数据库的存储过程及状态信息   'xxx'  数据库名称
SELECT * FROM INFORMATION_SCHEMA.ROUTINES WHERE ROUTINE_SCHEMA = 'xxx'; 

-- 查询某个存储过程的定义
SHOW CREATE PROCEDURE 存储过程名称 ; 

//ROUTINES routines 例程
//SCHEMA schema 架构
```



#### 4). 删除

```sql
DROP PROCEDURE [ IF EXISTS ] 存储过程名称 ；
```

注意: 

在命令行中，执行创建存储过程的SQL时，需要通过关键字 delimiter 指定SQL语句的结束符。



#### 演示示例: 

```sql
-- 存储过程基本语法 
-- 创建 
create procedure p1() 
begin
select count(*) from student;  --SQL语句
end; 
-- 调用 
call p1(); 
-- 查询指定数据库的存储过程及状态信息
select * from information_schema.ROUTINES where ROUTINE_SCHEMA = 'itcast'; 
-- 查询某个存储过程的定义
show create procedure p1; 
-- 删除 
drop procedure if exists p1;
```





### 4.2.3 变量 



在MySQL中变量分为三种类型: 系统变量、用户定义变量、局部变量。



#### 4.2.3.1 系统变量



系统变量 是MySQL服务器提供，不是用户定义的，属于服务器层面。分为全局变量[GLOBAL]、会话变量[SESSION]。

##### 1). 查看系统变量

```sql
SHOW [ SESSION | GLOBAL ] VARIABLES ; -- 查看所有系统变量 
SHOW [ SESSION | GLOBAL ] VARIABLES LIKE '......'; -- 可以通过LIKE模糊匹配方式查找变量 
SELECT @@[SESSION | GLOBAL] 系统变量名; -- 查看指定变量的值 

//VARIABLES variables 变量
```



##### 2). 设置系统变量

```sql
SET [ SESSION | GLOBAL ] 系统变量名 = 值 ; 
SET @@[SESSION | GLOBAL] 系统变量名 = 值 ; 
```

注意: 

如果没有指定SESSION/GLOBAL，默认是SESSION，会话变量。 



mysql服务重新启动之后，所设置的全局参数会失效，要想不失效，可以在 /etc/my.cnf 中配置。 



A. 全局变量(GLOBAL): 全局变量针对于所有的会话。 

B. 会话变量(SESSION): 会话变量针对于单个会话，在另外一个会话窗口就不生效了。



##### 演示示例: 

```sql
-- 查看系统变量 
show session variables ; 
show session variables like 'auto%'; 
show global variables like 'auto%'; 
select @@global.autocommit; 
select @@session.autocommit; 

-- 设置系统变量 
set session autocommit = 1; 
insert into course(id, name) VALUES (6, 'ES'); 
set global autocommit = 0; 
select @@global.autocommit; 
```





#### 4.2.3.2 用户定义变量



用户定义变量 是用户根据需要自己定义的变量，用户变量不用提前声明，在用的时候直接用 "@变量名" 使用就可以。其作用域为当前连接。



##### 1). 赋值

方式一:

```sql
SET @var_name = expr [, @var_name = expr] ... ; 
SET @var_name := expr [, @var_name := expr] ... ; 
```

赋值时，可以使用 = ，也可以使用 := 。



方式二:

```sql
SELECT @var_name := expr [, @var_name := expr] ... ; 
SELECT 字段名 INTO @var_name FROM 表名; 
```



##### 2). 使用

```sql
SELECT @var_name ;
```

注意: 用户定义的变量无需对其进行声明或初始化，只不过获取到的值为NULL。



##### 演示示例: 

```sql
-- 赋值 
set @myname = 'itcast'; 
set @myage := 10; 
set @mygender := '男',@myhobby := 'java'; 
select @mycolor := 'red'; 
select count(*) into @mycount from tb_user; 

-- 使用 
select @myname,@myage,@mygender,@myhobby; 
select @mycolor , @mycount;
select @abc;
```



#### 4.2.3.3 局部变量



局部变量 是根据需要定义的在局部生效的变量，访问之前，需要DECLARE(声明)声明。可用作存储过程内的局部变量和输入参数，局部变量的范围是在其内声明的BEGIN ... END块。



##### 1). 声明

```sql
DECLARE 变量名 变量类型 [DEFAULT ... ] ;
```

变量类型就是数据库字段类型：INT、BIGINT、CHAR、VARCHAR、DATE、TIME等。



##### 2). 赋值

```sql
SET 变量名 = 值 ; 
SET 变量名 := 值 ; 
SELECT 字段名 INTO 变量名 FROM 表名 ... ;
```



##### 演示示例: 

```sql
-- 声明局部变量 - declare 
-- 赋值 
create procedure p2() 
begin
    declare stu_count int default 0; 
    select count(*) into stu_count from student; 
    select stu_count; 
end; 


call p2(); 
```



#### 4.2.4 if



##### 1). 介绍

if 用于做条件判断，具体的语法结构为：

```sql
IF 条件1 THEN 
    ..... 
ELSEIF 条件2 THEN -- 可选 
    ..... 
ELSE -- 可选 
    ..... 
END IF; 
```

在if条件判断的结构中，ELSE IF 结构可以有多个，也可以没有。 ELSE结构可以有，也可以没有。



##### 2). 案例

根据定义的分数score变量，判定当前分数对应的分数等级。 

- score >= 85分，等级为优秀。 
- score >= 60分 且 score < 85分，等级为及格。 
- score < 60分，等级为不及格。

```sql
create procedure p3() 
begin
    declare score int default 58; 
    declare result varchar(10); 
    
    if score >= 85 then 
        set result := '优秀'; 
    elseif score >= 60 then 
        set result := '及格'; 
    else
        set result := '不及格'; 
    end if; 
    
    select result; 
end; 


call p3(); 
```

上述的需求我们虽然已经实现了，但是也存在一些问题，比如：score 分数我们是在存储过程中定义 

死的，而且最终计算出来的分数等级，我们也仅仅是最终查询展示出来而已。 

那么我们能不能，把score分数动态的传递进来，计算出来的分数等级是否可以作为返回值返回呢？ 

答案是肯定的，我们可以通过接下来所讲解的 参数 来解决上述的问题。



#### 4.2.5 参数



##### 1). 介绍

参数的类型，主要分为以下三种：IN、OUT、INOUT。 具体的含义如下：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652359867220-da07b602-877a-4337-a87d-995ce0fe59f4.png)



## 4.3 存储函数



## 4.4 触发器



# 5. InnoDB锁 



[mysql锁详解](https://www.jianshu.com/p/7004f7571427)

## 锁类型



在InnoDB内部用uint32类型数据表示锁的类型, 

- 最低的 4 个 bit 表示 lock_mode, 
- 5-8 bit 表示 lock_type(目前只用了 5 和 6 位，大小为 16 和 32 ，表示 LOCK_TABLE 和 LOCK_REC), 
- 剩下的高位 bit 表示行锁的类型record_lock_type

| record_lock_type | lock_type | lock_mode |
| ---------------- | --------- | --------- |
|                  |           |           |

我们说锁的时候，一般都是讲什么lock_mode的record_lock_type锁。因为我们很少分析表锁，一般分析行锁。比如LOCK_S的LOCK_REC_NOT_GAP锁，表示共享的记录锁（非间隙锁）



## 5.1 概述



锁是计算机协调多个进程或线程并发访问某一资源的机制。在数据库中，除传统的计算资源（CPU、RAM、I/O）的争用以外，数据也是一种供许多用户共享的资源。如何保证数据并发访问的一致性、有效性是所有数据库必须解决的一个问题，锁冲突也是影响数据库并发访问性能的一个重要因素。从这个角度来说，锁对数据库而言显得尤其重要，也更加复杂。



MySQL中的锁是   在并发访问时，解决数据访问的一致性、有效性问题的一种机制。



MySQL中的锁按照锁的粒度，分为以下三类： 

- 全局锁：锁定数据库中的所有表。 
- 表级锁：每次操作锁住整张表。 
- 行级锁：每次操作锁住对应的行数据。



## 5.2 全局锁 



### 5.2.1 介绍



全局锁：对整个数据库实例加锁，全局锁后整库就处于只读状态，则后续的DML的写语句，DDL语句，以及更新操作的事务提交语句都将被阻塞。



全局锁的一个典型场景：全库逻辑备份，--single-transaction实现一致性读。

对所有的表进行锁定，从而获取一致性视图，保证数据的完整性。



为什么全库逻辑备份，InnoDB底层会加全局锁呢？

A. 我们一起先来分析一下不加全局锁，可能存在的问题。

假设在数据库中存在这样三张表: tb_stock 库存表，tb_order 订单表，tb_orderlog 订单日志表。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652363068657-c1dd3205-fce8-4b38-a86f-39e0e0e11502.png)

- 在进行数据备份时，先备份了tb_stock库存表。 
- 然后接下来，在业务系统中，执行了下单操作，扣减库存，生成订单（更新tb_stock表，插入tb_order表）。 
- 然后再执行备份 tb_order表的逻辑。 
- 业务中执行插入订单日志操作。 
- 最后，又备份了tb_orderlog表。

此时备份出来的数据，是存在问题的。因为备份出来的数据，tb_stock表与tb_order表的数据不一 致(有最新操作的订单信息,但是库存数没减)。

那如何来规避这种问题呢? 此时就可以借助于MySQL的全局锁来解决。



B. 再来分析一下加了全局锁后的情况

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652363115249-4f060233-a238-4e92-8f6d-02a3392cd144.png)

对数据库进行进行逻辑备份之前，先对整个mysql系统加上全局锁，一旦加了全局锁之后，其他的DDL、DML全部都处于阻塞状态，但是可以执行DQL语句，也就是处于只读状态，而数据备份就是查询操作。 

加上全局锁后那么数据在进行逻辑备份的过程中，各个数据库中的数据就是不会发生变化的，这样就保证了数据的一致性和完整性。



### 5.2.2 语法



#### 1). 加全局锁

```sql
flush tables with read lock ;

//对整个mysql系统加上全局锁，整个mysql系统处于只读状态，各个数据库只能查询，数据库备份也属于查询操作。
```



#### 2). 数据备份

```sql
mysqldump -uroot –p1234 itcast > itcast.sql
```

mysql系统提供的备份工具，数据备份的相关指令, 在后面MySQL管理章节, 还会详细讲解.



#### 3). 释放锁

```sql
unlock tables ;
```



### 5.2.3 特点



数据库中加全局锁，是一个比较重的操作，存在以下问题： 

- 如果在主库上备份（给主库加了一把全局锁的话），那么在备份期间都不能执行更新，业务基本上就得停摆。 
- 如果在从库上备份（给从库加了一把全局锁的话），那么在备份期间从库不能执行主库同步过来的二进制日志（binlog），会导致主从延迟。

在InnoDB引擎中，我们可以在备份时加上参数 --single-transaction 参数来完成不加锁的一致性数据备份。

```sql
mysqldump --single-transaction -uroot –p123456 itcast > itcast.sql
```



## 5.3 表级锁 



### 5.3.1 介绍



表级锁，每次操作锁住的是整张表。锁定粒度大，发生锁冲突的概率最高，并发度最低。应用在MyISAM、 InnoDB、BDB等存储引擎中。



对于表级锁，主要分为以下三类：

- 表锁 
- 元数据锁（meta data lock，MDL） 
- 意向锁



### 5.3.2 表锁



对于表锁，分为两类： 

- 表共享读锁（read lock） 
- 表独占写锁（write lock）



语法： 

- 主动在表上加读锁或写锁：lock tables   表名...   read/write。 
- 释放锁：unlock tables / 客户端断开连接 。



特点: 

A. 读锁

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652425656543-a004be9f-1f59-4e40-b218-8015c37747ad.png)

左侧为客户端一，对指定表加了读锁（也阻断自己的写），不会影响右侧客户端二的读，但是会阻塞右侧客户端的写。 测试:

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652425798030-80909900-1c51-48a5-b7c5-8d7221185595.png)



B. 写锁

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652425904695-16f17834-6167-4758-b49d-de1b799e0ccd.png)

左侧为客户端一，对指定表加了写锁，会阻塞右侧客户端的读和写。

测试:

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652426086698-61d877df-523b-4fbb-a779-075fd249b559.png)



#### 结论: 

对指定表加读锁不会阻塞其他客户端的读，但是会阻塞写。

对指定表加写锁既会阻塞其他客户端的读，又会阻塞其他客户端的写。



### 5.3.3 元数据锁（MDL）



 元数据锁 meta data lock ，简写MDL锁。

MDL加锁过程是MySQL系统自动控制，无需显式使用，在客户端访问一张表的时候会自动加上。MDL锁的主要作用是维护表元数据的数据一致性，在表上有活动事务的时候，不可以对元数据进行写入操作。**元数据锁为了避免并发执行DML语句与 DDL语句的冲突，保证读写的正确性。**

这里的元数据，大家可以简单理解为就是一张表的表结构。 也就是说，某一张表涉及到未提交的事务时，是不能够修改这张表的表结构的。

在MySQL5.5中引入了MDL，当对一张表进行增删改查的时候，给表加MDL读锁(共享)；当对表结构进行变更操作的时候，给表加MDL写锁(排他)。



常见的SQL语句执行时，MySQL对于当前事务所添加的元数据锁MDL如下图：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652426367601-cef80bf7-70b9-4fc1-9f1e-48ea127abd80.png)



演示： 

当执行SELECT、INSERT、UPDATE、DELETE等语句时，MySQL给表添加的是元数据共享锁（SHARED_READ / SHARED_WRITE  这两种类型的DML锁之间是兼容的。）

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652451953281-fbba8cc3-59c2-4dd7-bb27-9b13e389773a.png)

当执行SELECT语句时，MySQL给表添加的是元数据共享锁（SHARED_READ），会阻塞元数据排他锁 

（EXCLUSIVE），之间是互斥的。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652453077154-9f7e7229-e746-4e3a-bd23-dffc3f72adf6.png)



我们可以通过下面的SQL，来查看数据库中的元数据锁的情况：

```sql
select object_type,object_schema,object_name,lock_type,lock_duration from performance_schema.metadata_locks ;
```

我们在操作过程中，可以通过上述的SQL语句，来查看元数据锁的加锁情况。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652453456223-288a8eb6-c5ce-45d1-89b8-349608415fd3.png)



### 5.3.4 意向锁



#### 1). 介绍



为了避免DML SQL语句在执行时，加的（行级锁中的）行锁与（表级锁中的）表锁的冲突，在InnoDB存储引擎中引入了（表级锁中的）意向锁，使得加（表级锁中的）表锁时不用检查每行数据是否加了（行级锁中的）行锁，使用（表级锁中的）意向锁来减少（行级锁中的）行锁的检查。



假如没有（表级锁中的）意向锁这个东西，客户端一对表加了（行级锁中的）行锁后，客户端二如何给表加（表级锁中的）表锁呢，来通过示意图简单分析一下：

首先客户端一，开启一个事务，然后执行DML SQL语句操作，在执行DML SQL语句时，会对涉及到的行加（行级锁中的）行锁。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652453619417-d07621b4-679d-403d-9304-cb359e9e9e90.png)

当客户端二，想对这张表加表锁时，会检查当前表是否有对应的行锁，如果没有，则添加表锁，此时就 

会从第一行数据，检查到最后一行数据，效率较低。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652453739468-de4e3576-429a-43a6-91bc-4747ac6a6e78.png)



有了意向锁之后 :

客户端一，在执行DML操作时，会对涉及的行加上行锁，同时也会对该 表 加上一把意向锁。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652453917635-9e86a60e-48ef-4e2d-9054-a9837109c24a.png)

而其他客户端，在对这张表加表锁的时候，InnoDB会根据该表上所加的意向锁来判定是否可以成功加表锁，而不用逐行检查判断行锁情况了。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652454015902-a2e4b01e-ae77-4e31-94a7-b74717e18c8f.png)



#### 2). 分类

- 意向共享锁(IS): 由语句select ... lock in share mode执行时给该表加上。 与 表锁共享读锁 (read lock)兼容，与表锁排他锁(write lock 表独占写锁)互斥。 
- 意向排他锁(IX): 由语句insert、update、delete、select...for update执行时给该表加上 。与 表锁共享锁(read lock)及表锁排他锁(write lock 表独占写锁)都互斥，意向锁之间不会互斥。

一旦事务提交了，在事务期间InnoDB给该表加上的意向共享锁(IS)、意向排他锁(IX)，都会自动释放。



可以通过以下SQL，查看意向锁及行锁的加锁情况：

```sql
select object_schema,object_name,index_name,lock_type,lock_mode,lock_data from performance_schema.data_locks;
```



演示： 

A. 意向共享锁(IS)与表共享读锁 (read lock)是兼容的，与表排他锁(write lock 表独占写锁)是互斥的

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652454722050-28904ca1-f551-4bd2-8a1f-4967beb91530.png)



B. 意向排他锁(IX)与表共享读锁 (read lock)、表独占写锁（write lock）都是互斥的

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652455066654-030dc9a0-ef58-4607-8332-147388802807.png)





## 5.4 行级锁 



### 5.4.1 介绍 



行级锁，InnoDB存储引擎操作行级锁，锁住对应的行记录。

分清注意：

行级锁的锁粒度最小，发生锁冲突的概率最低，并发能力最高。应用在InnoDB存储引擎中。 

InnoDB存储引擎存储的数据是基于(IOT)索引组织维护的，而行锁是通过对索引组织上的索引项加锁来实现的，并不是对row加锁实现滴。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660284380710-20c1f7b0-2eb9-4ac0-8404-f25e586e5167.png)



对于行级锁，主要分为以下三类（下面的定义要参考着(IOT)索引组织结构才能弄明白！）：

- 行锁（Record Lock）：

锁定单个行记录，防止其他事务对此行记录进行update业务和行delete业务，防止发生并发事务中的脏读问题、不可重复读问题。

在RC、RR隔离级别下被支持。   RC  Read Commited读已提交、RR Repeatable Read 可重复读。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660284761882-42de2592-0015-4de8-8dd9-d460b5696b7c.png)

- 间隙锁（Gap Lock）：

锁定索引记录之前的所有间隙（不含该记录），确保某个事务执行业务SQL时整个索引组织间隙不变，防止其他事务在这个间隙进行insert业务，这个间隙范围内防止发生并发事务中的幻读问题。

在RR隔离级别下被支持。RR Repeatable Read 可重复读。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660287354360-e1d61b27-8f99-4de0-a5fe-c21679f495ef.png)

- 临键锁（Next-Key Lock）：

行锁 Record Lock 和间隙锁 Gap Lock 组合，同时锁住当前记录，并锁住当前记录之前的所有间隙，这个临间范围内防止发生并发事务中的脏读、不可重复读问题、幻读。

  	在RR隔离级别下被支持。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660287660477-e4fa67a1-0545-4933-8231-d3712af0dff3.png)



### 5.4.2 行锁



#### 1). 介绍

InnoDB存储引起实现了以下两种类型的行锁： 

- 行锁之   共享锁（S）：允许获取共享锁的事务去读一行，阻止其他事务获得相同数据集的排它锁。 
- 行锁之   排他锁（X）：允许获取排他锁的事务去更新数据，阻止其他事务获得相同数据集的共享锁和排他锁。

两种类型的行锁之间的兼容情况如下:

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652455453275-3222f577-b8eb-4023-be0b-0013a26f5f15.png)



基础篇中常见的SQL语句在执行时，InnoDB存储引擎会自动给SQL语句涉及到的行记录加上行锁，如下：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1652455496829-618fb7a6-8651-4cf6-84be-be2b55df6888.png)



#### 2). 行锁演示



默认情况下，InnoDB在 REPEATABLE READ 可重复读事务隔离级别下运行，业务SQL被执行时，InnoDB使用 Next-Key Lock 临键锁进行检索和索引项扫描，以防止幻读。

- 针对唯一索引进行检索时，对已存在的记录进行等值查询(唯一索引)时，Next-Key Lock 临键锁将会自动优化为 Record Lock 行锁，以防止幻读。 
- InnoDB的行锁是针对于索引字段加的锁，如果业务SQL语句执行时未命中索引，那么InnoDB将会对表中的所有行记录加锁，意味着此时Next-Key Lock 临键锁 就会升级为（表级索中的）表锁，以防止幻读。



我们可以通过执行下面这条SQL，查看具体的业务SQL被执行时，InnoDB存储引擎下（表级索中的）意向锁及（行级索中的）Record Lock 行锁的加锁情况：

```sql
select object_schema,object_name,index_name,lock_type,lock_mode,lock_data from performance_schema.data_locks;
```



**示例演示**

数据准备: 

```sql
CREATE TABLE `stu` ( `id` INT NOT NULL PRIMARY KEY AUTO_INCREMENT, `name` VARCHAR ( 255 ) DEFAULT NULL, `age` INT NOT NULL ) ENGINE = INNODB CHARACTER 
SET = utf8mb4;
INSERT INTO `stu`
VALUES
	( 1, 'tom', 1 );
INSERT INTO `stu`
VALUES
	( 3, 'cat', 3 );
INSERT INTO `stu`
VALUES
	( 8, 'rose', 8 );
INSERT INTO `stu`
VALUES
	( 11, 'jetty', 11 );
INSERT INTO `stu`
VALUES
	( 19, 'lily', 19 );
INSERT INTO `stu`
VALUES
	( 25, 'luci', 25 );
```

演示 Record Lock 行锁的时候，我们就通过上面这张表来演示一下。



A. 普通的业务select语句被执行时，不会加锁。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660272768992-104419dd-df81-4c34-8584-341e560b0ee0.png)



B. select...lock in share mode，加行锁之   共享锁（S），下图中可以看出来 共享锁与共享锁之间是兼容的。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660273194462-92e5fa3b-fdfb-4b6a-8801-4a85ac5de78e.png)



下图中可以看出来 共享锁与排他锁之间是互斥的。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660273498511-44f66fb4-5aa7-446c-8a07-4d6b0925a5f7.png)

客户端一获取的是id = 1这行的共享锁，客户端二是可以获取id = 3这行的排它锁的，因为不是同一行记录。 而如果客户端二想获取id = 1这行的排他锁，会处于阻塞状态，因为共享锁与排他锁之间是互斥的。



C. 排它锁与排他锁之间互斥

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660273762397-69b975cc-d4e9-4e5a-9697-5e6f5f1520ef.png)

当客户端一，执行update语句，InnoDB会为id为1的行记录加排他锁； 客户端二，如果也执行update语句更新id为1的数据，InnoDB也要为id为1的行记录加排他锁，但是客户端二会处于阻塞状态，因为排他锁之间是互斥的。 直到客户端一把事务提交了，InnoDB才会把这一行的行锁释放掉，此时客户端二才有可能给id为1的行记录加排他锁，解除阻塞。



D. 业务SQL被执行时，无索引命中的话，那么行锁将升级为表锁，以防止幻读。

stu表中数据如下:

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660273982076-b527da1d-5dfc-4813-bbbc-c648b0f06e60.png)

我们在两个客户端中执行如下操作:

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660274077173-ab35083d-b4df-45c6-8e04-7809f78ab470.png)

在客户端一中，开启事务，并执行update业务语句，更新name为Lily的数据，也就是id为19的记录 。 然后在客户端二中更新id为3的记录，却不能直接执行，会处于阻塞状态，为什么呢？ 

原因就是因为此时，客户端一根据name字段进行更新时，name字段是没有索引的，如果业务语句执行时没有命中索引，此时行锁会升级为表锁(因为行锁是对索引项加的锁，而name字段没有索引)。



接下来，我们再针对name字段建立索引，索引建立之后，再次做一个测试：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660274436503-96e6d45b-71c1-44bc-ac0c-0fd4165213f3.png)

此时我们可以看到，客户端一，第一步开启事务，然后依然是根据name进行更新。而客户端二，第一步开启事务，在更新id为3的数据时，更新成功，并未进入阻塞状态。 这样就说明，我们编写的SQL业务语句根据索引字段进行更新的话，就可以避免行锁升级为表锁的情况。



### 5.4.3 间隙锁&临键锁



#### 1). 介绍

默认情况下，InnoDB在 REPEATABLE READ 可重复读事务隔离级别下运行，业务SQL被执行时，InnoDB使用 Next-Key Lock 临键锁进行检索和索引项扫描，以防止幻读。

- 索引上的等值查询(若业务sql执行时命中的是唯一索引)，给不存在的记录加锁时, Next-Key Lock 临键锁 优化为 Gap Lock 间隙锁，改使 Gap Lock 间隙锁来锁住一部分索引项以防止幻读  
- 索引上的等值查询(若业务sql执行时命中的是非唯一普通索引)，向右遍历时最后一个值不满足查询需求时，Next-Key Lock 临键锁退化为 Gap Lock 间隙锁，以防止幻读。 
- 索引上的范围查询(若业务sql执行时命中的是唯一索引)--会访问到不满足条件的第一个值为止，以防止幻读。

注意：Gap Lock 间隙锁唯一目的是防止其他事务插入间隙。间隙锁可以共存，所以一个事务执行业务SQL采用的间隙锁不会阻止另一个事务执行业务SQL在同一间隙上采用间隙锁。



#### 2). 示例演示



A. 索引上的等值查询(若业务sql执行时命中的是唯一索引)，给不存在的记录加锁时 ,  Next-Key Lock 临键锁 优化为 Gap Lock 间隙锁，改使 Gap Lock 间隙锁来锁住一部分索引项以防止幻读 。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660286144027-0d6cb55e-368c-4360-8eca-88bd49bb08c4.png)



B. 索引上的等值查询(若业务sql执行时命中的是非唯一普通索引)，向右遍历时直到最后一个值不满足查询需求时，Next-Key Lock 临键锁退化为 Gap Lock 间隙锁，以防止幻读。 

- 分析一下上面这句话：

我们知道InnoDB的B+树索引，叶子节点是有序的双向链表。 

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660276159893-8de42783-dd7b-4ab0-b5f6-53f55e9e7cc4.png)

假如我们编写的业务SQL  select  *  from t_xx where age = 18 lock in share mode; （ps：假设已经给age 字段建立了非唯一普通索引），

这条业务SQL要根据上面这个二级索引示意图中查询索引项值为18的数据，并加上了共享锁，InnoDB底层只锁定索引项值为18这一行数据就可以了吗？ 

并不是，虽然这条业务SQL执行时命中了索引，但因为age 字段是非唯一普通索引，意味着这个二级索引示意图中可能有多个索引项值为18的行数据存在，所以对于这条业务SQL被执行时，InnoDB底层在加锁时会继续往右查找，直到找到一个不满足条件的索引项值（上图案例中也就是值为29的行数据）。此时InnoDB底层会对索引项值为18加 Next-Key Lock 临键锁，并对索引项值为29之前的所有间隙加锁（所谓的 Gap Lock） 。以防止幻读。 



- 编写业务SQL演示上面这句话：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660289136309-a77803b7-06db-4a28-b100-50434f17d23b.png)

我的理解：

| LOCK_MODE     | LOCK_DATA | 锁范围           |
| ------------- | --------- | ---------------- |
| S             | 3,3       | 3 索引项的临建锁 |
| S,REC_NOT_GAP | 3         | 3 索引项的行锁   |
| S,GAP         | 7,7       | 7 索引项的间隙锁 |

select * from stu where age = 3 lock in share mode;这条业务SQL被执行时，

InnoDB底层会对age索引项值为 3的索引项加 Next-Key Lock 临键锁，锁住age 索引项值为3和3之前的所有间隙，

然后InnoDB底层在加锁时会继续往右查找，直到找到一个不满足条件的值（上图案例中也就是age索引项值为7的行数据），会对age索引项值为7的索引项加 Gap Lock 间隙锁，锁住age 索引项值为7（不包含索引项7）之前的所有间隙。以防止幻读。 





C. 索引上的范围查询(若业务sql执行时命中的是唯一索引)--会访问到不满足条件的第一个值为止。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660286843662-3cf26a5b-86a6-45ca-ac14-60678f821da2.png)

查询的条件为id>=19，并添加共享锁。 此时我们可以根据数据库表中现有的数据，将数据分为三个部分： 

[19] 

(19,25] 

(25,+∞] 

所以数据库数据在加锁是，就是将索引项值为19加了行锁，索引项值为25的临键锁（包含25及25之前的间隙），索引项值为正无穷的临键锁(包含正无穷及之前的间隙)。

我的理解：

| LOCK_MODE     | LOCK_DATA              | 锁范围                            |
| ------------- | ---------------------- | --------------------------------- |
| S,REC_NOT_GAP | 19                     | 19 索引项的行锁                   |
| S             | supermum pseudo-record | 正无穷 索引项的临键锁，包含正无穷 |
| S             | 25                     | 25 索引项的临建锁                 |



#  6. InnoDB引擎



## 6.1 逻辑存储结构



InnoDB的逻辑存储结构如下图所示:

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660300289245-19db21aa-ef65-4cda-a30d-8c6018a067e6.png)



1). 表空间

表空间是InnoDB存储引擎逻辑结构的最高层， 如果用户启用了参数 innodb_file_per_table(在8.0版本中默认开启) ，则每张表都会有一个表空间（xxx.ibd），一个mysql实例可以对应多个表空间，用于存储记录、索引等数据。



2). 段

段，分为数据段（Leaf node segment）、索引段（Non-leaf node segment）、回滚段（Rollback segment），InnoDB是索引组织表，数据段就是B+树的叶子节点， 索引段即为B+树的非叶子节点。段用来管理多个Extent（区）。 



3). 区

区，表空间的单元结构，每个区的大小为1M。 默认情况下， InnoDB存储引擎页大小为16K， 即一个区中一共有64个连续的页。



4). 页

页，是InnoDB 存储引擎磁盘管理的最小单元，每个页的大小默认为 16KB。为了保证页的连续性，InnoDB 存储引擎每次从磁盘申请 4-5 个区。



5). 行

行，InnoDB 存储引擎数据是按行进行存放的。

在行中，默认有两个隐藏字段： 

- Trx_id：每次对某条记录进行改动时，都会把对应的事务id赋值给trx_id隐藏列。 
- Roll_pointer：每次对某条引记录进行改动时，都会把旧的版本写入到undo日志中，然后这个隐藏列就相当于一个指针，可以通过它来找到该记录修改前的信息。



## 6.2 架构 



### 6.2.1 概述



MySQL5.5 版本开始，默认使用InnoDB存储引擎，它擅长事务处理，具有崩溃恢复特性，在日常开发中使用非常广泛。下面是InnoDB架构图，左侧为内存结构，右侧为磁盘结构。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660300446046-d44c606b-3b18-43c0-a028-8acaf90f86d2.png)



### 6.2.2 内存结构

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660300480955-52477683-ab87-4c30-819f-1d182e0e05b4.png)

在左侧的内存结构中，主要分为这么四大块儿： Buffer Pool、Change Buffer、Adaptive Hash Index、Log Buffer。 接下来介绍一下这四个部分。

#### 1). Buffer Pool

InnoDB存储引擎基于磁盘文件存储，访问物理硬盘和在内存中进行访问，速度相差很大，为了尽可能弥补这两者之间的I/O效率的差值，就需要把经常使用的数据加载到缓冲池中，避免每次访问都进行磁盘I/O。 

在InnoDB的缓冲池中不仅缓存了索引页和数据页，还包含了undo页、插入缓存、自适应哈希索引以及InnoDB的锁信息等等。

缓冲池 Buffer Pool，是主内存中的一个区域，里面可以缓存磁盘上经常操作的真实数据，在执行增删改查操作时，先操作缓冲池中的数据（若缓冲池没有数据，则从磁盘加载并缓存），然后再以一定频率刷新到磁盘，从而减少磁盘IO，加快处理速度。

缓冲池以Page页为单位，底层采用链表数据结构管理Page。根据状态，将Page分为三种类型：

-  free page：空闲page，未被使用。 
- clean page：被使用page，数据没有被修改过。 
- dirty page：脏页，被使用page，数据被修改过，也中数据与磁盘的数据产生了不一致。



在专用服务器上，通常将多达80％的物理内存分配给缓冲池 。参数设置： show variables like 'innodb_buffer_pool_size';

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660300592617-7ac76bc9-b859-4b67-8163-add0c8c6a4d4.png)



#### 2). Change Buffer

Change Buffer，更改缓冲区（针对于非唯一二级索引页），在执行DML语句时，如果这些数据Page没有在Buffer Pool中，不会直接操作磁盘，而会将数据变更存在更改缓冲区 Change Buffer中，在未来数据被读取时，再将数据合并恢复到Buffer Pool中，再将合并后的数据刷新到磁盘中。

Change Buffer的意义是什么呢? 

先来看一幅图，这个是二级索引的结构图：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660300636489-de8b6072-25e9-445c-a083-c19054a0f6da.png)

与聚集索引不同，二级索引通常是非唯一的，并且以相对随机的顺序插入二级索引。同样，删除和更新可能会影响索引树中不相邻的二级索引页，如果每一次都操作磁盘，会造成大量的磁盘IO。有了ChangeBuffer之后，我们可以在缓冲池中进行合并处理，减少磁盘IO。



#### 3). Adaptive Hash Index

自适应hash索引，用于优化对Buffer Pool数据的查询。MySQL的innoDB引擎中虽然没有直接支持hash索引，但是给我们提供了一个功能就是这个自适应hash索引。因为前面我们讲到过，hash索引在进行等值匹配时，一般性能是要高于B+树的，因为hash索引一般只需要一次IO即可，而B+树，可能需 要几次匹配，所以hash索引的效率要高，但是hash索引又不适合做范围查询、模糊匹配等。

InnoDB存储引擎会监控对表上各索引页的查询，如果观察到在特定的条件下hash索引可以提升速度，则建立hash索引，称之为自适应hash索引。



**自适应哈希索引，无需人工干预，是系统根据情况自动完成。**

参数： adaptive_hash_index



#### 4). Log Buffer

Log Buffer：日志缓冲区，用来保存要写入到磁盘中的log日志数据（redo log 、undo log），默认大小为 16MB，日志缓冲区的日志会定期刷新到磁盘中。如果需要更新、插入或删除许多行的事务，增加日志缓冲区的大小可以节省磁盘 I/O。

参数: 

innodb_log_buffer_size：缓冲区大小 

innodb_flush_log_at_trx_commit：日志刷新到磁盘时机，取值主要包含以下三个：

1: 日志在每次事务提交时写入并刷新到磁盘，默认值。 

0: 每秒将日志写入并刷新到磁盘一次。

2: 日志在每次事务提交后写入，并每秒刷新到磁盘一次。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660300769571-a659c60c-54b8-482a-955f-7c6ae80d45aa.png)



### 6.2.3 磁盘结构



接下来，再来看看InnoDB体系结构的右边部分，也就是磁盘结构：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660300808453-fadb9930-01fc-4b6a-abf7-de563d512ac3.png)

#### 1). System Tablespace

系统表空间是更改缓冲区的存储区域。如果表是在系统表空间而不是每个表文件或通用表空间中创建的，它也可能包含表和索引数据。(在MySQL5.x版本中还包含InnoDB数据字典、 undo log 等)

参数：innodb_data_file_path

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660300864020-c6d964d8-21b1-4f5b-bc8a-b663fd2978c9.png)

系统表空间，默认的文件名叫 ibdata1。



#### 2). File-Per-Table Tablespaces

如果开启了innodb_file_per_table开关 ，则每个表的文件表空间包含单个InnoDB表的数据和索引 ，并存储在文件系统上的单个数据文件中。

开关参数：innodb_file_per_table ，该参数默认开启。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660300903328-dd764bdc-6e76-4b6f-a3de-5cb6c79b201d.png)

那也就是说，我们没创建一个表，都会产生一个表空间文件，如图：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660300923785-854b2552-55c2-4505-879e-c09a031cd301.png)



#### 3). General Tablespaces

通用表空间，需要通过 CREATE TABLESPACE 语法创建通用表空间，在创建表时，可以指定该表空间。



A. 创建表空间

```sql
CREATE TABLESPACE ts_name ADD DATAFILE 'file_name' ENGINE = engine_name;
```



B. 创建表时指定表空间

```sql
CREATE TABLE xxx ... TABLESPACE ts_name;
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660300991198-03006340-cb95-4e9c-ab27-90a0b12215e3.png)



#### 4). Undo Tablespaces 

撤销表空间，MySQL实例在初始化时会自动创建两个默认的undo表空间（初始大小16M），用于存储undo log日志。



#### 5). Temporary Tablespaces 

InnoDB 使用会话临时表空间和全局临时表空间。存储用户创建的临时表等数据。



#### 6). Doublewrite Buffer Files

双写缓冲区，innoDB引擎将数据页从Buffer Pool刷新到磁盘前，先将数据页写入双写缓冲区文件中，便于系统异常时恢复数据。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660301055387-8a9de6cb-577b-4c13-9fcd-e7c1800aba84.png)



#### 7). Redo Log

重做日志，是用来实现事务的持久性。该日志文件由两部分组成：重做日志缓冲（redo log buffer）以及重做日志文件（redo log）,前者是在内存中，后者在磁盘中。当事务提交之后会把所有修改信息都会存到该日志中, 用于在刷新脏页到磁盘时,发生错误时, 进行数据恢复使用。

以循环方式写入重做日志文件，涉及两个文件：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660301092476-cb540bb6-5223-4ff0-9ec7-97384274d5f2.png)



前面我们介绍了InnoDB的内存结构，以及磁盘结构，那么内存中我们所更新的数据，又是如何到磁盘中的呢？ 此时，就涉及到一组后台线程，接下来，就来介绍一些InnoDB中涉及到的后台线程。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660301123743-7d0fb3c1-1aaf-4cad-8cbf-b09257d56b71.png)



### 6.2.4 后台线程

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660301158853-ec0db0f8-c521-4759-a283-f3d26855553a.png)

在InnoDB的后台线程中，分为4类，分别是：Master Thread 、IO Thread、Purge Thread、Page Cleaner Thread。



#### 1). Master Thread

核心后台线程，负责调度其他线程，还负责将缓冲池中的数据异步刷新到磁盘中, 保持数据的一致性，还包括脏页的刷新、合并插入缓存、undo页的回收 。



#### 2). IO Thread

在InnoDB存储引擎中大量使用了AIO来处理IO请求, 这样可以极大地提高数据库的性能，而IOThread主要负责这些IO请求的回调。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660301221237-593478bf-b301-4855-a8be-d90f61e73a7f.png)

我们可以通过以下的这条指令，查看到InnoDB的状态信息，其中就包含IO Thread信息。

```sql
show engine innodb status \G;
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660301289112-93a4583e-90ea-4e74-ad80-8180bcce814e.png)



#### 3). Purge Thread

主要用于回收事务已经提交了的undo log，在事务提交之后，undo log可能不用了，就用它来回收。



#### 4). Page Cleaner Thread

协助 Master Thread 刷新脏页到磁盘的线程，它可以减轻 Master Thread 的工作压力，减少阻塞。



### 6.3 事务原理 



#### 6.3.1 事务基础



##### 1). 事务 

事务 是一组操作的集合，它是一个不可分割的工作单位，事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求，即这些操作要么同时成功，要么同时失败。



##### 2). 特性

- 原子性（Atomicity）：事务是不可分割的最小操作单元，要么全部成功，要么全部失败。 
- 一致性（Consistency）：事务完成时，必须使所有的数据都保持一致状态。 
- 隔离性（Isolation）：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行。 
- 持久性（Durability）：事务一旦提交或回滚，它对数据库中的数据的改变就是永久的。

那实际上，我们研究事务的原理，就是研究MySQL的InnoDB引擎是如何保证事务的这四大特性的。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660301389376-38f2725b-03a4-446f-b8ee-1e3a6c6b81b8.png)

而对于这四大特性，实际上分为两个部分。 其中的原子性、一致性、持久化，实际上是由InnoDB中的两份日志来保证的，一份是redo log日志，一份是undo log日志。 而持久性是通过数据库的锁，加上MVCC来保证的。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660301418690-41aa8f92-7e29-414e-a6ab-b949aa7f5a65.png)

我们在讲解事务原理的时候，主要就是来研究一下 redo log ， undo log 以及MVCC。 



#### 6.3.2 redo log

重做日志，记录的是事务提交时数据页的物理修改，是用来实现事务的持久性。

该日志文件由两部分组成：重做日志缓冲（redo log buffer）以及重做日志文件（redo log file）,前者是在内存中，后者在磁盘中。当事务提交之后会把所有修改信息都存到该日志文件中, 用 于在刷新脏页到磁盘,发生错误时, 进行数据恢复使用。 

如果没有 redo log ，可能会存在什么问题的？ 我们一起来分析一下。



我们知道，在InnoDB引擎中的内存结构中，主要的内存区域就是缓冲池，在缓冲池中缓存了很多的数据页。 当我们在一个事务中，执行多个增删改的操作时，InnoDB引擎会先操作缓冲池中的数据，如果缓冲区没有对应的数据，会通过后台线程将磁盘中的数据加载出来，存放在缓冲区中，然后将缓冲池中的数据修改，修改后的数据页我们称为脏页。 而脏页则会在一定的时机，通过后台线程刷新到磁盘中，从而保证缓冲区与磁盘的数据一致。 而缓冲区的脏页数据并不是实时刷新的，而是一段时间之后将缓冲区的数据刷新到磁盘中，假如刷新到磁盘的过程出错了，而提示给用户事务提交成功，而数据却没有持久化下来，这就出现问题了，没有保证事务的持久性。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660301509230-7349650e-6dc4-4efc-ac56-4d87cd9d94d6.png)

那么，如何解决上述的问题呢？ 在InnoDB中提供了一份日志 redo log，接下来我们再来分析一下，通过 redo log 如何解决这个问题。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660301545347-2ce6cfbb-5fee-4147-a00b-99403d076de3.png)

有了 redo log 之后，当对缓冲区的数据进行增删改之后，会首先将操作的数据页的变化，记录在redo log buffer中。在事务提交时，会将redo log buffer中的数据刷新到redo log磁盘文件中。过一段时间之后，如果刷新缓冲区的脏页到磁盘时，发生错误，此时就可以借助于redo log进行数据恢复，这样就保证了事务的持久性。 而如果脏页成功刷新到磁盘 或 或者涉及到的数据已经落盘，此时 redo log 就没有作用了，就可以删除了，所以存在的两个 redo log 文件是循环写的。

那为什么每一次提交事务，要刷新redo log 到磁盘中呢，而不是直接将buffer pool中的脏页刷新到磁盘呢 ?

因为在业务操作中，我们操作数据一般都是随机读写磁盘的，而不是顺序读写磁盘。 而redo log在往磁盘文件中写入数据，由于是日志文件，所以都是顺序写的。顺序写的效率，要远大于随机写。 这种先写日志的方式，称之为 WAL（Write-Ahead Logging）。



#### 6.3.3 undo log

回滚日志，用于记录数据被修改前的信息 , 作用包含两个 : 提供回滚(保证事务的原子性) 和MVCC(多版本并发控制) 。

undo log和redo log记录物理日志不一样，它是逻辑日志。可以认为当delete一条记录时，undo log中会记录一条对应的insert记录，反之亦然，当update一条记录时，它记录一条对应相反的 update记录。当执行rollback时，就可以从undo log中的逻辑记录读取到相应的内容并进行回滚。

Undo log销毁：undo log在事务执行时产生，事务提交时，并不会立即删除undo log，因为这些日志可能还用于MVCC。

Undo log存储：undo log采用段的方式进行管理和记录，存放在前面介绍的 rollback segment回滚段中，内部包含1024个undo log segment。 



### 6.4 MVCC 



#### 6.4.1 基本概念



##### 1). 当前读

读取的是记录的最新版本，读取时还要保证其他并发事务不能修改当前记录，会对读取的记录进行加锁。对于我们日常的操作，如：select ... lock in share mode(共享锁)，select ... for update、update、insert、delete(排他锁)都是一种当前读。

测试：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660301709509-d8d8caa8-0a02-4acb-b0c8-040dcb02c0d0.png)

在测试中我们可以看到，即使是在默认的RR隔离级别下，事务A中依然可以读取到事务B最新提交的内容，因为在查询语句后面加上了 lock in share mode 共享锁，此时是当前读操作。当然，当我们加排他锁的时候，也是当前读操作。



##### 2). 快照读

简单的select（不加锁）就是快照读，快照读，读取的是记录数据的可见版本，有可能是历史数据，不加锁，是非阻塞读。 

- Read Committed：每次select，都生成一个快照读。 
- Repeatable Read：开启事务后第一个select语句才是快照读的地方。 
- Serializable：快照读会退化为当前读。

测试:

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660301795900-5ecb466a-27ee-4d8c-ae17-b16dbf6efa03.png)

在测试中,我们看到即使事务B提交了数据,事务A中也查询不到。 原因就是因为普通的select是快照读，而在当前默认的RR隔离级别下，开启事务后第一个select语句才是快照读的地方，后面执行相同的select语句都是从快照中获取数据，可能不是当前的最新数据，这样也就保证了可重复读。



##### 3). MVCC多版本并发控制

全称 Multi-Version Concurrency Control，多版本并发控制。指维护一个数据的多个版本，使得读写操作没有冲突，快照读为MySQL实现MVCC提供了一个非阻塞读功能。MVCC的具体实现，还需要依赖于数据库记录中的三个隐式字段、undo log日志、readView。

接下来，我们再来介绍一下InnoDB引擎的表中涉及到的隐藏字段 、 undo log  以及 readview，从而来分析MVCC的原理。

#### 6.4.2 隐藏字段



##### 6.4.2.1 介绍



![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660301863177-b173c32a-5b5d-43a2-87c9-3b9635280c7c.png)

当我们创建了上面的这张表，我们在查看表结构的时候，就可以显式的看到这三个字段。 实际上除了这三个字段以外，InnoDB还会自动的给我们添加三个隐藏字段及其含义分别是：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660301887012-e5872c27-8f2e-4263-b847-0d5589a4911f.png)

而上述的前两个字段是肯定会添加的， 是否添加最后一个字段DB_ROW_ID，得看当前表有没有主键，如果有主键，则不会添加该隐藏字段。



##### 6.4.2.2 测试



1). 查看有主键的表 stu

进入服务器中的 /var/lib/mysql/itcast/ , 查看stu的表结构信息, 通过如下指令:

```sql
ibd2sdi stu.ibd
```

查看到的表结构信息中，有一栏 columns，在其中我们会看到处理我们建表时指定的字段以外，还有额外的两个字段 分别是：DB_TRX_ID 、 DB_ROLL_PTR ，因为该表有主键，所以没有DB_ROW_ID隐藏字段。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660301976742-4d71fadd-8d38-48e2-9213-d34c8013842f.png)

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660301992239-ffd868a7-e695-47b3-8fd6-8ebd64516e2b.png)



2). 查看没有主键的表 employee 

建表语句：

```sql
create table employee (id int , name varchar(10));
```

此时，我们再通过以下指令来查看表结构及其其中的字段信息：

```sql
ibd2sdi employee.ibd
```

查看到的表结构信息中，有一栏 columns，在其中我们会看到处理我们建表时指定的字段以外，还有额外的三个字段 分别是：DB_TRX_ID 、 DB_ROLL_PTR 、DB_ROW_ID，因为employee表是没有指定主键的。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660368970559-90efb768-e60e-4309-a1ea-b86a7295aa90.png)

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660368980863-8393cbe7-3549-4830-a39e-73ee2c9b4010.png)

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660368989344-cc0d1b69-9b90-475d-bc3c-c4b6891d2e65.png)





#### 6.4.3 undo log



##### 6.4.3.1 介绍



回滚日志，在insert、update、delete的时候产生的便于数据回滚的日志。 

当insert的时候，产生的undo log日志只在回滚时需要，在事务提交后，可被立即删除。 

而update、delete的时候，产生的undo log日志不仅在回滚时需要，在快照读时也需要，不会立即被删除。



##### 6.4.3.2 版本链



有一张表原始数据为：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660368928152-e4c1601f-7962-4998-9dd7-3be43d91b247.png)

DB_TRX_ID : 代表最近修改事务ID，记录插入这条记录或最后一次修改该记录的事务ID，是自增的。 

DB_ROLL_PTR ： 由于这条数据是才插入的，没有被更新过，所以该字段值为null。

然后，假设有四个并发事务同时在访问这张表。

A. 第一步 

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660368879649-f6b759ac-0525-43f4-80c1-d355592d293b.png)

当事务2执行第一条修改语句时，会记录undo log日志，记录数据变更之前的样子; 然后更新记录，并且记录本次操作的事务ID，回滚指针，回滚指针用来指定如果发生回滚，回滚到哪一个版本。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660369068883-77f79413-56ff-448c-8815-5e34fad82a09.png)



B.第二步

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660369085988-9d1551f5-b19d-4d61-a1ff-ce468f137d2b.png)

当事务3执行第一条修改语句时，也会记录undo log日志，记录数据变更之前的样子; 然后更新记录，并且记录本次操作的事务ID，回滚指针，回滚指针用来指定如果发生回滚，回滚到哪一个版本。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660369116050-ae70410f-5f2f-4f2d-912b-2edd2f7c73c3.png)



C. 第三步

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660369162349-1152d159-f9e5-4c4b-a7e1-aed30da7e759.png)

当事务4执行第一条修改语句时，也会记录undo log日志，记录数据变更之前的样子; 然后更新记录，并且记录本次操作的事务ID，回滚指针，回滚指针用来指定如果发生回滚，回滚到哪一个版本。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660369181020-b97602cf-c286-416b-a021-86cfcdc4c6ad.png)





最终我们发现，不同事务或相同事务对同一条记录进行修改，会导致该记录的 undo log 生成一条记录版本链表，链表的头部是最新的旧记录，链表尾部是最早的旧记录。



#### 6.4.4 readview

ReadView（读视图）是快照读SQL执行时MVCC提取数据的依据，记录并维护系统当前活跃的事务（未提交的）id。 

ReadView中包含了四个核心字段：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660369295668-4c1e7f46-568c-49b8-9cf6-87f2afac2439.png)

而在readview中就规定了版本链数据的访问规则：trx_id 代表当前 undo log 版本链对应事务ID。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660369360555-6cc5865c-9d31-4a5d-83c3-b443e0ea09cb.png)

不同的隔离级别，生成ReadView的时机不同： 

- READ COMMITTED ：在事务中每一次执行快照读时生成ReadView。 
- REPEATABLE READ：仅在事务中第一次执行快照读时生成ReadView，后续复用该ReadView。 



#### 6.4.5 MVCC的实现原理



##### 6.4.5.1 RC隔离级别



RC隔离级别下，在事务中每一次执行快照读时生成ReadView。

我们就来分析事务5中，两次快照读读取数据，是如何获取数据的?

在事务5中，查询了两次id为30的记录，由于隔离级别为Read Committed，所以每一次进行快照读都会生成一个ReadView，那么两次生成的ReadView如下。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660369433679-656ecaad-0037-4870-9e41-cfd7c76ecf04.png)

那么这两次快照读在获取数据时，就需要根据所生成的ReadView以及ReadView的版本链访问规则，到 undo log 版本链中匹配数据，最终决定此次快照读返回的数据。



A. 先来看第一次快照读具体的读取过程：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660369569727-60e86def-87d7-4b47-ae97-ab7531364805.png)

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660369682851-568c2949-e316-480c-aa34-eeced12dbb25.png)

在进行匹配时，会从undo log的版本链，从上到下进行挨个匹配：

- 先匹配![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660369701401-80a0c7c3-d854-454b-a0cd-d869c0388dd0.png)这条记录，这条记录对应的trx_id为4，也就是将4带入右侧的匹配规则中。①不满足②不满足③不满足④也不满足，都不满足，则继续匹配undo log版本链的下一条。
- 再匹配第二条![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660369728037-4c987704-664f-41fc-9fbe-b9e104c985f0.png)，这条记录对应的trx_id为3，也就是将3带入右侧的匹配规则中。①不满足②不满足③不满足④也不满足，都不满足，则继续匹配undo log版本链的下一条。
- 再匹配第三条![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660369746663-b90b52df-7f43-48ae-9ff5-7820e27247b6.png)，这条记录对应的trx_id为2，也就是将2带入右侧的匹配规则中。①不满足②满足终止匹配，此次快照读，返回的数据就是版本链中记录的这条数据。



B. 再来看第二次快照读具体的读取过程:

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660369781847-bda43c79-ec88-418d-a9b7-98598d852808.png)

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660369789537-525b6895-9f8b-4662-b1a5-581ace23d2db.png)

在进行匹配时，会从undo log的版本链，从上到下进行挨个匹配：

- 先匹配![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660369806413-931eb1aa-5d00-43e4-a0d1-ac2619661cd4.png)这条记录，这条记录对应的trx_id为4，也就是将4带入右侧的匹配规则中。①不满足②不满足③不满足④也不满足，都不满足，则继续匹配undo log版本链的下一条。
- 再匹配第二条![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660369822643-62553532-301f-43c7-91ac-cff934765924.png)，这条记录对应的trx_id为3，也就是将3带入右侧的匹配规则中。①不满足②满足。终止匹配，此次快照读，返回的数据就是版本链中记录的这条数据。



##### 6.4.5.2 RR隔离级别 

RR隔离级别下，仅在事务中第一次执行快照读时生成ReadView，后续复用该ReadView。而RR 是可重复读，在一个事务中，执行两次相同的select语句，查询到的结果是一样的。 

那MySQL是如何做到可重复读的呢?     我们简单分析一下就知道了

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660369890846-4ed5a8d2-b9ed-4c9b-8c22-2b88698f7cfe.png)

我们看到，在RR隔离级别下，只是在事务中第一次快照读时生成ReadView，后续都是复用该ReadView，那么既然ReadView都一样，ReadView的版本链匹配规则也一样，那么最终快照读返回的结果也是一样的。

所以呢，MVCC的实现原理就是通过InnoDB表的隐藏字段、 undo log  版本链、ReadView来实现的。而MVCC机制 + InnoDB锁，则实现了事务的隔离性。而一致性则是由 redo log  与 undo log 保证的。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660370006361-2f0baf7b-1712-4687-97ef-bf3056065acd.png)



# 7. MySQL管理



## 7.1MySQL系统自带的数据库



Mysql数据库安装完成后，自带了一下四个数据库，具体作用如下：

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660370341750-2b5738bc-c29b-4ef0-8c1f-9285d09f4e08.png)



## 7.2 bin目录下常用的客户端工具



### 7.2.1 mysql



该mysql不是指mysql服务实例，而是指mysql的客户端工具。

```shell
语法 ：
    mysql [options] [database] 

选项 ：
    -u, --user=name    #指定用户名 
    -p, --password[=name]     #指定密码 
    -h, --host=name     #指定服务器IP或域名 
    -P, --port=port     #指定连接端口 
    -e, --execute=name     #执行SQL语句并退出
```

-e选项可以在Mysql客户端执行SQL语句，而不用连接到MySQL数据库再执行，对于一些批处理脚本，这种方式尤其方便。

示例：

```shell
mysql -uroot –p123456 db01 -e "select * from stu"; 1
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660370583896-354c330d-a03e-4a08-bd8b-1ed7cc71db01.png)



### 7.2.2 mysqladmin



mysqladmin 是一个执行管理操作的客户端程序。可以用它来检查服务器的配置和当前状态、创建并删除数据库等。

```shell
通过帮助文档查看选项： 
        mysqladmin --help
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660370667100-60454c78-1893-4dd4-a312-5072917fb2d0.png)

```shell
语法:
    mysqladmin [options] command ... 
选项:
    -u, --user=name #指定用户名 
    -p, --password[=name] #指定密码 
    -h, --host=name #指定服务器IP或域名 
    -P, --port=port #指定连接端口
```

示例：

```shell
mysqladmin -uroot –p1234 drop 'test01'; 
mysqladmin -uroot –p1234 version;
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660370749503-4a050097-eeed-460e-b3b7-300c73092e9a.png)



**7.2.3mysqlbinlog** 



由于服务器生成的二进制日志文件以二进制格式保存，所以如果想要检查这些文本的文本格式，就会使用到mysqlbinlog 日志管理工具。

```shell
语法 ：
      mysqlbinlog [options] log-files1 log-files2 ... 

选项 ：
      -d, --database=name 指定数据库名称，只列出指定的数据库相关操作。 
      -o, --offset=# 忽略掉日志中的前n行命令。 
      -r,--result-file=name 将输出的文本格式日志输出到指定文件。 
      -s, --short-form 显示简单格式， 省略掉一些信息。 
      --start-datatime=date1 --stop-datetime=date2 指定日期间隔内的所有日志。 
      --start-position=pos1 --stop-position=pos2 指定位置间隔内的所有日志。
```

示例:

A. 查看 binlog.000008这个二进制文件中的数据信息

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660370949332-b2658be3-df4b-47d7-b029-462f6cfb1ee4.png)

上述查看到的二进制日志文件数据信息量太多了，不方便查询。我们可以加上一个参数-s 来显示简单格式。

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660370969671-f7e32b36-397b-4469-be57-bc944c521139.png)



### 7.2.4 mysqlshow 



mysqlshow 客户端对象查找工具，用来很快地查找存在哪些数据库、数据库中的表、表中的列或者索引。

```shell
语法 ：
      mysqlshow [options] [db_name [table_name [col_name]]] 
选项 ：
      --count 显示数据库及表的统计信息（数据库，表 均可以不指定） 
      -i 显示指定数据库或者指定表的状态信息 

示例：

      #查询test库中每个表中的字段书，及行数 
      mysqlshow -uroot -p2143 test --count 

      #查询test库中book表的详细情况 
      mysqlshow -uroot -p2143 test book --count
```

示例：

A. 查询每个数据库的表的数量及表中记录的数量 

```shell
mysqlshow -uroot -p1234 --count 
```

![img](https://cdn.nlark.com/yuque/0/2022/png/12843528/1660371082336-40894e24-67e7-4ec8-9f6b-f53fecced881.png)



B. 查看数据库db01的统计信息 

```shell
mysqlshow -uroot -p1234 db01 --count
```



# 1. MySQL主从复制

MySQL数据库默认是支持主从复制的，不需要借助于其他的技术，我们只需要在数据库中简单的配置即可。接下来，我们就从以下的几个方面，来介绍一下主从复制：



## 1.1 介绍

MySQL主从复制是一个异步的复制过程，底层是基于Mysql数据库自带的 **二进制日志** 功能。就是一台或多台MySQL数据库（slave，即**从库**）从另一台MySQL数据库（master，即**主库**）进行日志的复制，然后再解析日志并应用到自身，最终实现 **从库** 的数据和 **主库** 的数据保持一致。MySQL主从复制是MySQL数据库自带功能，无需借助第三方工具。

> **二进制日志：** 
>
> ​	二进制日志（BINLOG）记录了所有的 DDL（数据定义语言）语句和 DML（数据操纵语言）语句，但是不包括数据查询语句。此日志对于灾难时的数据恢复起着极其重要的作用，MySQL的主从复制， 就是通过该binlog实现的。默认MySQL是未开启该日志的。



**MySQL的主从复制原理如下：** 

![image-20210825110417975](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262136467.png) 



**MySQL复制过程分成三步：**

1). MySQL master 将数据变更写入二进制日志( binary log)

2). slave将master的binary log拷贝到它的中继日志（relay log）

3). slave重做中继日志中的事件，将数据变更反映它自己的数据





## 1.2 搭建

### 1.2.1 准备工作

提前准备两台服务器，并且在服务器中安装MySQL，服务器的信息如下：

| 数据库 | IP              | 数据库版本 |
| ------ | --------------- | ---------- |
| Master | 192.168.200.200 | 5.7.25     |
| Slave  | 192.168.200.201 | 5.7.25     |



**并在两台服务器上做如下准备工作:** 

1). 防火墙开放3306端口号

```
firewall-cmd --zone=public --add-port=3306/tcp --permanent

firewall-cmd --zone=public --list-ports
```

![image-20210825124800430](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262136496.png) 



2). 并将两台数据库服务器启动起来：

```
systemctl start mysqld
```

登录MySQL，验证是否正常启动

![image-20210825111414157](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262136537.png) 





#### 1.2.2 主库配置

> 服务器： 192.168.200.200



**1). 修改Mysql数据库的配置文件/etc/my.cnf**

在最下面增加配置: 

```
log-bin=mysql-bin   #[必须]启用二进制日志
server-id=200       #[必须]服务器唯一ID(唯一即可)
```

![image-20210825115719668](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262142319.png) 





**2). 重启Mysql服务**

执行指令： 

```
 systemctl restart mysqld
```

![image-20210825115853116](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262136664.png) 





**3). 创建数据同步的用户并授权**

登录mysql，并执行如下指令，创建用户并授权：

```sql
GRANT REPLICATION SLAVE ON *.* to 'xiaoming'@'%' identified by 'Root@123456';
```

==注：上面SQL的作用是创建一个用户 xiaoming ，密码为 Root@123456 ，并且给xiaoming用户授予REPLICATION SLAVE权限。常用于建立复制时所需要用到的用户权限，也就是slave必须被master授权具有该权限的用户，才能通过该用户复制。==



> MySQL密码复杂程度说明: 
>
> ​	![image-20210825144818269](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262138808.png) 
>
> ​	目前mysql5.7默认密码校验策略等级为 MEDIUM , 该等级要求密码组成为: 数字、小写字母、大写字母 、特殊字符、长度至少8位



**4). 登录Mysql数据库，查看master同步状态**

执行下面SQL，记录下结果中**File**和**Position**的值

```
    show master status;
```

![image-20210825120355600](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262138312.png) 



==注：上面SQL的作用是查看Master的状态，执行完此SQL后不要再执行任何操作==







#### 1.2.3 从库配置

> 服务器： 192.168.200.201



**1). 修改Mysql数据库的配置文件/etc/my.cnf**

```
server-id=201 	#[必须]服务器唯一ID
```

![image-20210825125156597](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262142902.png) 



**2). 重启Mysql服务**

```
systemctl restart mysqld
```



**3). 登录Mysql数据库，设置主库地址及同步位置**

```
change master to master_host='192.168.136.130',master_user='ljw',master_password='Root@123456',master_log_file='mysql-bin.000001',master_log_pos=436;

start slave;
```

> 参数说明： 
>
> ​	A. master_host : 主库的IP地址
>
> ​	B. master_user : 访问主库进行主从复制的用户名(上面在主库创建的)
>
> ​	C. master_password : 访问主库进行主从复制的用户名对应的密码
>
> ​	D. master_log_file : 从哪个日志文件开始同步(上述查询master状态中展示的有)
>
> ​	E. master_log_pos : 从指定日志文件的哪个位置开始同步(上述查询master状态中展示的有)



**4). 查看从数据库的状态**

```
show slave status;
```

然后通过状态信息中的 Slave_IO_running 和 Slave_SQL_running 可以看出主从同步是否就绪，如果这两个参数全为Yes，表示主从同步已经配置完成。

 ![image-20210825142313382](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262142190.png)



> MySQL命令行技巧： 
>
> ​	\G : 在MySQL的sql语句后加上\G，表示将查询结果进行按列打印，可以使每个字段打印到单独的行。即将查到的结构旋转90度变成纵向；





## 1.3 测试

主从复制的环境,已经搭建好了,接下来,我们可以通过Navicat连接上两台MySQL服务器,进行测试。测试时，我们只需要在主库Master执行操作，查看从库Slave中是否将数据同步过去即可。

1). 在master中创建数据库itcast, 刷新slave查看是否可以同步过去

![image-20210825143518383](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262136193.png) 



2). 在master的itcast数据下创建user表, 刷新slave查看是否可以同步过去

![image-20210825143549689](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262136400.png) 



3). 在master的user表中插入一条数据, 刷新slave查看是否可以同步过去

![image-20210825143658516](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262139607.png) 







## 2. 读写分离案例

### 2.1 背景介绍

面对日益增加的系统访问量，数据库的吞吐量面临着巨大瓶颈。 对于同一时刻有大量并发读操作和较少写操作类型的应用系统来说，将数据库拆分为**主库**和**从库**，主库负责处理事务性的增删改操作，从库负责处理查询操作，能够有效的避免由数据更新导致的行锁，使得整个系统的查询性能得到极大的改善。

![image-20210825145647274](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262136214.png) 

通过读写分离,就可以降低单台数据库的访问压力, 提高访问效率，也可以避免单机故障。

主从复制的结构，我们在第一节已经完成了，那么我们在项目中，如何通过java代码来完成读写分离呢，如何在执行select的时候查询从库，而在执行insert、update、delete的时候，操作主库呢？这个时候，我们就需要介绍一个新的技术 ShardingJDBC。



### 2.2 ShardingJDBC介绍

Sharding-JDBC定位为轻量级Java框架，在Java的JDBC层提供的额外服务。 它使用客户端直连数据库，以jar包形式提供服务，无需额外部署和依赖，可理解为增强版的JDBC驱动，完全兼容JDBC和各种ORM框架。

使用Sharding-JDBC可以在程序中轻松的实现数据库读写分离。



Sharding-JDBC具有以下几个特点： 

1). 适用于任何基于JDBC的ORM框架，如：JPA, Hibernate, Mybatis, Spring JDBC Template或直接使用JDBC。

2). 支持任何第三方的数据库连接池，如：DBCP, C3P0, BoneCP, Druid, HikariCP等。

3). 支持任意实现JDBC规范的数据库。目前支持MySQL，Oracle，SQLServer，PostgreSQL以及任何遵循SQL92标准的数据库。



依赖: 

```xml
<dependency>
    <groupId>org.apache.shardingsphere</groupId>
    <artifactId>sharding-jdbc-spring-boot-starter</artifactId>
    <version>4.0.0-RC1</version>
</dependency>
```



### 2.3 数据库环境

在主库中创建一个数据库rw, 并且创建一张表， 该数据库及表结构创建完毕后会自动同步至从数据库，SQL语句如下： 

```SQL
create database rw default charset utf8mb4;

use rw;

CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `age` int(11) DEFAULT NULL,
  `address` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```



![image-20210825160658477](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262142220.png) 





### 2.4 初始工程导入

我们本案例主要是演示一下读写分离操作，对于基本的增删改查的业务操作，我们就不再去编写了，我们可以直接导入资料中提供的demo工程（rw_demo），在demo工程中，我们已经完成了user的增删改查操作，具体的工程结构如下： 

![image-20210825161155163](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262136124.png) 



### 2.5 读写分离配置

1). 在pom.xml中增加shardingJdbc的maven坐标

```xml
<dependency>
    <groupId>org.apache.shardingsphere</groupId>
    <artifactId>sharding-jdbc-spring-boot-starter</artifactId>
    <version>4.0.0-RC1</version>
</dependency>
```



2). 在application.yml中增加数据源的配置

```yml
spring:
  shardingsphere:
    datasource:
      names:
        master,slave
      # 主数据源
      master:
        type: com.alibaba.druid.pool.DruidDataSource
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://192.168.200.200:3306/rw?characterEncoding=utf-8
        username: root
        password: root
      # 从数据源
      slave:
        type: com.alibaba.druid.pool.DruidDataSource
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://192.168.200.201:3306/rw?characterEncoding=utf-8
        username: root
        password: root
    masterslave:
      # 读写分离配置
      load-balance-algorithm-type: round_robin #轮询
      # 最终的数据源名称
      name: dataSource
      # 主库数据源名称
      master-data-source-name: master
      # 从库数据源名称列表，多个逗号分隔
      slave-data-source-names: slave
    props:
      sql:
        show: true #开启SQL显示，默认false
```



配置解析: 

![image-20210825162910711](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262136133.png) 



3). 在application.yml中增加配置

```yml
spring:  
  main:
    allow-bean-definition-overriding: true
```

该配置项的目的,就是如果当前项目中存在同名的bean,后定义的bean会覆盖先定义的。



==如果不配置该项，项目启动之后将会报错：== 

![image-20210825163737687](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262139320.png) 

报错信息表明，在声明 org.apache.shardingsphere.shardingjdbc.spring.boot 包下的SpringBootConfiguration中的dataSource这个bean时出错, 原因是有一个同名的 dataSource 的bean在com.alibaba.druid.spring.boot.autoconfigure包下的DruidDataSourceAutoConfigure类加载时已经声明了。

![image-20210825164147056](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262139289.png) 

![image-20210825164227927](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262136993.png) 

而我们需要用到的是 shardingjdbc包下的dataSource，所以我们需要配置上述属性，让后加载的覆盖先加载的。





### 2.6 测试

我们使用shardingjdbc来实现读写分离，直接通过上述简单的配置就可以了。配置完毕之后，我们就可以重启服务，通过postman来访问controller的方法，来完成用户信息的增删改查，我们可以通过debug及日志的方式来查看每一次执行增删改查操作，使用的是哪个数据源，连接的是哪个数据库。

**1). 保存数据**

![image-20210825170601641](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262136325.png) 

控制台输出日志，可以看到操作master主库：

![image-20210825172748209](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262136238.png)  



**2). 修改数据**

![image-20210825171507059](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262140178.png) 

控制台输出日志，可以看到操作master主库：

![image-20210825172534790](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262136056.png)  



**3). 查询数据**

![image-20210825171609997](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262139144.png) 

控制台输出日志，可以看到操作slave主库： 

![image-20210825171623011](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262139397.png) 



**4). 删除数据**

![image-20210825172329600](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262140076.png) 

控制台输出日志，可以看到操作master主库：

![image-20210825172353414](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262139656.png) 


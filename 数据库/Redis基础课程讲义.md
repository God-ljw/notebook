# Redis基础

## 课程内容

- Redis入门
- Redis数据类型
- Redis常用命令
- 在Java中操作Redis



## 1. 前言

### 1.1 什么是Redis

Redis是一个基于**内存**的key-value结构数据库。Redis 是互联网技术领域使用最为广泛的存储中间件，它是「**Re**mote **Di**ctionary **S**ervice」的首字母缩写，也就是「远程字典服务」。

- [ ] 基于内存存储，读写性能高

![image-20210927090555559](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143359.png)

- [ ] 适合存储热点数据（热点商品、资讯、新闻）

![image-20210927090604994](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209241426653.png)

- [ ] 企业应用广泛

![image-20210927090612540](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209241426251.png)

### 1.2 使用Redis能做什么

- 数据缓存
- 消息队列
- 注册中心
- 发布订阅



## 2. Redis入门

### 2.1 Redis简介

Redis is an open source (BSD licensed), in-memory data structure store, used as a database, cache, and message broker. 翻译为：Redis是一个开源的内存中的数据结构存储系统，它可以用作：数据库、缓存和消息中间件。

官网：[https://redis.io](https://redis.io/)

Redis是用C语言开发的一个开源的高性能键值对(key-value)数据库，官方提供的数据是可以达到100000+的QPS（每秒内查询次数）。它存储的value类型比较丰富，也被称为结构化的NoSql数据库。

NoSql（Not Only SQL），不仅仅是SQL，泛指**非关系型数据库**。NoSql数据库并不是要取代关系型数据库，而是关系型数据库的补充。

关系型数据库(RDBMS)：

- Mysql
- Oracle
- DB2
- SQLServer

非关系型数据库(NoSql)：

- Redis
- Mongo db
- MemCached



### 2.2 Redis下载与安装

## 1.3.安装Redis

大多数企业都是基于Linux服务器来部署项目，而且Redis官方也没有提供Windows版本的安装包。因此课程中我们会基于Linux系统来安装Redis.

此处选择的Linux版本为CentOS 7.

### 1.3.1.依赖库

Redis是基于C语言编写的，因此首先需要安装Redis所需要的gcc依赖：

```sh
yum install -y gcc tcl
```



### 1.3.2.上传安装包并解压

然后将课前资料提供的Redis安装包上传到虚拟机的任意目录：

![](https://cdn.jsdelivr.net/gh/God-ljw/picbed@main/img3/202210141600038.png)

例如，我放到了/usr/local/src 目录：

![](https://cdn.jsdelivr.net/gh/God-ljw/picbed@main/img3/202210141600460.png)

解压缩：

```sh
tar -xzf redis-6.2.6.tar.gz
```

解压后：

![image-20211211080339076](https://cdn.jsdelivr.net/gh/God-ljw/picbed@main/img3/202210141600939.png)

进入redis目录：

```sh
cd redis-6.2.6
```



运行编译命令：

```sh
make && make install
```

如果没有出错，应该就安装成功了。



默认的安装路径是在 `/usr/local/bin`目录下：

![](https://i.imgur.com/YSxkGm7.png)

该目录已经默认配置到环境变量，因此可以在任意目录下运行这些命令。其中：

- redis-cli：是redis提供的命令行客户端
- redis-server：是redis的服务端启动脚本
- redis-sentinel：是redis的哨兵启动脚本



### 1.3.3.启动

redis的启动方式有很多种，例如：

- 默认启动
- 指定配置启动
- 开机自启



### 1.3.4.默认启动

安装完成后，在任意目录输入redis-server命令即可启动Redis：

```
redis-server
```

如图：

![](https://cdn.jsdelivr.net/gh/God-ljw/picbed@main/img3/202210141600136.png)

这种启动属于`前台启动`，会阻塞整个会话窗口，窗口关闭或者按下`CTRL + C`则Redis停止。不推荐使用。

### 1.3.5.指定配置启动

如果要让Redis以`后台`方式启动，则必须修改Redis配置文件，就在我们之前解压的redis安装包下（`/usr/local/src/redis-6.2.6`），名字叫redis.conf：

![image-20211211082225509](https://cdn.jsdelivr.net/gh/God-ljw/picbed@main/img3/202210141603771.png)

我们先将这个配置文件备份一份：

```
cp redis.conf redis.conf.bck
```



然后修改redis.conf文件中的一些配置：

```properties
# 允许访问的地址，默认是127.0.0.1，会导致只能在本地访问。修改为0.0.0.0则可以在任意IP访问，生产环境不要设置为0.0.0.0
bind 0.0.0.0
# 守护进程，修改为yes后即可后台运行
daemonize yes 
# 密码，设置后访问Redis必须输入密码
requirepass 123321
```



Redis的其它常见配置：

```properties
# 监听的端口
port 6379
# 工作目录，默认是当前目录，也就是运行redis-server时的命令，日志、持久化等文件会保存在这个目录
dir .
# 数据库数量，设置为1，代表只使用1个库，默认有16个库，编号0~15
databases 1
# 设置redis能够使用的最大内存
maxmemory 512mb
# 日志文件，默认为空，不记录日志，可以指定日志文件名
logfile "redis.log"
```



启动Redis：

```sh
# 进入redis安装目录 
cd /usr/local/src/redis-6.2.6
# 启动
redis-server redis.conf
```



停止服务：

```sh
# 利用redis-cli来执行 shutdown 命令，即可停止 Redis 服务，
# 因为之前配置了密码，因此需要通过 -u 来指定密码
redis-cli -u 123321 shutdown
```



### 1.3.6.开机自启

我们也可以通过配置来实现开机自启。

首先，新建一个系统服务文件：

```sh
vi /etc/systemd/system/redis.service
```

内容如下：

```conf
[Unit]
Description=redis-server
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/bin/redis-server /usr/local/src/redis-6.2.6/redis.conf
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```



然后重载系统服务：

```sh
systemctl daemon-reload
```



现在，我们可以用下面这组命令来操作redis了：

```sh
# 启动
systemctl start redis
# 停止
systemctl stop redis
# 重启
systemctl restart redis
# 查看状态
systemctl status redis
```



执行下面的命令，可以让redis开机自启：

```sh
systemctl enable redis
```



## 1.4.Redis桌面客户端

安装完成Redis，我们就可以操作Redis，实现数据的CRUD了。这需要用到Redis客户端，包括：

- 命令行客户端
- 图形化桌面客户端
- 编程客户端

### 1.4.1.Redis命令行客户端

Redis安装完成后就自带了命令行客户端：redis-cli，使用方式如下：

```sh
redis-cli [options] [commonds]
```

其中常见的options有：

- `-h 127.0.0.1`：指定要连接的redis节点的IP地址，默认是127.0.0.1
- `-p 6379`：指定要连接的redis节点的端口，默认是6379
- `-a 123321`：指定redis的访问密码 

其中的commonds就是Redis的操作命令，例如：

- `ping`：与redis服务端做心跳测试，服务端正常会返回`pong`

不指定commond时，会进入`redis-cli`的交互控制台：

![](https://cdn.jsdelivr.net/gh/God-ljw/picbed@main/img3/202210141601654.png)



### 1.4.2.图形化桌面客户端

GitHub上的大神编写了Redis的图形化桌面客户端，地址：https://github.com/uglide/RedisDesktopManager

不过该仓库提供的是RedisDesktopManager的源码，并未提供windows安装包。



在下面这个仓库可以找到安装包：https://github.com/lework/RedisDesktopManager-Windows/releases



### 1.4.3.安装

在课前资料中可以找到Redis的图形化桌面客户端：

![](https://cdn.jsdelivr.net/gh/God-ljw/picbed@main/img3/202210141601621.png)

解压缩后，运行安装程序即可安装：

![](https://cdn.jsdelivr.net/gh/God-ljw/picbed@main/img3/202210141601487.png)

安装完成后，在安装目录下找到rdm.exe文件：

![](https://cdn.jsdelivr.net/gh/God-ljw/picbed@main/img3/202210141601759.png)

双击即可运行：

![](https://cdn.jsdelivr.net/gh/God-ljw/picbed@main/img3/202210141601464.png)



### 1.4.4.建立连接

点击左上角的`连接到Redis服务器`按钮：

![](https://cdn.jsdelivr.net/gh/God-ljw/picbed@main/img3/202210141601835.png)

在弹出的窗口中填写Redis服务信息：

![](https://cdn.jsdelivr.net/gh/God-ljw/picbed@main/img3/202210141601161.png)

点击确定后，在左侧菜单会出现这个链接：

![](https://cdn.jsdelivr.net/gh/God-ljw/picbed@main/img3/202210141601579.png)

点击即可建立连接了。

![](https://cdn.jsdelivr.net/gh/God-ljw/picbed@main/img3/202210141602738.png)



Redis默认有16个仓库，编号从0至15.  通过配置文件可以设置仓库数量，但是不超过16，并且不能自定义仓库名称。

如果是基于redis-cli连接Redis服务，可以通过select命令来选择数据库：

```sh
# 选择 0号库
select 0
```



## 2.2.1 Redis下载 window

Redis安装包分为windows版和Linux版：

- Windows版下载地址：https://github.com/microsoftarchive/redis/releases
- Linux版下载地址： https://download.redis.io/releases/ 

下载后得到下面安装包：

![image-20210927092053283](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143360.png)

#### 2.2.2 Redis安装

**1）在Linux中安装Redis**

在Linux系统安装Redis步骤：

1. 将Redis安装包上传到Linux
2. 解压安装包，命令：==tar -zxvf redis-4.0.0.tar.gz -C /usr/local==
3. 安装Redis的依赖环境gcc，命令：==yum install gcc-c++==
4. 进入/usr/local/redis-4.0.0，进行编译，命令：==make==
5. 进入redis的src目录进行安装，命令：==make install==

安装后重点文件说明：

> /usr/local/redis-4.0.0/src/redis-server：Redis服务启动脚本
>
> /usr/local/redis-4.0.0/src/redis-cli：Redis客户端脚本
>
> /usr/local/redis-4.0.0/redis.conf：Redis配置文件



**2）在Windows中安装Redis**

Redis的Windows版属于绿色软件，直接解压即可使用，解压后目录结构如下：

![image-20210927093112281](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143361.png)

### 2.3 Redis服务启动与停止

**1）Linux系统中启动和停止Redis**

执行Redis服务启动脚本文件==redis-server==：

![image-20210927094452556](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143362.png)

通过启动日志可以看到，Redis默认端口号为==6379==。

==Ctrl + C==停止Redis服务

通过==redis-cli==可以连接到本地的Redis服务，默认情况下不需要认证即可连接成功。

退出客户端可以输入==exit==或者==quit==命令。

**2）Windows系统中启动和停止Redis**

Windows系统中启动Redis，直接双击redis-server.exe即可启动Redis服务，redis服务默认端口号为6379

![image-20210927100421213](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143363.png)

==Ctrl + C==停止Redis服务

双击==redis-cli.exe==即可启动Redis客户端，默认连接的是本地的Redis服务，而且不需要认证即可连接成功。

![image-20210927100319016](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143364.png)

退出客户端可以输入==exit==或者==quit==命令。

### 2.4 Redis配置文件

前面我们已经启动了Redis服务，默认情况下Redis启动后是在前台运行，而且客户端不需要密码就可以连接到Redis服务。如果我们希望**Redis服务启动后是在后台运行，同时希望客户端认证通过后才能连接到Redis服务**，应该如果做呢？

此时就需要修改Redis的配置文件：

- Linux系统中Redis配置文件：REDIS_HOME/redis.conf
- Windows系统中Redis配置文件：REDIS_HOME/redis.windows.conf



**通过修改Redis配置文件可以进行如下配置：**

**1）**设置Redis服务后台运行

​	将配置文件中的==daemonize==配置项改为yes，默认值为no。**在redis-4.0.0目录下输入（src/redis-server ./redis.conf）**

​	注意：Windows版的Redis不支持后台运行。

**2）**设置Redis服务密码

​	将配置文件中的 ==# requirepass foobared== 配置项取消注释，默认为注释状态。foobared为密码**（现在改为123456）**，可以根据情况自己指定。

**3）**设置允许客户端远程连接Redis服务

​	Redis服务默认只能客户端本地连接，不允许客户端远程连接。将配置文件中的 ==bind 127.0.0.1== 配置项注释掉。



**解释说明：**

> Redis配置文件中 ==#== 表示注释
>
> Redis配置文件中的配置项前面不能有空格，需要顶格写
>
> daemonize：用来指定redis是否要用守护线程的方式启动，设置成yes时，代表开启守护进程模式。在该模式下，redis会在后台运行
>
> requirepass：设置Redis的连接密码
>
> bind：如果指定了bind，则说明只允许来自指定网卡的Redis请求。如果没有指定，就说明可以接受来自任意一个网卡的Redis请求。



**注意**：修改配置文件后需要重启Redis服务配置才能生效，并且启动Redis服务时需要显示的指定配置文件：

1）Linux中启动Redis服务

~~~
# 进入Redis安装目录
cd /usr/local/redis-4.0.0
# 启动Redis服务，指定使用的配置文件
./src/redis-server ./redis.conf
~~~

2）Windows中启动Redis服务

![image-20210927104929169](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143365.png)



由于Redis配置文件中开启了认证校验，即客户端连接时需要提供密码，此时客户端连接方式变为：

![image-20210927105909600](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143366.png)

**解释说明：**

> -h：指定连接的Redis服务的ip地址
>
> -p：指定连接的Redis服务的端口号
>
> -a：指定连接的Redis服务的密码

## 3. Redis数据类型

### 3.1 介绍

Redis存储的是key-value结构的数据，其中key是字符串类型，value有5种常用的数据类型：

- 字符串 string
- 哈希 hash
- 列表 list
- 集合 set
- 有序集合 sorted set / zset

### 3.2 Redis 5种常用数据类型

![image-20210927111819871](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143367.png)

**解释说明：**

> 字符串(string)：普通字符串，常用
>
> 哈希(hash)：适合存储对象
>
> 列表(list)：按照插入顺序排序，可以有重复元素
>
> 集合(set)：无序集合，没有重复元素
>
> 有序集合(sorted set / zset)：集合中每个元素关联一个分数（score），根据分数升序排序，没有重复元素

## 4. Redis常用命令

### 4.1 字符串string操作命令

Redis 中字符串类型常用命令：

* SET：添加或者修改已经存在的一个String类型的键值对
* GET：根据key获取String类型的value
* MSET：批量添加多个String类型的键值对
* MGET：根据多个key获取多个String类型的value
* INCR：让一个整型的key自增1
* INCRBY:让一个整型的key自增并指定步长，例如：incrby num 2 让num值自增2
* INCRBYFLOAT：让一个浮点类型的数字自增并指定步长
* SETNX：添加一个String类型的键值对，前提是这个key不存在，否则不执行
* SETEX：添加一个String类型的键值对，并且指定有效期

**贴心小提示**：以上命令除了INCRBYFLOAT 都是常用命令

* SET 和GET: 如果key不存在则是新增，如果存在则是修改

```java
127.0.0.1:6379> set name Rose  //原来不存在
OK

127.0.0.1:6379> get name 
"Rose"

127.0.0.1:6379> set name Jack //原来存在，就是修改
OK

127.0.0.1:6379> get name
"Jack"
```

* MSET和MGET

```java
127.0.0.1:6379> MSET k1 v1 k2 v2 k3 v3
OK

127.0.0.1:6379> MGET name age k1 k2 k3
1) "Jack" //之前存在的name
2) "10"   //之前存在的age
3) "v1"
4) "v2"
5) "v3"
```

* INCR和INCRBY和DECY

```java
127.0.0.1:6379> get age 
"10"

127.0.0.1:6379> incr age //增加1
(integer) 11
    
127.0.0.1:6379> get age //获得age
"11"

127.0.0.1:6379> incrby age 2 //一次增加2
(integer) 13 //返回目前的age的值
    
127.0.0.1:6379> incrby age 2
(integer) 15
    
127.0.0.1:6379> incrby age -1 //也可以增加负数，相当于减
(integer) 14
    
127.0.0.1:6379> incrby age -2 //一次减少2个
(integer) 12
    
127.0.0.1:6379> DECR age //相当于 incr 负数，减少正常用法
(integer) 11
    
127.0.0.1:6379> get age 
"11"

```

* SETNX

```java
127.0.0.1:6379> help setnx

  SETNX key value
  summary: Set the value of a key, only if the key does not exist
  since: 1.0.0
  group: string

127.0.0.1:6379> set name Jack  //设置名称
OK
127.0.0.1:6379> setnx name lisi //如果key不存在，则添加成功
(integer) 0
127.0.0.1:6379> get name //由于name已经存在，所以lisi的操作失败
"Jack"
127.0.0.1:6379> setnx name2 lisi //name2 不存在，所以操作成功
(integer) 1
127.0.0.1:6379> get name2 
"lisi"
```

* SETEX

```sh
127.0.0.1:6379> setex name 10 jack
OK

127.0.0.1:6379> ttl name
(integer) 8

127.0.0.1:6379> ttl name
(integer) 7

127.0.0.1:6379> ttl name
(integer) 5
```



更多命令可以参考Redis中文网：https://www.redis.net.cn

### 4.2 哈希hash操作命令

Redis hash 是一个string类型的 field 和 value 的映射表，hash特别适合用于存储对象，常用命令：

- HSET key field value：添加或者修改hash类型key的field的值
- HGET key field：获取一个hash类型key的field的值

- HMSET：批量添加多个hash类型key的field的值

- HMGET：批量获取多个hash类型key的field的值

- HGETALL：获取一个hash类型的key中的所有的field和value
- HKEYS：获取一个hash类型的key中的所有的field
- HINCRBY:让一个hash类型key的字段值自增并指定步长
- HSETNX：添加一个hash类型的key的field值，前提是这个field不存在，否则不执行

**贴心小提示**：哈希结构也是我们以后实际开发中常用的命令哟

* HSET和HGET

```java
127.0.0.1:6379> HSET heima:user:3 name Lucy//大key是 heima:user:3 小key是name，小value是Lucy
(integer) 1
127.0.0.1:6379> HSET heima:user:3 age 21// 如果操作不存在的数据，则是新增
(integer) 1
127.0.0.1:6379> HSET heima:user:3 age 17 //如果操作存在的数据，则是修改
(integer) 0
127.0.0.1:6379> HGET heima:user:3 name 
"Lucy"
127.0.0.1:6379> HGET heima:user:3 age
"17"
```

* HMSET和HMGET

```java
127.0.0.1:6379> HMSET heima:user:4 name HanMeiMei
OK
127.0.0.1:6379> HMSET heima:user:4 name LiLei age 20 sex man
OK
127.0.0.1:6379> HMGET heima:user:4 name age sex
1) "LiLei"
2) "20"
3) "man"
```

* HGETALL

```java
127.0.0.1:6379> HGETALL heima:user:4
1) "name"
2) "LiLei"
3) "age"
4) "20"
5) "sex"
6) "man"
```

* HKEYS和HVALS

```java
127.0.0.1:6379> HKEYS heima:user:4
1) "name"
2) "age"
3) "sex"
127.0.0.1:6379> HVALS heima:user:4
1) "LiLei"
2) "20"
3) "man"
```

* HINCRBY

```java
127.0.0.1:6379> HINCRBY  heima:user:4 age 2
(integer) 22
127.0.0.1:6379> HVALS heima:user:4
1) "LiLei"
2) "22"
3) "man"
127.0.0.1:6379> HINCRBY  heima:user:4 age -2
(integer) 20
```

* HSETNX

```java
127.0.0.1:6379> HSETNX heima:user4 sex woman
(integer) 1
127.0.0.1:6379> HGETALL heima:user:3
1) "name"
2) "Lucy"
3) "age"
4) "17"
127.0.0.1:6379> HSETNX heima:user:3 sex woman
(integer) 1
127.0.0.1:6379> HGETALL heima:user:3
1) "name"
2) "Lucy"
3) "age"
4) "17"
5) "sex"
6) "woman"
```

![image-20210927113014567](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143368.png)

### 4.3 列表list操作命令

Redis 列表是简单的字符串列表，按照插入顺序排序，常用命令：

- Redis中的List类型与Java中的LinkedList类似，可以看做是一个双向链表结构。既可以支持正向检索和也可以支持反向检索。

  特征也与LinkedList类似：

  * 有序
  * 元素可以重复
  * 插入和删除快
  * 查询速度一般

  常用来存储一个有序数据，例如：朋友圈点赞列表，评论列表等。

  **List的常见命令有：**

  - LPUSH key element ... ：向列表左侧插入一个或多个元素
  - LPOP key：移除并返回列表左侧的第一个元素，没有则返回nil
  - RPUSH key element ... ：向列表右侧插入一个或多个元素
  - RPOP key：移除并返回列表右侧的第一个元素
  - LRANGE key star end：返回一段角标范围内的所有元素
  - BLPOP和BRPOP：与LPOP和RPOP类似，只不过在没有元素时等待指定时间，而不是直接返回nil

  ![1652943604992](G:/第三阶段框架/第二阶段/2022redis/Redis-笔记资料/01-入门篇/讲义/Redis注释版/Redis.assets/1652943604992.png)

  * LPUSH和RPUSH

  ```java
  127.0.0.1:6379> LPUSH users 1 2 3
  (integer) 3
  127.0.0.1:6379> RPUSH users 4 5 6
  (integer) 6
  ```

  * LPOP和RPOP

  ```java
  127.0.0.1:6379> LPOP users
  "3"
  127.0.0.1:6379> RPOP users
  "6"
  ```

  * LRANGE

  ```java
  127.0.0.1:6379> LRANGE users 1 2
  1) "1"
  2) "4"
  ```


![image-20210927113312384](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143369.png)

### 4.4 集合set操作命令

Redis set 是string类型的无序集合。集合成员是唯一的，这就意味着集合中不能出现重复的数据，常用命令：

- SADD key member ... ：向set中添加一个或多个元素
- SREM key member ... : 移除set中的指定元素

- SCARD key： 返回set中元素的个数

- SISMEMBER key member：判断一个元素是否存在于set中

- SMEMBERS：获取set中的所有元素

- SINTER key1 key2 ... ：求key1与key2的交集

- SDIFF key1 key2 ... ：求key1与key2的差集

- SUNION key1 key2 ..：求key1和key2的并集

- 具体命令**

  ```java
  127.0.0.1:6379> sadd s1 a b c
  (integer) 3
  127.0.0.1:6379> smembers s1
  1) "c"
  2) "b"
  3) "a"
  127.0.0.1:6379> srem s1 a
  (integer) 1
      
  127.0.0.1:6379> SISMEMBER s1 a
  (integer) 0
      
  127.0.0.1:6379> SISMEMBER s1 b
  (integer) 1
      
  127.0.0.1:6379> SCARD s1
  (integer) 2
  ```

  **案例**

  * 将下列数据用Redis的Set集合来存储：
  * 张三的好友有：李四.王五.赵六
  * 李四的好友有：王五.麻子.二狗
  * 利用Set的命令实现下列功能：
  * 计算张三的好友有几人
  * 计算张三和李四有哪些共同好友
  * 查询哪些人是张三的好友却不是李四的好友
  * 查询张三和李四的好友总共有哪些人
  * 判断李四是否是张三的好友
  * 判断张三是否是李四的好友
  * 将李四从张三的好友列表中移除

  ```java
  127.0.0.1:6379> SADD zs lisi wangwu zhaoliu
  (integer) 3
      
  127.0.0.1:6379> SADD ls wangwu mazi ergou
  (integer) 3
      
  127.0.0.1:6379> SCARD zs
  (integer) 3
      
  127.0.0.1:6379> SINTER zs ls
  1) "wangwu"
      
  127.0.0.1:6379> SDIFF zs ls
  1) "zhaoliu"
  2) "lisi"
      
  127.0.0.1:6379> SUNION zs ls
  1) "wangwu"
  2) "zhaoliu"
  3) "lisi"
  4) "mazi"
  5) "ergou"
      
  127.0.0.1:6379> SISMEMBER zs lisi
  (integer) 1
      
  127.0.0.1:6379> SISMEMBER ls zhangsan
  (integer) 0
      
  127.0.0.1:6379> SREM zs lisi
  (integer) 1
      
  127.0.0.1:6379> SMEMBERS zs
  1) "zhaoliu"
  2) "wangwu"
  ```


![image-20210927113632472](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143370.png)

### 4.5 有序集合sorted set操作命令

Redis sorted set 有序集合是 string 类型元素的集合，且不允许重复的成员。每个元素都会关联一个double类型的分数(score) 。redis正是通过分数来为集合中的成员进行从小到大排序。有序集合的成员是唯一的，但分数却可以重复。

常用命令：

- ZADD key score member：添加一个或多个元素到sorted set ，如果已经存在则更新其score值
- ZREM key member：删除sorted set中的一个指定元素
- ZSCORE key member : 获取sorted set中的指定元素的score值
- ZRANK key member：获取sorted set 中的指定元素的排名
- ZCARD key：获取sorted set中的元素个数
- ZCOUNT key min max：统计score值在给定范围内的所有元素的个数
- ZINCRBY key increment member：让sorted set中的指定元素自增，步长为指定的increment值
- ZRANGE key min max：按照score排序后，获取指定排名范围内的元素
- ZRANGEBYSCORE key min max：按照score排序后，获取指定score范围内的元素
- ZDIFF.ZINTER.ZUNION：求差集.交集.并集

注意：所有的排名默认都是升序，如果要降序则在命令的Z后面添加REV即可，例如：

- **升序**获取sorted set 中的指定元素的排名：ZRANK key member
- **降序**获取sorted set 中的指定元素的排名：ZREVRANK key memeber

![image-20210927114003383](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143371.png)

### 4.6 通用命令

Redis中的通用命令，主要是针对key进行操作的相关命令：

- **KEYS** pattern  查找所有符合给定模式( pattern)的 key 
- **EXISTS** key  检查给定 key 是否存在
- **TYPE** key  返回 key 所储存的值的类型
- **TTL** key  返回给定 key 的剩余生存时间(TTL, time to live)，以秒为单位
- **DEL** key  该命令用于在 key 存在是删除 key

* KEYS

```sh
127.0.0.1:6379> keys *
1) "name"
2) "age"
127.0.0.1:6379>

# 查询以a开头的key
127.0.0.1:6379> keys a*
1) "age"
127.0.0.1:6379>
```

**贴心小提示：在生产环境下，不推荐使用keys 命令，因为这个命令在key过多的情况下，效率不高**

* DEL

```sh
127.0.0.1:6379> help del

  DEL key [key ...]
  summary: Delete a key
  since: 1.0.0
  group: generic

127.0.0.1:6379> del name #删除单个
(integer) 1  #成功删除1个

127.0.0.1:6379> keys *
1) "age"

127.0.0.1:6379> MSET k1 v1 k2 v2 k3 v3 #批量添加数据
OK

127.0.0.1:6379> keys *
1) "k3"
2) "k2"
3) "k1"
4) "age"

127.0.0.1:6379> del k1 k2 k3 k4
(integer) 3   #此处返回的是成功删除的key，由于redis中只有k1,k2,k3 所以只成功删除3个，最终返回
127.0.0.1:6379>

127.0.0.1:6379> keys * #再查询全部的key
1) "age"	#只剩下一个了
127.0.0.1:6379>
```

**贴心小提示：同学们在拷贝代码的时候，只需要拷贝对应的命令哦~**

* EXISTS

```sh
127.0.0.1:6379> help EXISTS

  EXISTS key [key ...]
  summary: Determine if a key exists
  since: 1.0.0
  group: generic

127.0.0.1:6379> exists age
(integer) 1

127.0.0.1:6379> exists name
(integer) 0
```

* EXPIRE

**贴心小提示**：内存非常宝贵，对于一些数据，我们应当给他一些过期时间，当过期时间到了之后，他就会自动被删除~

```sh
127.0.0.1:6379> expire age 10
(integer) 1

127.0.0.1:6379> ttl age
(integer) 8

127.0.0.1:6379> ttl age
(integer) 6

127.0.0.1:6379> ttl age
(integer) -2

127.0.0.1:6379> ttl age
(integer) -2  #当这个key过期了，那么此时查询出来就是-2 

127.0.0.1:6379> keys *
(empty list or set)

127.0.0.1:6379> set age 10 #如果没有设置过期时间
OK

127.0.0.1:6379> ttl age
(integer) -1  # ttl的返回值就是-1
```



## 5. 在Java中操作Redis

### 5.1 介绍

前面我们讲解了Redis的常用命令，这些命令是我们操作Redis的基础，那么我们在java程序中应该如何操作Redis呢？这就需要使用Redis的Java客户端，就如同我们使用JDBC操作MySQL数据库一样。

Redis 的 Java 客户端很多，官方推荐的有三种：

- Jedis
- Lettuce
- Redisson

Spring 对 Redis 客户端进行了整合，提供了 Spring Data Redis，在Spring Boot项目中还提供了对应的Starter，即 spring-boot-starter-data-redis。

### 5.2 Jedis

Jedis 是 Redis 的 Java 版本的客户端实现。

maven坐标：

~~~xml
<dependency>
	<groupId>redis.clients</groupId>
	<artifactId>jedis</artifactId>
	<version>2.8.0</version>
</dependency>
~~~

使用 Jedis 操作 Redis 的步骤：

1. 获取连接
2. 执行操作
3. 关闭连接

示例代码：

~~~java
package com.itheima.test;

import org.junit.Test;
import redis.clients.jedis.Jedis;
import java.util.Set;

/**
 * 使用Jedis操作Redis
 */
public class JedisTest {

    @Test
    public void testRedis(){
        //1 获取连接
        Jedis jedis = new Jedis("localhost",6379);
        
        //2 执行具体的操作
        jedis.set("username","xiaoming");

        String value = jedis.get("username");
        System.out.println(value);

        //jedis.del("username");

        jedis.hset("myhash","addr","bj");
        String hValue = jedis.hget("myhash", "addr");
        System.out.println(hValue);

        Set<String> keys = jedis.keys("*");
        for (String key : keys) {
            System.out.println(key);
        }

        //3 关闭连接
        jedis.close();
    }
}
~~~

### 5.3 Spring Data Redis

#### 5.3.1 介绍

Spring Data Redis 是 Spring 的一部分，提供了在 Spring 应用中通过简单的配置就可以访问 Redis 服务，对 Redis 底层开发包进行了高度封装。在 Spring 项目中，可以使用Spring Data Redis来简化 Redis 操作。

网址：https://spring.io/projects/spring-data-redis

![image-20210927143741458](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143372.png)

maven坐标：

~~~xml
<dependency>
	<groupId>org.springframework.data</groupId>
	<artifactId>spring-data-redis</artifactId>
	<version>2.4.8</version>
</dependency>
~~~

Spring Boot提供了对应的Starter，maven坐标：

~~~xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
~~~

Spring Data Redis中提供了一个高度封装的类：**RedisTemplate**，针对 Jedis 客户端中大量api进行了归类封装,将同一类型操作封装为operation接口，具体分类如下：

- ValueOperations：简单K-V操作
- SetOperations：set类型数据操作
- ZSetOperations：zset类型数据操作
- HashOperations：针对hash类型的数据操作
- ListOperations：针对list类型的数据操作

#### 5.3.2 使用方式

##### 5.3.2.1 环境搭建

第一步：创建maven项目springdataredis_demo，配置pom.xml文件

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.5</version>
        <relativePath/>
    </parent>
    <groupId>com.itheima</groupId>
    <artifactId>springdataredis_demo</artifactId>
    <version>1.0-SNAPSHOT</version>
    <properties>
        <java.version>1.8</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>2.4.5</version>
            </plugin>
        </plugins>
    </build>
</project>
~~~

第二步：编写启动类

~~~java
package com.itheima;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class App {

    public static void main(String[] args) {
        SpringApplication.run(App.class,args);
    }

}
~~~

第三步：配置application.yml

~~~yaml
spring:
  application:
    name: springdataredis_demo
  #Redis相关配置
  redis:
    host: localhost
    port: 6379
    #password: 123456
    database: 0 #操作的是0号数据库
    jedis:
      #Redis连接池配置
      pool:
        max-active: 8 #最大连接数
        max-wait: 1ms #连接池最大阻塞等待时间
        max-idle: 4 #连接池中的最大空闲连接
        min-idle: 0 #连接池中的最小空闲连接
~~~

解释说明：

> spring.redis.database：指定使用Redis的哪个数据库，Redis服务启动后默认有16个数据库，编号分别是从0到15。
>
> 可以通过修改Redis配置文件来指定数据库的数量。

第四步：提供配置类

~~~java
package com.itheima.config;

import org.springframework.cache.annotation.CachingConfigurerSupport;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.StringRedisSerializer;

/**
 * Redis配置类
 */
@Configuration
public class RedisConfig extends CachingConfigurerSupport {

    @Bean
    public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory connectionFactory) {

        RedisTemplate<Object, Object> redisTemplate = new RedisTemplate<>();

        //默认的Key序列化器为：JdkSerializationRedisSerializer
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());

        redisTemplate.setConnectionFactory(connectionFactory);

        return redisTemplate;
    }

}
~~~

解释说明：

> 当前配置类不是必须的，因为 Spring Boot 框架会自动装配 RedisTemplate 对象，但是默认的key序列化器为JdkSerializationRedisSerializer，导致我们存到Redis中后的数据和原始数据有差别

第五步：提供测试类

~~~java
package com.itheima.test;

import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

@SpringBootTest
@RunWith(SpringRunner.class)
public class SpringDataRedisTest {

    @Autowired
    private RedisTemplate redisTemplate;
    
}
~~~

##### 5.3.2.2 操作字符串类型数据

~~~java
/**
 * 操作String类型数据
*/
@Test
public void testString(){
    //存值
    redisTemplate.opsForValue().set("city123","beijing");

    //取值
    String value = (String) redisTemplate.opsForValue().get("city123");
    System.out.println(value);

    //存值，同时设置过期时间
    redisTemplate.opsForValue().set("key1","value1",10l, TimeUnit.SECONDS);

    //存值，如果存在则不执行任何操作
    Boolean aBoolean = redisTemplate.opsForValue().setIfAbsent("city1234", "nanjing");
    System.out.println(aBoolean);
}
~~~



##### 5.3.2.3 操作哈希类型数据

~~~java
/**
 * 操作Hash类型数据
*/
@Test
public void testHash(){
    HashOperations hashOperations = redisTemplate.opsForHash();

    //存值
    hashOperations.put("002","name","xiaoming");
    hashOperations.put("002","age","20");
    hashOperations.put("002","address","bj");

    //取值
    String age = (String) hashOperations.get("002", "age");
    System.out.println(age);

    //获得hash结构中的所有字段
    Set keys = hashOperations.keys("002");
    for (Object key : keys) {
        System.out.println(key);
    }

    //获得hash结构中的所有值
    List values = hashOperations.values("002");
    for (Object value : values) {
        System.out.println(value);
    }
}
~~~



##### 5.3.2.4 操作列表类型数据

~~~java
/**
 * 操作List类型的数据
*/
@Test
public void testList(){
    ListOperations listOperations = redisTemplate.opsForList();

    //存值
    listOperations.leftPush("mylist","a");
    listOperations.leftPushAll("mylist","b","c","d");

    //取值
    List<String> mylist = listOperations.range("mylist", 0, -1);
    for (String value : mylist) {
        System.out.println(value);
    }

    //获得列表长度 llen
    Long size = listOperations.size("mylist");
    int lSize = size.intValue();
    for (int i = 0; i < lSize; i++) {
        //出队列
        String element = (String) listOperations.rightPop("mylist");
        System.out.println(element);
    }
}
~~~



##### 5.3.2.5 操作集合类型数据

~~~java
/**
 * 操作Set类型的数据
*/
@Test
public void testSet(){
    SetOperations setOperations = redisTemplate.opsForSet();

    //存值
    setOperations.add("myset","a","b","c","a");

    //取值
    Set<String> myset = setOperations.members("myset");
    for (String o : myset) {
        System.out.println(o);
    }

    //删除成员
    setOperations.remove("myset","a","b");

    //取值
    myset = setOperations.members("myset");
    for (String o : myset) {
        System.out.println(o);
    }

}
~~~



##### 5.3.2.6 操作有序集合类型数据

~~~java
/**
 * 操作ZSet类型的数据
*/
@Test
public void testZset(){
    ZSetOperations zSetOperations = redisTemplate.opsForZSet();

    //存值
    zSetOperations.add("myZset","a",10.0);
    zSetOperations.add("myZset","b",11.0);
    zSetOperations.add("myZset","c",12.0);
    zSetOperations.add("myZset","a",13.0);

    //取值
    Set<String> myZset = zSetOperations.range("myZset", 0, -1);
    for (String s : myZset) {
        System.out.println(s);
    }

    //修改分数
    zSetOperations.incrementScore("myZset","b",20.0);

    //取值
    myZset = zSetOperations.range("myZset", 0, -1);
    for (String s : myZset) {
        System.out.println(s);
    }

    //删除成员
    zSetOperations.remove("myZset","a","b");

    //取值
    myZset = zSetOperations.range("myZset", 0, -1);
    for (String s : myZset) {
        System.out.println(s);
    }
}
~~~



##### 5.3.2.7 通用操作

~~~java
/**
 * 通用操作，针对不同的数据类型都可以操作
*/
@Test
public void testCommon(){
    //获取Redis中所有的key
    Set<String> keys = redisTemplate.keys("*");
    for (String key : keys) {
        System.out.println(key);
    }

    //判断某个key是否存在
    Boolean itcast = redisTemplate.hasKey("itcast");
    System.out.println(itcast);

    //删除指定key
    redisTemplate.delete("myZset");

    //获取指定key对应的value的数据类型
    DataType dataType = redisTemplate.type("myset");
    System.out.println(dataType.name());

}
~~~



## 6. SpringCache

### 4.1 介绍

**Spring Cache**是一个框架，实现了基于注解的缓存功能，只需要简单地加一个注解，就能实现缓存功能，大大简化我们在业务中操作缓存的代码。

Spring Cache只是提供了一层抽象，底层可以切换不同的cache实现。具体就是通过**CacheManager**接口来统一不同的缓存技术。CacheManager是Spring提供的各种缓存技术抽象接口。



针对不同的缓存技术需要实现不同的CacheManager：

| **CacheManager**    | **描述**                           |
| ------------------- | ---------------------------------- |
| EhCacheCacheManager | 使用EhCache作为缓存技术            |
| GuavaCacheManager   | 使用Google的GuavaCache作为缓存技术 |
| RedisCacheManager   | 使用Redis作为缓存技术              |



### 4.2 注解

在SpringCache中提供了很多缓存操作的注解，常见的是以下的几个：

| **注解**       | **说明**                                                     |
| -------------- | ------------------------------------------------------------ |
| @EnableCaching | 开启缓存注解功能                                             |
| @Cacheable     | 在方法执行前spring先查看缓存中是否有数据，如果有数据，则直接返回缓存数据；若没有数据，调用方法并将方法返回值放到缓存中 |
| @CachePut      | 将方法的返回值放到缓存中                                     |
| @CacheEvict    | 将一条或多条数据从缓存中删除                                 |



在spring boot项目中，使用缓存技术只需在项目中导入相关缓存技术的依赖包，并在启动类上使用@EnableCaching开启缓存支持即可。

例如，使用Redis作为缓存技术，只需要导入Spring data Redis的maven坐标即可。



### 4.3 入门程序

接下来，我们将通过一个入门案例来演示一下SpringCache的常见用法。 上面我们提到，SpringCache可以集成不同的缓存技术，如Redis、Ehcache甚至我们可以使用Map来缓存数据， 接下来我们在演示的时候，就先通过一个Map来缓存数据，最后我们再换成Redis来缓存。



#### 4.3.1 环境准备

**1). 数据库准备**

将今天资料中的SQL脚本直接导入数据库中。

![image-20210822230236957](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143373.png) 



**2). 导入基础工程**

基础环境的代码，在我们今天的资料中已经准备好了， 大家只需要将这个工程导入进来就可以了。导入进来的工程结构如下： 

![image-20210822225934512](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143374.png) 

由于SpringCache的基本功能是Spring核心(spring-context)中提供的，所以目前我们进行简单的SpringCache测试，是可以不用额外引入其他依赖的。



**3). 注入CacheManager**

我们可以在UserController注入一个CacheManager，在Debug时，我们可以通过CacheManager跟踪缓存中数据的变化。

<img src="https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143375.png" alt="image-20210822231333527" style="zoom:80%;" /> 



我们可以看到CacheManager是一个接口，默认的实现有以下几种 ；

![image-20210822231217450](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143376.png) 

而在上述的这几个实现中，默认使用的是 ConcurrentMapCacheManager。稍后我们可以通过断点的形式跟踪缓存数据的变化。



**4). 引导类上加@EnableCaching**

在引导类上加该注解，就代表当前项目开启缓存注解功能。

![image-20210822231616569](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143377.png) 







#### 4.3.2 @CachePut注解

> @CachePut 说明： 
>
> ​	作用: 将方法返回值，放入缓存
>
> ​	value: 缓存的名称, 每个缓存名称下面可以有很多key
>
> ​	key: 缓存的key  ----------> 支持Spring的表达式语言SPEL语法



**1). 在save方法上加注解@CachePut**

当前UserController的save方法是用来保存用户信息的，我们希望在该用户信息保存到数据库的同时，也往缓存中缓存一份数据，我们可以在save方法上加上注解 @CachePut，用法如下： 

```java
/**
* CachePut：将方法返回值放入缓存
* value：缓存的名称，每个缓存名称下面可以有多个key
* key： 缓存的key 其表达式都是以#开头
*/
@CachePut(value = "userCache", key = "#user.id")
@PostMapping
public User save(User user){
    userService.save(user);
    return user;
}
```



> key的写法如下： 
>
> ​	#user.id : #user指的是方法形参的名称, id指的是user的id属性 , 也就是使用user的id属性作为key ;
>
> ​	#user.name: #user指的是方法形参的名称, name指的是user的name属性 ,也就是使用user的name属性作为key ;
>
> ​	
>
> ​	#result.id : #result代表方法返回值，该表达式 代表以返回对象的id属性作为key ；
>
> ​	#result.name : #result代表方法返回值，该表达式 代表以返回对象的name属性作为key ；



**2). 测试**

启动服务,通过postman请求访问UserController的方法, 然后通过断点的形式跟踪缓存数据。

![image-20210822233438182](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143378.png)



第一次访问时，缓存中的数据是空的，因为save方法执行完毕后才会缓存数据。 

![image-20210822233724439](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143379.png) 



第二次访问时，我们通过debug可以看到已经有一条数据了，就是上次保存的数据，已经缓存了，缓存的key就是用户的id。

![image-20210822234105085](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143380.png) 



==注意: 上述的演示，最终的数据，实际上是缓存在ConcurrentHashMap中，那么当我们的服务器重启之后，缓存中的数据就会丢失。 我们后面使用了Redis来缓存就不存在这样的问题了。==







#### 4.3.3 @CacheEvict注解

> @CacheEvict 说明： 
>
> ​	作用: 清理指定缓存
>
> ​	value: 缓存的名称，每个缓存名称下面可以有多个key
>
> ​	key: 缓存的key  ----------> 支持Spring的表达式语言SPEL语法



**1). 在 delete 方法上加注解@CacheEvict**

当我们在删除数据库user表的数据的时候,我们需要删除缓存中对应的数据,此时就可以使用@CacheEvict注解, 具体的使用方式如下: 

```java
/**
* CacheEvict：清理指定缓存
* value：缓存的名称，每个缓存名称下面可以有多个key
* key：缓存的key
*/
@CacheEvict(value = "userCache",key = "#p0")  //#p0 代表第一个参数
//@CacheEvict(value = "userCache",key = "#root.args[0]") //#root.args[0] 代表第一个参数
//@CacheEvict(value = "userCache",key = "#id") //#id 代表变量名为id的参数
@DeleteMapping("/{id}")
public void delete(@PathVariable Long id){
    userService.removeById(id);
}
```





**2). 测试**

要测试缓存的删除，我们先访问save方法4次，保存4条数据到数据库的同时，也保存到缓存中，最终我们可以通过debug看到缓存中的数据信息。 然后我们在通过postman访问delete方法， 如下： 

![image-20210823000431356](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143381.png) 



删除数据时，通过debug我们可以看到已经缓存的4条数据：

![image-20210823000458089](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143382.png) 



当执行完delete操作之后，我们再次保存一条数据，在保存的时候debug查看一下删除的ID值是否已经被删除。

![image-20210823000733218](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143383.png) 





**3). 在 update 方法上加注解@CacheEvict**

在更新数据之后，数据库的数据已经发生了变更，我们需要将缓存中对应的数据删除掉，避免出现数据库数据与缓存数据不一致的情况。

``` java
//@CacheEvict(value = "userCache",key = "#p0.id")   //第一个参数的id属性
//@CacheEvict(value = "userCache",key = "#user.id") //参数名为user参数的id属性
//@CacheEvict(value = "userCache",key = "#root.args[0].id") //第一个参数的id属性
@CacheEvict(value = "userCache",key = "#result.id")         //返回值的id属性
@PutMapping
public User update(User user){
    userService.updateById(user);
    return user;
}
```



加上注解之后，我们可以重启服务，然后测试方式，基本和上述相同，先缓存数据，然后再更新某一条数据，通过debug的形式查询缓存数据的情况。





#### 4.3.4 @Cacheable注解

> @Cacheable 说明:
>
> ​	作用: 在方法执行前，spring先查看缓存中是否有数据，如果有数据，则直接返回缓存数据；若没有数据，调用方法并将方法返回值放到缓存中
>
> ​	value: 缓存的名称，每个缓存名称下面可以有多个key
>
> ​	key: 缓存的key  ----------> 支持Spring的表达式语言SPEL语法



**1). 在getById上加注解@Cacheable**

```java
/**
* Cacheable：在方法执行前spring先查看缓存中是否有数据，如果有数据，则直接返回缓存数据；若没有数据，调用方法并将方法返回值放到缓存中
* value：缓存的名称，每个缓存名称下面可以有多个key
* key：缓存的key
*/
@Cacheable(value = "userCache",key = "#id")
@GetMapping("/{id}")
public User getById(@PathVariable Long id){
    User user = userService.getById(id);
    return user;
}
```



**2). 测试**

我们可以重启服务，然后通过debug断点跟踪程序执行。我们发现，第一次访问，会请求我们controller的方法，查询数据库。后面再查询相同的id，就直接获取到数据库，不用再查询数据库了，就说明缓存生效了。

![image-20210823002517941](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143384.png) 



当我们在测试时，查询一个数据库不存在的id值，第一次查询缓存中没有，也会查询数据库。而第二次再查询时，会发现，不再查询数据库了，而是直接返回，那也就是说如果根据ID没有查询到数据,那么会自动缓存一个null值。 我们可以通过debug，验证一下： 

![image-20210823002907048](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143385.png) 



我们能不能做到，当查询到的值不为null时，再进行缓存，如果为null，则不缓存呢? 答案是可以的。



**3). 缓存非null值**

在@Cacheable注解中，提供了两个属性分别为： condition， unless 。

> condition : 表示满足什么条件, 再进行缓存 ;
>
> unless : 表示满足条件则不缓存 ; 与上述的condition是反向的 ;



具体实现方式如下: 

```java
/**
 * Cacheable：在方法执行前spring先查看缓存中是否有数据，如果有数据，则直接返回缓存数据；若没有数据，调用方法并将方法返回值放到缓存中
 * value：缓存的名称，每个缓存名称下面可以有多个key
 * key：缓存的key
 * condition：条件，满足条件时才缓存数据
 * unless：满足条件则不缓存
 */
@Cacheable(value = "userCache",key = "#id", unless = "#result == null")
@GetMapping("/{id}")
public User getById(@PathVariable Long id){
    User user = userService.getById(id);
    return user;
}
```

==注意： 此处，我们使用的时候只能够使用 unless， 因为在condition中，我们是无法获取到结果 #result的。==



**4). 在list方法上加注解@Cacheable**

在list方法中进行查询时，有两个查询条件，如果传递了id，根据id查询； 如果传递了name， 根据name查询，那么我们缓存的key在设计的时候，就需要既包含id，又包含name。 具体的代码实现如下： 

```java
@Cacheable(value = "userCache",key = "#user.id + '_' + #user.name")
@GetMapping("/list")
public List<User> list(User user){
    LambdaQueryWrapper<User> queryWrapper = new LambdaQueryWrapper<>();
    queryWrapper.eq(user.getId() != null,User::getId,user.getId());
    queryWrapper.eq(user.getName() != null,User::getName,user.getName());
    List<User> list = userService.list(queryWrapper);
    return list;
}
```



然后再次重启服务，进行测试。

![image-20210823005220230](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143386.png) 

第一次查询时，需要查询数据库，在后续的查询中，就直接查询了缓存，不再查询数据库了。







### 4.4 集成Redis

在使用上述默认的ConcurrentHashMap做缓存时，服务重启之后，之前缓存的数据就全部丢失了，操作起来并不友好。在项目中使用，我们会选择使用redis来做缓存，主要需要操作以下几步： 

1). pom.xml

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```



2). application.yml

```yml
spring:
  redis:
    host: 192.168.200.200
    port: 6379
    password: root@123456
    database: 0
  cache:
    redis:
      time-to-live: 1800000   #设置缓存过期时间，可选
```



3). 测试

重新启动项目，通过postman发送根据id查询数据的请求，然后通过redis的图形化界面工具，查看redis中是否可以正常的缓存数据。

![image-20210823010810680](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143387.png)  

![image-20210823010742530](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209262143388.png)


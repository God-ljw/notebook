#  MySQL基础

## MySQL

### 2.1  MySQL安装

> **安装环境:Win10 64位**
> **软件版本:MySQL 5.7.24 解压版**

#### 2.1.1  下载

https://downloads.mysql.com/archives/community/

选择选择和自己**系统位数**相对应的版本点击右边的`Download`，此时会进到另一个页面，同样在接近页面底部的地方找到如下图所示的位置：



不用理会上面的登录和注册按钮，直接点击 `No thanks, just start my download.` 就可以下载。

#### 2.1.2  安装（解压)

下载完成后我们得到的是一个压缩包，将其解压，我们就可以得到MySQL 5.7.24的软件本体了(就是一个文件夹)，我们可以把它放在你想安装的位置。

### 2.2  MySQL卸载

如果你想卸载MySQL，也很简单。

右键开始菜单，选择`命令提示符(管理员)`，打开黑框。

1. 敲入`net stop mysql`，回车。

```
net stop mysql
```

![image-20220601173906220](https://s2.loli.net/2022/06/01/cg8LqKArv6HZhsl.png)



2. 再敲入`mysqld -remove mysql`，回车。

```
mysqld -remove mysql
```

![image-20220601181458096](https://s2.loli.net/2022/06/01/NjF6RnB9WdaK8c7.png)

3. 最后删除MySQL目录及相关的环境变量。

**至此，MySQL卸载完成！**



### 2.3  MySQL配置

#### 2.3.1  添加环境变量

> 环境变量里面有很多选项，这里我们只用到`Path`这个参数。为什么在初始化的开始要添加环境变量呢？
> 在黑框(即CMD)中输入一个可执行程序的名字，Windows会先在环境变量中的`Path`所指的路径中寻找一遍，如果找到了就直接执行，没找到就在当前工作目录找，如果还没找到，就报错。我们添加环境变量的目的就是能够在任意一个黑框直接调用MySQL中的相关程序而不用总是修改工作目录，大大简化了操作。



右键`此电脑`→`属性`，点击`高级系统设置`

![image-20220601181511930](https://s2.loli.net/2022/06/01/rInQa2gY6MsCblT.png)

点击`环境变量`

![image-20220601181518457](https://s2.loli.net/2022/06/01/OhrdUiAzc4y26Du.png)

在`系统变量`中新建MYSQL_HOME

![image-20220601181528421](https://s2.loli.net/2022/06/01/Ad2QCOKWFkwTV75.png)

在`系统变量`中找到并**双击**`Path`

![image-20220601181608325](https://s2.loli.net/2022/06/01/oD2irxRYzqPjKLa.png)

点击`新建`

![image-20220601181616303](https://s2.loli.net/2022/06/01/qENvCRkIAs8VzGf.png)

最后点击确定。

**如何验证是否添加成功？**

右键开始菜单(就是屏幕左下角)，选择`命令提示符(管理员)`，打开黑框，敲入`mysql`，回车。
如果提示`Can't connect to MySQL server on 'localhost'`则证明添加成功；
如果提示`mysql不是内部或外部命令，也不是可运行的程序或批处理文件`则表示添加添加失败，请重新检查步骤并重试。

#### 2.3.2  新建配置文件

新建一个文本文件，内容如下：

```properties
[mysql]
default-character-set=utf8

[mysqld]
character-set-server=utf8
default-storage-engine=INNODB
sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
```

把上面的文本文件另存为，在保存类型里选`所有文件 (*.*)`，文件名叫`my.ini`，存放的路径为MySQL的`根目录`(例如我的是`D:\software\mysql-5.7.24-winx64`,根据自己的MySQL目录位置修改)。



![image-20220601181627940](https://s2.loli.net/2022/06/01/vhPUt3guXf9nN4S.png)

![image-20220601181644789](https://s2.loli.net/2022/06/01/tjfKTEAXdu2nP3Q.png)

上面代码意思就是配置数据库的默认编码集为utf-8和默认存储引擎为INNODB。

#### 2.3.3  初始化MySQL

在刚才的黑框中敲入`mysqld --initialize-insecure`，回车，稍微等待一会，如果出现没有出现报错信息(如下图)则证明data目录初始化没有问题，此时再查看MySQL目录下已经有data目录生成。

```
mysqld --initialize-insecure
```

![image-20220601181653425](https://s2.loli.net/2022/06/01/NplCnJLmVcRt6Bh.png)





tips：如果出现如下错误

![image-20220601181700786](https://s2.loli.net/2022/06/01/HKliNuoMvWd1nFe.png)

是由于权限不足导致的，去`C:\Windows\System32` 下以管理员方式运行 cmd.exe

![image-20220601181707156](https://s2.loli.net/2022/06/01/IEcLkH7GDyPd9AK.png)

![image-20220601181713558](https://s2.loli.net/2022/06/01/ZoqY4DCL9pRcyXJ.png)

#### 2.3.4  注册MySQL服务

在黑框里敲入`mysqld -install`，回车。

```
mysqld -install
```



![image-20220601181720496](https://s2.loli.net/2022/06/01/9NaeXx2gQTOEHV7.png)

现在你的计算机上已经安装好了MySQL服务了。

MySQL服务器





#### 2.3.5  启动MySQL服务

在黑框里敲入`net start mysql`，回车。

```java
net start mysql  // 启动mysql服务
    
net stop mysql  // 停止mysql服务
```



![image-20220601181733037](https://s2.loli.net/2022/06/01/uxjiebUNODWoVpr.png)

#### 2.3.6  修改默认账户密码

在黑框里敲入`mysqladmin -u root password 1234`，这里的`1234`就是指默认管理员(即root账户)的密码，可以自行修改成你喜欢的。

```
mysqladmin -u root password 1234
```



![image-20220601181740737](https://s2.loli.net/2022/06/01/Prd1hAi7Z3LBUjp.png)

**至此，MySQL 5.7 解压版安装完毕！**



### 2.4  MySQL登陆和退出

#### 2.4.1  登陆

右键开始菜单，选择`命令提示符`，打开黑框。
在黑框中输入，`mysql -uroot -p1234`，回车，出现下图且左下角为`mysql>`，则登录成功。

```
mysql -uroot -p1234
```

![image-20220601181751536](https://s2.loli.net/2022/06/01/6eopdZPV2scYTaC.png)

**到这里你就可以开始你的MySQL之旅了！**

登陆参数：

```
mysql -u用户名 -p密码 -h要连接的mysql服务器的ip地址(默认127.0.0.1) -P端口号(默认3306)
```



#### 2.4.2  退出

退出mysql：

```
exit
quit
```



### 2.5  MySQL数据模型

**关系型数据库：**

> 关系型数据库是建立在关系模型基础上的数据库，简单说，关系型数据库是由多张能互相连接的 二维表 组成的数据库

如下图，`订单信息表` 和 `客户信息表` 都是有行有列二维表我们将这样的称为关系型数据![image-20220601181852309](https://s2.loli.net/2022/06/01/BzwuEpYxjL8HoSf.png)

接下来看关系型数据库的优点：

* 都是使用表结构，格式一致，易于维护。
* 使用通用的 SQL 语言操作，使用方便，可用于复杂查询。
  * 关系型数据库都可以通过SQL进行操作，所以使用方便。
  * 复杂查询。现在需要查询001号订单数据，我们可以看到该订单是1号客户的订单，而1号订单是李聪这个客户。以后也可以在一张表中进行统计分析等操作。
* 数据存储在磁盘中，安全。

**数据模型：**

![image-20220601181843688](https://s2.loli.net/2022/06/01/NhZBUPLbQ24lRjt.png)

如上图，我们通过客户端可以通过数据库管理系统创建数据库，在数据库中创建表，在表中添加数据。创建的每一个数据库对应到磁盘上都是一个文件夹。比如可以通过SQL语句创建一个数据库（数据库名称为db1），语句如下。该语句咱们后面会学习。

![image-20220601181831147](https://s2.loli.net/2022/06/01/votT5JMzQA78sIU.png)



我们可以在数据库安装目录下的data目录下看到多了一个 `db1` 的文件夹。所以，**在MySQL中一个数据库对应到磁盘上的一个文件夹。**

而一个数据库下可以创建多张表，我们到MySQL中自带的mysql数据库的文件夹目录下：

![image-20220601181824458](https://s2.loli.net/2022/06/01/TwLPhSjIYRgCHnJ.png)

而上图中右边的 `db.frm` 是表文件，`db.MYD` 是数据文件，通过这两个文件就可以查询到数据展示成二维表的效果。

**小结：**

* MySQL中可以创建多个数据库，每个数据库对应到磁盘上的一个文件夹
* 在每个数据库中可以创建多个表，每张都对应到磁盘上一个 frm 文件
* 每张表可以存储多条数据，数据会被存储到磁盘中  MYD 文件中



## 1，DDL:操作数据库

我们先来学习DDL来操作数据库。而操作数据库主要就是对数据库的增删查操作。

### 1.1  查询

查询所有的数据库

```sql
SHOW DATABASES;
```

上述查询到的是的这些数据库是mysql安装好自带的数据库，我们以后不要操作这些数据库。

### 1.2  创建数据库

* **创建数据库**：

```sql
CREATE DATABASE 数据库名称;
```

* **创建数据库(判断，如果不存在则创建)**

```sql
CREATE DATABASE IF NOT EXISTS 数据库名称;
```

### 1.3  删除数据库

* **删除数据库**

```sql
DROP DATABASE 数据库名称;
```

* **删除数据库(判断，如果存在则删除)**

```sql
DROP DATABASE IF EXISTS 数据库名称;
```

### 1.4  使用数据库

数据库创建好了，要在数据库中创建表，得先明确在哪儿个数据库中操作，此时就需要使用数据库。

* **使用数据库**

```sql
USE 数据库名称;
```

* **查看当前使用的数据库**

```sql
SELECT DATABASE();
```

## 2，DDL:操作表(alter)【对表和库的操作】

操作表也就是对表进行增（Create）删（Retrieve）改（Update）查（Delete）。

### 2.1  查询表

* **查询当前数据库下所有表名称**

```sql
SHOW TABLES;
```

我们创建的数据库中没有任何表，因此我们进入mysql自带的mysql数据库，执行上述语句查看

* **查询表结构**

```sql
DESC 表名称;
```

### 2.2  创建表

* **创建表**

```sql
CREATE TABLE 表名 (
	字段名1  数据类型1,
	字段名2  数据类型2,
	…
	字段名n  数据类型n
);

```

> 注意：最后一行末尾，不能加逗号

```sql
create table tb_user (
	id int,
    username varchar(20),
    password varchar(32)
);
```

### 2.3  数据类型

MySQL 支持多种类型，可以分为三类：

* 数值

  ```sql
  tinyint : 小整数型，占一个字节
  int	： 大整数类型，占四个字节
  	eg ： age int
  double ： 浮点类型
  	使用格式： 字段名 double(总长度,小数点后保留的位数)
  	eg ： score double(5,2)   
  decimal ：精准数据类型
      使用格式： 字段名 decimal(总长度,小数点后保留的位数)
      eg：  score decimal(5,2)
  ```

* 日期

  ```sql
  date ： 日期值。只包含年月日
  	eg ：birthday date ： 
  datetime ： 混合日期和时间值。包含年月日时分秒
  ```

* 字符串

  ```sql
  char ： 定长字符串。
  	优点：存储性能高
  	缺点：浪费空间
  	eg ： name char(10)  如果存储的数据字符个数不足10个，也会占10个的空间
  varchar ： 变长字符串。
  	优点：节约空间
  	缺点：存储性能底
  	eg ： name varchar(10) 如果存储的数据字符个数不足10个，那就数据字符个数是几就占几个的空间	
  ```

> 注意：其他类型参考资料中的《MySQL数据类型].xlsx》

**案例：**

```
需求：设计一张学生表，请注重数据类型、长度的合理性
	1. 编号
	2. 姓名，姓名最长不超过10个汉字
	3. 性别，因为取值只有两种可能，因此最多一个汉字
	4. 生日，取值为年月日
	5. 入学成绩，小数点后保留两位
	6. 邮件地址，最大长度不超过 64
	7. 家庭联系电话，不一定是手机号码，可能会出现 - 等字符
	8. 学生状态（用数字表示，正常、休学、毕业...）
```

语句设计如下：

```sql
create table student (
	id int,
    name varchar(10),
    gender char(1),
    birthday date,
    score double(5,2),
    email varchar(15),
    tel varchar(15),
    status tinyint
);
```

### 2.4  删除表

* **删除表**

```sql
DROP TABLE 表名;
```

* **删除表时判断表是否存在**

```sql
DROP TABLE IF EXISTS 表名;
```

运行语句效果如下：

### 2.5  修改表

* **修改表名**

```sql
ALTER TABLE 表名 RENAME TO 新的表名;

-- 将表名student修改为stu
alter table student rename to stu;
```

* **添加一列**

```sql
ALTER TABLE 表名 ADD 列名 数据类型;

-- 给stu表添加一列address，该字段类型是varchar(50)
alter table stu add address varchar(50);
```

* **修改数据类型**

```sql
ALTER TABLE 表名 MODIFY 列名 新数据类型;

-- 将stu表中的address字段的类型改为 char(50)
alter table stu modify address char(50);
```

* **修改列名和数据类型**

```sql
ALTER TABLE 表名 CHANGE 列名 新列名 新数据类型;

-- 将stu表中的address字段名改为 addr，类型改为varchar(50)
alter table stu change address addr varchar(50);
```

* **删除列**

```sql
ALTER TABLE 表名 DROP 列名;

-- 将stu表中的addr字段 删除
alter table stu drop addr;
```

## 3，DML(insert)【对数据的增删改查】

DML主要是对数据进行增（insert）删（delete）改（update）操作。

### 3.1  添加数据

* **给指定列添加数据**

```sql
INSERT INTO 表名(列名1,列名2,…) VALUES(值1,值2,…);
```

* **给全部列添加数据**

```sql
INSERT INTO 表名 VALUES(值1,值2,…);
```

* **批量添加数据**

```sql
INSERT INTO 表名(列名1,列名2,…) VALUES(值1,值2,…),(值1,值2,…),(值1,值2,…)…;
INSERT INTO 表名 VALUES(值1,值2,…),(值1,值2,…),(值1,值2,…)…;
```

* **练习**

为了演示以下的增删改操作是否操作成功，故先将查询所有数据的语句介绍给大家：

```sql
select * from stu;
```



```sql
-- 给指定列添加数据
INSERT INTO stu (id, NAME) VALUES (1, '张三');
-- 给所有列添加数据，列名的列表可以省略的
INSERT INTO stu (id,NAME,sex,birthday,score,email,tel,STATUS) VALUES (2,'李四','男','1999-11-11',88.88,'lisi@itcast.cn','13888888888',1);

INSERT INTO stu VALUES (2,'李四','男','1999-11-11',88.88,'lisi@itcast.cn','13888888888',1);

-- 批量添加数据
INSERT INTO stu VALUES 
	(2,'李四','男','1999-11-11',88.88,'lisi@itcast.cn','13888888888',1),
	(2,'李四','男','1999-11-11',88.88,'lisi@itcast.cn','13888888888',1),
	(2,'李四','男','1999-11-11',88.88,'lisi@itcast.cn','13888888888',1);
```



### 3.2  修改数据

* **修改表数据**

```sql
UPDATE 表名 SET 列名1=值1,列名2=值2,… [WHERE 条件] ;
```

> 注意：
>
> 1. 修改语句中如果不加条件，则将所有数据都修改！
> 2. 像上面的语句中的中括号，表示在写sql语句中可以省略这部分



* **练习**

  * 将张三的性别改为女

    ```sql
    update stu set sex = '女' where name = '张三';
    ```

  * 将张三的生日改为 1999-12-12 分数改为99.99

    ```sql
    update stu set birthday = '1999-12-12', score = 99.99 where name = '张三';
    ```

  * 注意：如果update语句没有加where条件，则会将表中所有数据全部修改！

    ```sql
    update stu set sex = '女';
    ```


### 3.3  删除数据

* **删除数据**

```sql
DELETE FROM 表名 [WHERE 条件] ;
```

* **练习**

```sql
-- 删除张三记录
delete from stu where name = '张三';

-- 删除stu表中所有的数据
delete from stu;
```



## 4，DQL(select)【对数据的查询操作】

接下来我们先介绍查询的完整语法：

```sql
SELECT 
    字段列表
FROM 
    表名列表 
WHERE 
    条件列表
GROUP BY
    分组字段
HAVING
    分组后条件
ORDER BY
    排序字段
LIMIT
    分页限定
```

为了给大家演示查询的语句，我们需要先准备表及一些数据：

```sql
-- 删除stu表
drop table if exists stu;


-- 创建stu表
CREATE TABLE stu (
 id int, -- 编号
 name varchar(20), -- 姓名
 age int, -- 年龄
 sex varchar(5), -- 性别
 address varchar(100), -- 地址
 math double(5,2), -- 数学成绩
 english double(5,2), -- 英语成绩
 hire_date date -- 入学时间
);

-- 添加数据
INSERT INTO stu(id,NAME,age,sex,address,math,english,hire_date) 
VALUES 
(1,'马运',55,'男','杭州',66,78,'1995-09-01'),
(2,'马花疼',45,'女','深圳',98,87,'1998-09-01'),
(3,'马斯克',55,'男','香港',56,77,'1999-09-02'),
(4,'柳白',20,'女','湖南',76,65,'1997-09-05'),
(5,'柳青',20,'男','湖南',86,NULL,'1998-09-01'),
(6,'刘德花',57,'男','香港',99,99,'1998-09-01'),
(7,'张学右',22,'女','香港',99,99,'1998-09-01'),
(8,'德玛西亚',18,'男','南京',56,65,'1994-09-02');
```

接下来咱们从最基本的查询语句开始学起。

### 4.1  基础查询

#### 4.1.1  语法

* **查询多个字段**

```sql
SELECT 字段列表 FROM 表名;
SELECT * FROM 表名; -- 查询所有数据
```

* **去除重复记录**

```sql
SELECT DISTINCT 字段列表 FROM 表名;
```

* **起别名**

```sql
AS: AS 也可以省略
```



#### 4.1.2  练习

* 查询name、age两列

  ```sql
  select name,age from stu;
  ```

* 查询所有列的数据，列名的列表可以使用*替代

  ```sql
  select * from stu;
  ```

  上面语句中的\*不建议大家使用，因为在这写\*不方便我们阅读sql语句。我们写字段列表的话，可以添加注释对每一个字段进行说明

* 查询地址信息

  ```sql
  select address from stu;
  ```

  

  从上面的结果我们可以看到有重复的数据，我们也可以使用 `distinct` 关键字去重重复数据。

* 去除重复记录

  ```sql
  select distinct address from stu;
  ```

* 查询姓名、数学成绩、英语成绩。并通过as给math和english起别名（as关键字可以省略）

  ```sql
  select name,math as 数学成绩,english as 英文成绩 from stu;
  select name,math 数学成绩,english 英文成绩 from stu;
  ```

  

### 4.2  条件查询

#### 4.2.1  语法

```sql
SELECT 字段列表 FROM 表名 WHERE 条件列表;
```

* **条件**

条件列表可以使用以下运算符

![image-20220812163230721]()

#### 4.2.2  条件查询练习

* 查询年龄大于20岁的学员信息

  ```sql
  select * from stu where age > 20;
  ```

* 查询年龄大于等于20岁的学员信息

  ```sql
  select * from stu where age >= 20;
  ```

* 查询年龄大于等于20岁 并且 年龄 小于等于 30岁 的学员信息

  ```sql
  select * from stu where age >= 20 &&  age <= 30;
  select * from stu where age >= 20 and  age <= 30;
  ```

  > 上面语句中 &&  和  and  都表示并且的意思。建议使用 and 。
  >
  > 也可以使用  between ... and 来实现上面需求

  ```sql
  select * from stu where age BETWEEN 20 and 30;
  ```

* 查询入学日期在'1998-09-01' 到 '1999-09-01'  之间的学员信息

  ```sql
  select * from stu where hire_date BETWEEN '1998-09-01' and '1999-09-01';
  ```

* 查询年龄等于18岁的学员信息

  ```sql
  select * from stu where age = 18;
  ```

* 查询年龄不等于18岁的学员信息

  ```sql
  select * from stu where age != 18;
  select * from stu where age <> 18;
  ```

* 查询年龄等于18岁 或者 年龄等于20岁 或者 年龄等于22岁的学员信息

  ```sql
  select * from stu where age = 18 or age = 20 or age = 22;
  select * from stu where age in (18,20 ,22);
  ```

* 查询英语成绩为 null的学员信息

  null值的比较不能使用 =  或者 != 。需要使用 is  或者 is not

  ```sql
  select * from stu where english = null; -- 这个语句是不行的
  select * from stu where english is null;
  select * from stu where english is not null;
  ```

#### 4.2.3  模糊查询练习

> 模糊查询使用like关键字，可以使用通配符进行占位:
>
> （1）_ : 代表单个任意字符
>
> （2）% : 代表任意个数字符

* 查询姓'马'的学员信息

  ```sql
  select * from stu where name like '马%';
  ```

* 查询第二个字是'花'的学员信息  

  ```sql
  select * from stu where name like '_花%';
  ```

* 查询名字中包含 '德' 的学员信息

  ```sql
  select * from stu where name like '%德%';
  ```

  

### 4.3  排序查询

#### 4.3.1  语法

```sql
SELECT 字段列表 FROM 表名 ORDER BY 排序字段名1 [排序方式1],排序字段名2 [排序方式2] …;
```

上述语句中的排序方式有两种，分别是：

* ASC ： 升序排列 **（默认值）**
* DESC ： 降序排列

> 注意：如果有多个排序条件，当前边的条件值一样时，才会根据第二条件进行排序



#### 4.3.2  练习

* 查询学生信息，按照年龄升序排列 

  ```sql
  select * from stu order by age ;
  ```

* 查询学生信息，按照数学成绩降序排列

  ```sql
  select * from stu order by math desc ;
  ```

* 查询学生信息，按照数学成绩降序排列，如果数学成绩一样，再按照英语成绩升序排列

  ```sql
  select * from stu order by math desc , english asc ;
  ```

  

### 4.4  聚合函数

#### 4.4.1  概念

 ==将一列数据作为一个整体，进行纵向计算。==

#### 4.4.2  聚合函数分类

| 函数名      | 功能                             |
| ----------- | -------------------------------- |
| count(列名) | 统计数量（一般选用不为null的列） |
| max(列名)   | 最大值                           |
| min(列名)   | 最小值                           |
| sum(列名)   | 求和                             |
| avg(列名)   | 平均值                           |

#### 8.4.3  聚合函数语法

```sql
SELECT 聚合函数名(列名) FROM 表;
```

> 注意：null 值不参与所有聚合函数运算



#### 4.4.4  练习

* 统计班级一共有多少个学生

  ```sql
  select count(id) from stu;
  select count(english) from stu;
  ```

  上面语句根据某个字段进行统计，如果该字段某一行的值为null的话，将不会被统计。所以可以在count(*) 来实现。\* 表示所有字段数据，一行中也不可能所有的数据都为null，所以建议使用 count(\*)

  ```sql
  select count(*) from stu;
  ```

* 查询数学成绩的最高分

  ```sql
  select max(math) from stu;
  ```

* 查询数学成绩的最低分

  ```sql
  select min(math) from stu;
  ```

* 查询数学成绩的总分

  ```sql
  select sum(math) from stu;
  ```

* 查询数学成绩的平均分

  ```sql
  select avg(math) from stu;
  ```

* 查询英语成绩的最低分

  ```sql
  select min(english) from stu;
  ```

  

### 4.5  分组查询

#### 4.5.1  语法

```sql
SELECT 字段列表 FROM 表名 [WHERE 分组前条件限定] GROUP BY 分组字段名 [HAVING 分组后条件过滤];
```

> 注意：分组之后，查询的字段为聚合函数和分组字段，查询其他字段无任何意义



#### 4.5.2  练习

* 查询男同学和女同学各自的数学平均分

  ```sql
  select sex, avg(math) from stu group by sex;
  ```

  > 注意：分组之后，查询的字段为聚合函数和分组字段，查询其他字段无任何意义

  ```sql
  select name, sex, avg(math) from stu group by sex;  -- 这里查询name字段就没有任何意义
  ```

* 查询男同学和女同学各自的数学平均分，以及各自人数

  ```sql
  select sex, avg(math),count(*) from stu group by sex;
  ```

* 查询男同学和女同学各自的数学平均分，以及各自人数，要求：分数低于70分的不参与分组

  ```sql
  select sex, avg(math),count(*) from stu where math > 70 group by sex;
  ```

* 查询男同学和女同学各自的数学平均分，以及各自人数，要求：分数低于70分的不参与分组，分组之后人数大于2个的

  ```sql
  select sex, avg(math),count(*) from stu where math > 70 group by sex having count(*)  > 2;
  ```

  

**where 和 having 区别：**

* 执行时机不一样：where 是分组之前进行限定，不满足where条件，则不参与分组，而having是分组之后对结果进行过滤。

* 可判断的条件不一样：where 不能对聚合函数进行判断，having 可以。



### 4.6  分页查询

接下来我们先说分页查询的语法。

#### 4.6.1  语法

```sql
SELECT 字段列表 FROM 表名 LIMIT  起始索引 , 查询条目数;
```

> 注意： 上述语句中的起始索引是从0开始



#### 4.6.2  练习

* 从0开始查询，查询3条数据

  ```sql
  select * from stu limit 0 , 3;
  ```

* 每页显示3条数据，查询第1页数据

  ```sql
  select * from stu limit 0 , 3;
  ```

* 每页显示3条数据，查询第2页数据

  ```sql
  select * from stu limit 3 , 3;
  ```

* 每页显示3条数据，查询第3页数据

  ```sql
  select * from stu limit 6 , 3;
  ```

从上面的练习推导出起始索引计算公式：

```sql
起始索引 = (当前页码 - 1) * 每页显示的条数
```



## 5.约束

- 约束

  - 约束是作用于表中列上的规则，用于限制加入表的数据
  - 约束的存在保证了数据库中数据的正确性、有效性和完整性


### 1.2  分类

* **非空约束： 关键字是 NOT NULL**

  保证列中所有的数据不能有null值。

  例如：id列在添加 `马花疼` 这条数据时就不能添加成功。

* **唯一约束：关键字是  UNIQUE**

  保证列中所有数据各不相同。

  例如：id列中三条数据的值都是1，这样的数据在添加时是绝对不允许的。

* **主键约束： 关键字是  PRIMARY KEY**

  主键是一行数据的唯一标识，要求非空且唯一。一般我们都会给没张表添加一个主键列用来唯一标识数据。

  例如：上图表中id就可以作为主键，来标识每条数据。那么这样就要求数据中id的值不能重复，不能为null值。

* **检查约束： 关键字是  CHECK** 

  保证列中的值满足某一条件。

  例如：我们可以给age列添加一个范围，最低年龄可以设置为1，最大年龄就可以设置为300，这样的数据才更合理些。

  > 注意：MySQL不支持检查约束。
  >
  > 这样是不是就没办法保证年龄在指定的范围内了？从数据库层面不能保证，以后可以在java代码中进行限制，一样也可以实现要求。

* **默认约束： 关键字是   DEFAULT**

  保存数据时，未指定值则采用默认值。

  例如：我们在给english列添加该约束，指定默认值是0，这样在添加数据时没有指定具体值时就会采用默认给定的0。

* **外键约束： 关键字是  FOREIGN KEY**

  外键用来让两个表的数据之间建立链接，保证数据的一致性和完整性。

  外键约束现在可能还不太好理解，后面我们会重点进行讲解。

### 1.3  非空约束

* 概念

  非空约束用于保证列中所有数据不能有NULL值

* 语法

  * 添加约束

    ```sql
    -- 创建表时添加非空约束
    CREATE TABLE 表名(
       列名 数据类型 NOT NULL,
       …
    ); 
    
    ```

    ```sql
    -- 建完表后添加非空约束
    ALTER TABLE 表名 MODIFY 字段名 数据类型 NOT NULL;
    ```

  * 删除约束

    ```sql
    ALTER TABLE 表名 MODIFY 字段名 数据类型;
    ```

### 1.4  唯一约束

* 概念

  唯一约束用于保证列中所有数据各不相同**（就是输入的数据不能与之前输入的数据一样）**

* 语法

  * 添加约束

    ```sql
    -- 创建表时添加唯一约束
    CREATE TABLE 表名(
       列名 数据类型 UNIQUE [AUTO_INCREMENT],
       -- AUTO_INCREMENT: 当不指定值时自动增长
       …
    ); 
    CREATE TABLE 表名(
       列名 数据类型,
       …
       [CONSTRAINT] [约束名称] UNIQUE(列名)
    ); 
    ```

    ```sql
    -- 建完表后添加唯一约束
    ALTER TABLE 表名 MODIFY 字段名 数据类型 UNIQUE;
    ```

  * 删除约束

    ```sql
    ALTER TABLE 表名 DROP INDEX 字段名;
    ```

### 1.5  主键约束

* 概念

  主键是一行数据的唯一标识，要求非空且唯一**（就是添加的数据不能为空且不能重复 例如id等）**一张表只能有一个主键

* 语法

  * 添加约束

    ```sql
    -- 创建表时添加主键约束
    CREATE TABLE 表名(
       列名 数据类型 PRIMARY KEY [AUTO_INCREMENT]
        -- 自动增长,当写了auto_increment的时候可以为空也可以写null
       …
    ); 
    CREATE TABLE 表名(
       列名 数据类型,
       [CONSTRAINT] [约束名称] PRIMARY KEY(列名)
    ); 
    
    ```

    ```sql
    -- 建完表后添加主键约束
    ALTER TABLE 表名 ADD PRIMARY KEY(字段名);
    ```

  * 删除约束

    ```sql
    ALTER TABLE 表名 DROP PRIMARY KEY;
    ```

### 1.6  默认约束

* 概念

  保存数据时，未指定值则采用默认值

* 语法

  * 添加约束

    ```sql
    -- 创建表时添加默认约束
    CREATE TABLE 表名(
       列名 数据类型 DEFAULT 默认值,
       …
    ); 
    ```

    ```sql
    -- 建完表后添加默认约束
    ALTER TABLE 表名 ALTER 列名 SET DEFAULT 默认值;
    ```

  * 删除约束

    ```sql
    ALTER TABLE 表名 ALTER 列名 DROP DEFAULT;
    ```

### 1.7  约束练习

**根据需求，为表添加合适的约束**

```sql
-- 员工表
CREATE TABLE emp (
	id INT,  -- 员工id，主键且自增长
    ename VARCHAR(50), -- 员工姓名，非空且唯一
    joindate DATE,  -- 入职日期，非空
    salary DOUBLE(7,2),  -- 工资，非空
    bonus DOUBLE(7,2)  -- 奖金，如果没有将近默认为0
);
```

上面一定给出了具体的要求，我们可以根据要求创建这张表，并为每一列添加对应的约束。建表语句如下：

```sql
DROP TABLE IF EXISTS emp;

-- 员工表
CREATE TABLE emp (
  id INT PRIMARY KEY, -- 员工id，主键且自增长
  ename VARCHAR(50) NOT NULL UNIQUE, -- 员工姓名，非空并且唯一
  joindate DATE NOT NULL , -- 入职日期，非空
  salary DOUBLE(7,2) NOT NULL , -- 工资，非空
  bonus DOUBLE(7,2) DEFAULT 0 -- 奖金，如果没有奖金默认为0
);
```

通过上面语句可以创建带有约束的 `emp` 表，约束能不能发挥作用呢。接下来我们一一进行验证，先添加一条没有问题的数据

```sql
INSERT INTO emp(id,ename,joindate,salary,bonus) values(1,'张三','1999-11-11',8800,5000);
```

* **验证主键约束，非空且唯一**

```sql
INSERT INTO emp(id,ename,joindate,salary,bonus) values(null,'张三','1999-11-11',8800,5000);
```

![image-20220812170356302](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208121703400.png)

从上面的结果可以看到，字段 `id` 不能为null。那我们重新添加一条数据，如下：

```sql
INSERT INTO emp(id,ename,joindate,salary,bonus) values(1,'张三','1999-11-11',8800,5000);
```

执行结果如下：

![image-20220601182025518](https://s2.loli.net/2022/06/01/cxQPRHo2GuCvJDF.png)

从上面结果可以看到，1这个值重复了。所以主键约束是用来限制数据非空且唯一的。那我们再添加一条符合要求的数据

```sql
INSERT INTO emp(id,ename,joindate,salary,bonus) values(2,'李四','1999-11-11',8800,5000);
```

执行结果如下：

![image-20220601182035752](https://s2.loli.net/2022/06/01/PVIo5yUtMBfvDS6.png)

* **验证非空约束**

```sql
INSERT INTO emp(id,ename,joindate,salary,bonus) values(3,null,'1999-11-11',8800,5000);
```

执行结果如下：

![image-20220601182051525](https://s2.loli.net/2022/06/01/x2S6VQyEC9rNn1b.png)

从上面结果可以看到，`ename` 字段的非空约束生效了。

* **验证唯一约束**

```sql
INSERT INTO emp(id,ename,joindate,salary,bonus) values(3,'李四','1999-11-11',8800,5000);
```

执行结果如下：

![image-20220601182058205](https://s2.loli.net/2022/06/01/bZ3on5Nw9MgWzp7.png)

从上面结果可以看到，`ename` 字段的唯一约束生效了。

* **验证默认约束**

```sql
INSERT INTO emp(id,ename,joindate,salary) values(3,'王五','1999-11-11',8800);
```

执行完上面语句后查询表中数据，如下图可以看到王五这条数据的bonus列就有了默认值0。

![image-20220601182105552](https://s2.loli.net/2022/06/01/4XDVCu2LR9aWNkx.png)

==注意：默认约束只有在不给值时才会采用默认值。如果给了null，那值就是null值。==

如下：

```sql
INSERT INTO emp(id,ename,joindate,salary,bonus) values(4,'赵六','1999-11-11',8800,null);
```

执行完上面语句后查询表中数据，如下图可以看到赵六这条数据的bonus列的值是null。

![image-20220601182115467](https://s2.loli.net/2022/06/01/vyUW1X5ca4bAR8s.png)

* **验证自动增长： auto_increment  当列是数字类型 并且唯一约束**

重新创建 `emp` 表，并给id列添加自动增长

```sql
-- 员工表
CREATE TABLE emp (
  id INT PRIMARY KEY auto_increment, -- 员工id，主键且自增长
  ename VARCHAR(50) NOT NULL UNIQUE, -- 员工姓名，非空并且唯一
  joindate DATE NOT NULL , -- 入职日期，非空
  salary DOUBLE(7,2) NOT NULL , -- 工资，非空
  bonus DOUBLE(7,2) DEFAULT 0 -- 奖金，如果没有奖金默认为0
);
```

接下来给emp添加数据，分别验证不给id列添加值以及给id列添加null值，id列的值会不会自动增长：

```sql
INSERT INTO emp(ename,joindate,salary,bonus) values('赵六','1999-11-11',8800,null);
INSERT INTO emp(id,ename,joindate,salary,bonus) values(null,'赵六2','1999-11-11',8800,null);
INSERT INTO emp(id,ename,joindate,salary,bonus) values(null,'赵六3','1999-11-11',8800,null);
```



### 1.8  外键约束(**注意**)

#### 1.8.1  概述

外键用来让两个表的数据之间建立链接，保证数据的一致性和完整性。

如何理解上面的概念呢？如下图有两张表，员工表和部门表：

![image-20220601182128226](https://s2.loli.net/2022/06/01/vAQj4IYyVHU9Oo6.png)

员工表中的dep_id字段是部门表的id字段关联，也就是说1号学生张三属于1号部门研发部的员工。现在我要删除1号部门，就会出现错误的数据（员工表中属于1号部门的数据）。而我们上面说的两张表的关系只是我们认为它们有关系，此时需要通过外键让这两张表产生数据库层面的关系，这样你要删除部门表中的1号部门的数据将无法删除。

#### 1.8.2  语法

* 添加外键约束

```sql
-- 创建表时添加外键约束
CREATE TABLE 表名(
   列名 数据类型,
   …
   [CONSTRAINT] [外键名称] FOREIGN KEY(外键列名) REFERENCES 主表(主表列名) 
   /* 
    [CONSTRAINT] [自定义名称] FOREIGN KEY(主表与外表关联的主表列名) REFERENCES 被关联的表(被关联表的列名) 
    
    */
); 
```

```sql
-- 建完表后添加外键约束
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称);
```

* 删除外键约束

```sql
ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;
```



#### 1.8.3  练习

根据上述语法创建员工表和部门表，并添加上外键约束：

```sql
-- 删除表
DROP TABLE IF EXISTS emp;
DROP TABLE IF EXISTS dept;

-- 部门表
CREATE TABLE dept(
	id int primary key auto_increment,
	dep_name varchar(20),
	addr varchar(20)
);
-- 员工表 
CREATE TABLE emp(
	id int primary key auto_increment,
	name varchar(20),
	age int,
	dep_id int,

	-- 添加外键 dep_id,关联 dept 表的id主键
	CONSTRAINT fk_emp_dept FOREIGN KEY(dep_id) REFERENCES dept(id)	
);
```

添加数据

```sql
-- 添加 2 个部门
insert into dept(dep_name,addr) values
('研发部','广州'),('销售部', '深圳');

-- 添加员工,dep_id 表示员工所在的部门
INSERT INTO emp (NAME, age, dep_id) VALUES 
('张三', 20, 1),
('李四', 20, 1),
('王五', 20, 1),
('赵六', 20, 2),
('孙七', 22, 2),
('周八', 18, 2);
```

此时删除 `研发部` 这条数据，会发现无法删除。

删除外键

```sql
alter table emp drop FOREIGN key fk_emp_dept;
```

重新添加外键

```sql
alter table emp add CONSTRAINT fk_emp_dept FOREIGN key(dep_id) REFERENCES dept(id);
```

## 6，数据库设计

### 2.1  数据库设计简介

* 软件的研发步骤

  ![image-20220601182143782](https://s2.loli.net/2022/06/01/oJzPjX6GHkhE5er.png)

* 数据库设计概念

  * 数据库设计就是根据业务系统的具体需求，结合我们所选用的DBMS，为这个业务系统构造出最优的数据存储模型。
  * 建立数据库中的==表结构==以及==表与表之间的关联关系==的过程。
  * 有哪些表？表里有哪些字段？表和表之间有什么关系？

* 数据库设计的步骤

  * 需求分析（数据是什么? 数据具有哪些属性? 数据与属性的特点是什么）

  * 逻辑分析（通过ER图对数据库进行逻辑建模，不需要考虑我们所选用的数据库管理系统）

    如下图就是ER(Entity/Relation)图：

    ![image-20220601182156014](https://s2.loli.net/2022/06/01/8Wr7wdmyjR56CQ1.png)

  * 物理设计（根据数据库自身的特点把逻辑设计转换为物理设计）

  * 维护设计（1.对新的需求进行建表；2.表优化）

* 表关系

  * 一对一

    * 如：用户 和 用户详情
    * 一对一关系多用于表拆分，将一个实体中经常使用的字段放一张表，不经常使用的字段放另一张表，用于提升查询性能

    ![image-20220812174108289](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208121741429.png)

    上图左边是用户的详细信息，而我们真正在展示用户信息时最长用的则是上图右边红框所示，所以我们会将详细信息查分成两周那个表。

  * 一对多

    * 如：部门 和 员工

    * 一个部门对应多个员工，一个员工对应一个部门。如下图：

      ![image-20220601182207737](https://s2.loli.net/2022/06/01/lyHNQu3ULAB95wn.png)

  * 多对多

    * 如：商品 和 订单

    * 一个商品对应多个订单，一个订单包含多个商品。如下图：

      ![image-20220812174148257](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208121741378.png)

### 2.2  表关系(一对多)

* 一对多

  * 如：部门 和 员工
  * 一个部门对应多个员工，一个员工对应一个部门。

* 实现方式

  ==在多的一方建立外键，指向一的一方的主键==

* 案例

  我们还是以 `员工表` 和 `部门表` 举例:

  ![image-20220601182226799](https://s2.loli.net/2022/06/01/YF8iAWIjRvaBpSw.png)

  经过分析发现，员工表属于多的一方，而部门表属于一的一方，此时我们会在员工表中添加一列（dep_id），指向于部门表的主键（id）：

  ![image-20220601182233734](https://s2.loli.net/2022/06/01/trmk5i7JNxTwVOE.png)

  建表语句如下：

  ```sql
  -- 删除表
  DROP TABLE IF EXISTS tb_emp;
  DROP TABLE IF EXISTS tb_dept;
  
  -- 部门表
  CREATE TABLE tb_dept(
  	id int primary key auto_increment,
  	dep_name varchar(20),
  	addr varchar(20)
  );
  -- 员工表 
  CREATE TABLE tb_emp(
  	id int primary key auto_increment,
  	name varchar(20),
  	age int,
  	dep_id int,
  
  	-- 添加外键 dep_id,关联 dept 表的id主键
  	CONSTRAINT fk_emp_dept FOREIGN KEY(dep_id) REFERENCES tb_dept(id)	
  );
  ```

  查看表结构模型图：

  ![image-20220601182243122](https://s2.loli.net/2022/06/01/XbevgUodVhDAjYK.png)

### 2.3  表关系(多对多)

* 多对多

  * 如：商品 和 订单
  * 一个商品对应多个订单，一个订单包含多个商品

* 实现方式

  ==建立第三张中间表，中间表至少包含两个外键，分别关联两方主键==

* 案例

  我们以 `订单表` 和 `商品表` 举例：

  ![image-20220601182250947](https://s2.loli.net/2022/06/01/BepWkRImOLTh9uV.png)

  经过分析发现，订单表和商品表都属于多的一方，此时需要创建一个中间表，在中间表中添加订单表的外键和商品表的外键指向两张表的主键：

  ![image-20220601182257993](https://s2.loli.net/2022/06/01/FnCz5v1I9uLSdh6.png)

  建表语句如下：

  ```sql
  -- 删除表
  DROP TABLE IF EXISTS tb_order_goods;
  DROP TABLE IF EXISTS tb_order;
  DROP TABLE IF EXISTS tb_goods;
  
  -- 订单表
  CREATE TABLE tb_order(
  	id int primary key auto_increment,
  	payment double(10,2),
  	payment_type TINYINT,
  	status TINYINT
  );
  
  -- 商品表
  CREATE TABLE tb_goods(
  	id int primary key auto_increment,
  	title varchar(100),
  	price double(10,2)
  );
  
  -- 订单商品中间表
  CREATE TABLE tb_order_goods(
  	id int primary key auto_increment,
  	order_id int,
  	goods_id int,
  	count int
  );
  
  -- 建完表后，添加外键
  alter table tb_order_goods add CONSTRAINT fk_order_id FOREIGN key(order_id) REFERENCES tb_order(id);
  alter table tb_order_goods add CONSTRAINT fk_goods_id FOREIGN key(goods_id) REFERENCES tb_goods(id);
  ```

  查看表结构模型图：

  ![image-20220601182306306](https://s2.loli.net/2022/06/01/kBqgHXhKfjx8nYT.png)

### 2.4  表关系(一对一)

* 一对一

  * 如：用户 和 用户详情
  * 一对一关系多用于表拆分，将一个实体中经常使用的字段放一张表，不经常使用的字段放另一张表，用于提升查询性能

* 实现方式

  ==在任意一方加入外键，关联另一方主键，并且设置外键为唯一(UNIQUE)==

* 案例

  我们以 `用户表` 举例：

  ![image-20220601182315935](https://s2.loli.net/2022/06/01/Ulo6pwCPIbnvVAJ.png)

  而在真正使用过程中发现 id、photo、nickname、age、gender 字段比较常用，此时就可以将这张表查分成两张表。

![image-20220601182323435](https://s2.loli.net/2022/06/01/2SxKAYaiMglnZHU.png)

​	

​	建表语句如下：

```sql
create table tb_user_desc (
	id int primary key auto_increment,
	city varchar(20),
	edu varchar(10),
	income int,
	status char(2),
	des varchar(100)
);

create table tb_user (
	id int primary key auto_increment,
	photo varchar(100),
	nickname varchar(50),
	age int,
	gender char(1),
	desc_id int unique,
	-- 添加外键
	CONSTRAINT fk_user_desc FOREIGN KEY(desc_id) REFERENCES tb_user_desc(id)	
);
```

​	查看表结构模型图：

![image-20220812174546112](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208121745217.png)



### 2.5  数据库设计案例

根据下图设计表及表和表之间的关系：

经过分析，我们分为 `专辑表`  `曲目表`  `短评表`  `用户表`   4张表。

![image-20220601182335044](https://s2.loli.net/2022/06/01/85DxHNmVTKuyR3A.png)

一个专辑可以有多个曲目，一个曲目只能属于某一张专辑，所以专辑表和曲目表的关系是==一对多==。

一个专辑可以被多个用户进行评论，一个用户可以对多个专辑进行评论，所以专辑表和用户表的关系是 ==多对多==。

一个用户可以发多个短评，一个短评只能是某一个人发的，所以用户表和短评表的关系是 ==一对多==。

![image-20220812174615820](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208121746934.png)



## 7，多表查询

多表查询顾名思义就是从多张表中一次性的查询出我们想要的数据。我们通过具体的sql给他们演示，先准备环境

```sql
DROP TABLE IF EXISTS emp;
DROP TABLE IF EXISTS dept;


# 创建部门表
	CREATE TABLE dept(
        did INT PRIMARY KEY AUTO_INCREMENT,
        dname VARCHAR(20)
    );

	# 创建员工表
	CREATE TABLE emp (
        id INT PRIMARY KEY AUTO_INCREMENT,
        NAME VARCHAR(10),
        gender CHAR(1), -- 性别
        salary DOUBLE, -- 工资
        join_date DATE, -- 入职日期
        dep_id INT,
        FOREIGN KEY (dep_id) REFERENCES dept(did) -- 外键，关联部门表(部门表的主键)
    );
	-- 添加部门数据
	INSERT INTO dept (dNAME) VALUES ('研发部'),('市场部'),('财务部'),('销售部');
	-- 添加员工数据
	INSERT INTO emp(NAME,gender,salary,join_date,dep_id) VALUES
	('孙悟空','男',7200,'2013-02-24',1),
	('猪八戒','男',3600,'2010-12-02',2),
	('唐僧','男',9000,'2008-08-08',2),
	('白骨精','女',5000,'2015-10-07',3),
	('蜘蛛精','女',4500,'2011-03-14',1),
	('小白龙','男',2500,'2011-02-14',null);	
```

执行下面的多表查询语句

```sql
select * from emp , dept;  -- 从emp和dept表中查询所有的字段数据
```

结果如下：

![image-20220601182359299](https://s2.loli.net/2022/06/01/JyipQumYcFgh1xX.png)

从上面的结果我们看到有一些无效的数据，如 `孙悟空` 这个员工属于1号部门，但也同时关联的2、3、4号部门。所以我们要通过限制员工表中的 `dep_id` 字段的值和部门表 `did` 字段的值相等来消除这些无效的数据，

```sql
select * from emp , dept where emp.dep_id = dept.did;
```

执行后结果如下：

![image-20220601182406288](https://s2.loli.net/2022/06/01/v8lpbKV4UBiWjxu.png)

上面语句就是连接查询，那么多表查询都有哪些呢？

* 连接查询

  ![image-20220601182414601](https://s2.loli.net/2022/06/01/c1bJ4BzHNxIKLVl.png)

  * 内连接查询 ：相当于查询AB交集数据
  * 外连接查询
    * 左外连接查询 ：相当于查询A表所有数据和交集部门数据
    * 右外连接查询 ： 相当于查询B表所有数据和交集部分数据

* 子查询

### 3.1  内连接查询

* 语法

```sql
-- 隐式内连接
SELECT 字段列表 FROM 表1,表2… WHERE 条件;

-- 显示内连接
SELECT 字段列表 FROM 表1 [INNER] JOIN 表2 ON 条件;
```

> 内连接相当于查询 A B 交集数据

![image-20220601182422014](https://s2.loli.net/2022/06/01/oJXsxzkLfBNarqn.png)

* 案例

  * 隐式内连接

    ```sql
    SELECT
    	*
    FROM
    	emp,
    	dept
    WHERE
    	emp.dep_id = dept.did;
    ```

    执行上述语句结果如下：

    ![image-20220601182429576](https://s2.loli.net/2022/06/01/VHJvRbiw4SEfxaD.png)

  * 查询 emp的 name， gender，dept表的dname

    ```sql
    SELECT
    	emp. NAME,
    	emp.gender,
    	dept.dname
    FROM
    	emp,
    	dept
    WHERE
    	emp.dep_id = dept.did;
    ```

    执行语句结果如下：

    ![image-20220601182444141](https://s2.loli.net/2022/06/01/rAheWzXd7vmi1LT.png)

    上面语句中使用表名指定字段所属有点麻烦，sql也支持给表指别名，上述语句可以改进为

    ```sql
    SELECT
    	t1. NAME,
    	t1.gender,
    	t2.dname
    FROM
    	emp t1,
    	dept t2
    WHERE
    	t1.dep_id = t2.did;
    ```

  * 显式内连接

    ```sql
    select * from emp inner join dept on emp.dep_id = dept.did;
    -- 上面语句中的inner可以省略，可以书写为如下语句
    select * from emp  join dept on emp.dep_id = dept.did;
    ```

    执行结果如下：

    ![image-20220601182452918](https://s2.loli.net/2022/06/01/E5vBjhZw2XoQ639.png)

### 3.2  外连接查询

* 语法

  ```sql
  -- 左外连接
  SELECT 字段列表 FROM 表1 LEFT [OUTER] JOIN 表2 ON 条件;
  
  -- 右外连接
  SELECT 字段列表 FROM 表1 RIGHT [OUTER] JOIN 表2 ON 条件;
  ```

  > 左外连接：相当于查询A表所有数据和交集部分数据
  >
  > 右外连接：相当于查询B表所有数据和交集部分数据

  ![image-20220601182459870](https://s2.loli.net/2022/06/01/IHTPSzkcsLXCvWx.png)

* 案例

  * 查询emp表所有数据和对应的部门信息（左外连接）

    ```sql
    select * from emp left join dept on emp.dep_id = dept.did;
    ```

    执行语句结果如下：

    ![image-20220601182508761](https://s2.loli.net/2022/06/01/qbTVlZdmGiFpr98.png)

    结果显示查询到了左表（emp）中所有的数据及两张表能关联的数据。

  * 查询dept表所有数据和对应的员工信息（右外连接）

    ```sql
    select * from emp right join dept on emp.dep_id = dept.did;
    ```

    执行语句结果如下：

    ![image-20220601182516220](https://s2.loli.net/2022/06/01/ljdgUQs3wauTeEf.png)

    结果显示查询到了右表（dept）中所有的数据及两张表能关联的数据。

    要查询出部门表中所有的数据，也可以通过左外连接实现，只需要将两个表的位置进行互换：

    ```sql
    select * from dept left join emp on emp.dep_id = dept.did;
    ```

    

### 3.3  子查询

* 概念

  ==查询中嵌套查询，称嵌套查询为子查询。==

  什么是查询中嵌套查询呢？我们通过一个例子来看：

  **需求：查询工资高于猪八戒的员工信息。**

  来实现这个需求，我们就可以通过二步实现，第一步：先查询出来 猪八戒的工资

  ```sql
  select salary from emp where name = '猪八戒'
  ```

   第二步：查询工资高于猪八戒的员工信息

  ```sql
  select * from emp where salary > 3600;
  ```

  第二步中的3600可以通过第一步的sql查询出来，所以将3600用第一步的sql语句进行替换

  ```sql
  select * from emp where salary > (select salary from emp where name = '猪八戒');
  ```

  这就是查询语句中嵌套查询语句。

* 子查询根据查询结果不同，作用不同

  * 子查询语句结果是**单行单列**，子查询语句作为条件值，使用 =  !=  >  <  等进行条件判断

    ```
    select 字段列表 FROM 表 WHERE 字段名=（子查询）
    ```

    

  * 子查询语句结果是**多行单列**，子查询语句作为条件值，使用 in 等关键字进行条件判断

    ```
    SELECT 字段列表 FROM 表 WHERE 字段名 in（子查询）
    ```

    

  * 子查询语句结果是**多行多列**，子查询语句作为虚拟表

* 案例

  ```
  SELECT 字段列表 FROM （子查询） WHERE 条件
  ```

  

  * 查询 '财务部' 和 '市场部' 所有的员工信息

    ```sql
    -- 查询 '财务部' 或者 '市场部' 所有的员工的部门did
    select did from dept where dname = '财务部' or dname = '市场部';
    
    select * from emp where dep_id in (select did from dept where dname = '财务部' or dname = '市场部');
    ```

  * 查询入职日期是 '2011-11-11' 之后的员工信息和部门信息

    ```sql
    -- 查询入职日期是 '2011-11-11' 之后的员工信息
    select * from emp where join_date > '2011-11-11' ;
    -- 将上面语句的结果作为虚拟表和dept表进行内连接查询
    select * from (select * from emp where join_date > '2011-11-11' ) t1, dept where t1.dep_id = dept.did;
    ```



### 3.4  案例

* 环境准备：

```sql
DROP TABLE IF EXISTS emp;
DROP TABLE IF EXISTS dept;
DROP TABLE IF EXISTS job;
DROP TABLE IF EXISTS salarygrade;

-- 部门表
CREATE TABLE dept (
  did INT PRIMARY KEY PRIMARY KEY, -- 部门id
  dname VARCHAR(50), -- 部门名称
  loc VARCHAR(50) -- 部门所在地
);

-- 职务表，职务名称，职务描述
CREATE TABLE job (
  id INT PRIMARY KEY,
  jname VARCHAR(20),
  description VARCHAR(50)
);

-- 员工表
CREATE TABLE emp (
  id INT PRIMARY KEY, -- 员工id
  ename VARCHAR(50), -- 员工姓名
  job_id INT, -- 职务id
  mgr INT , -- 上级领导
  joindate DATE, -- 入职日期
  salary DECIMAL(7,2), -- 工资
  bonus DECIMAL(7,2), -- 奖金
  dept_id INT, -- 所在部门编号
  CONSTRAINT emp_jobid_ref_job_id_fk FOREIGN KEY (job_id) REFERENCES job (id),
  CONSTRAINT emp_deptid_ref_dept_id_fk FOREIGN KEY (dept_id) REFERENCES dept (did)
);
-- 工资等级表
CREATE TABLE salarygrade (
  grade INT PRIMARY KEY,   -- 级别
  losalary INT,  -- 最低工资
  hisalary INT -- 最高工资
);
				
-- 添加4个部门
INSERT INTO dept(did,dname,loc) VALUES 
(10,'教研部','北京'),
(20,'学工部','上海'),
(30,'销售部','广州'),
(40,'财务部','深圳');

-- 添加4个职务
INSERT INTO job (id, jname, description) VALUES
(1, '董事长', '管理整个公司，接单'),
(2, '经理', '管理部门员工'),
(3, '销售员', '向客人推销产品'),
(4, '文员', '使用办公软件');


-- 添加员工
INSERT INTO emp(id,ename,job_id,mgr,joindate,salary,bonus,dept_id) VALUES 
(1001,'孙悟空',4,1004,'2000-12-17','8000.00',NULL,20),
(1002,'卢俊义',3,1006,'2001-02-20','16000.00','3000.00',30),
(1003,'林冲',3,1006,'2001-02-22','12500.00','5000.00',30),
(1004,'唐僧',2,1009,'2001-04-02','29750.00',NULL,20),
(1005,'李逵',4,1006,'2001-09-28','12500.00','14000.00',30),
(1006,'宋江',2,1009,'2001-05-01','28500.00',NULL,30),
(1007,'刘备',2,1009,'2001-09-01','24500.00',NULL,10),
(1008,'猪八戒',4,1004,'2007-04-19','30000.00',NULL,20),
(1009,'罗贯中',1,NULL,'2001-11-17','50000.00',NULL,10),
(1010,'吴用',3,1006,'2001-09-08','15000.00','0.00',30),
(1011,'沙僧',4,1004,'2007-05-23','11000.00',NULL,20),
(1012,'李逵',4,1006,'2001-12-03','9500.00',NULL,30),
(1013,'小白龙',4,1004,'2001-12-03','30000.00',NULL,20),
(1014,'关羽',4,1007,'2002-01-23','13000.00',NULL,10);


-- 添加5个工资等级
INSERT INTO salarygrade(grade,losalary,hisalary) VALUES 
(1,7000,12000),
(2,12010,14000),
(3,14010,20000),
(4,20010,30000),
(5,30010,99990);
```

* 需求

  1. 查询所有员工信息。查询员工编号，员工姓名，工资，职务名称，职务描述

     ```sql
     /*
     	分析：
     		1. 员工编号，员工姓名，工资 信息在emp 员工表中
     		2. 职务名称，职务描述 信息在 job 职务表中
     		3. job 职务表 和 emp 员工表 是 一对多的关系 emp.job_id = job.id
     */
     -- 方式一 ：隐式内连接
     SELECT
     	emp.id,
     	emp.ename,
     	emp.salary,
     	job.jname,
     	job.description
     FROM
     	emp,
     	job
     WHERE
     	emp.job_id = job.id;
     
     -- 方式二 ：显式内连接
     SELECT
     	emp.id,
     	emp.ename,
     	emp.salary,
     	job.jname,
     	job.description
     FROM
     	emp
     INNER JOIN job ON emp.job_id = job.id;
     ```

  2. 查询员工编号，员工姓名，工资，职务名称，职务描述，部门名称，部门位置

     ```sql
     /*
     	分析：
     		1. 员工编号，员工姓名，工资 信息在emp 员工表中
     		2. 职务名称，职务描述 信息在 job 职务表中
     		3. job 职务表 和 emp 员工表 是 一对多的关系 emp.job_id = job.id
     
     		4. 部门名称，部门位置 来自于 部门表 dept
     		5. dept 和 emp 一对多关系 dept.id = emp.dept_id
     */
     
     -- 方式一 ：隐式内连接
     SELECT
     	emp.id,
     	emp.ename,
     	emp.salary,
     	job.jname,
     	job.description,
     	dept.dname,
     	dept.loc
     FROM
     	emp,
     	job,
     	dept
     WHERE
     	emp.job_id = job.id
     	and dept.id = emp.dept_id
     ;
     
     -- 方式二 ：显式内连接
     SELECT
     	emp.id,
     	emp.ename,
     	emp.salary,
     	job.jname,
     	job.description,
     	dept.dname,
     	dept.loc
     FROM
     	emp
     INNER JOIN job ON emp.job_id = job.id
     INNER JOIN dept ON dept.id = emp.dept_id
     ```

  3. 查询员工姓名，工资，工资等级

     ```sql
     /*
     	分析：
     		1. 员工姓名，工资 信息在emp 员工表中
     		2. 工资等级 信息在 salarygrade 工资等级表中
     		3. emp.salary >= salarygrade.losalary  and emp.salary <= salarygrade.hisalary
     */
     SELECT
     	emp.ename,
     	emp.salary,
     	t2.*
     FROM
     	emp,
     	salarygrade t2
     WHERE
     	emp.salary >= t2.losalary
     AND emp.salary <= t2.hisalary
     ```

  4. 查询员工姓名，工资，职务名称，职务描述，部门名称，部门位置，工资等级

     ```sql
     /*
     	分析：
     		1. 员工编号，员工姓名，工资 信息在emp 员工表中
     		2. 职务名称，职务描述 信息在 job 职务表中
     		3. job 职务表 和 emp 员工表 是 一对多的关系 emp.job_id = job.id
     
     		4. 部门名称，部门位置 来自于 部门表 dept
     		5. dept 和 emp 一对多关系 dept.id = emp.dept_id
     		6. 工资等级 信息在 salarygrade 工资等级表中
     		7. emp.salary >= salarygrade.losalary  and emp.salary <= salarygrade.hisalary
     */
     SELECT
     	emp.id,
     	emp.ename,
     	emp.salary,
     	job.jname,
     	job.description,
     	dept.dname,
     	dept.loc,
     	t2.grade
     FROM
     	emp
     INNER JOIN job ON emp.job_id = job.id
     INNER JOIN dept ON dept.id = emp.dept_id
     INNER JOIN salarygrade t2 ON emp.salary BETWEEN t2.losalary and t2.hisalary;
     ```

  5. 查询出部门编号、部门名称、部门位置、部门人数

     ```sql
     /*
     	分析：
     		1. 部门编号、部门名称、部门位置 来自于部门 dept 表
     		2. 部门人数: 在emp表中 按照dept_id 进行分组，然后count(*)统计数量
     		3. 使用子查询，让部门表和分组后的表进行内连接
     */
     -- 根据部门id分组查询每一个部门id和员工数
     select dept_id, count(*) from emp group by dept_id;
     
     SELECT
     	dept.id,
     	dept.dname,
     	dept.loc,
     	t1.count
     FROM
     	dept,
     	(
     		SELECT
     			dept_id,
     			count(*) count
     		FROM
     			emp
     		GROUP BY
     			dept_id
     	) t1
     WHERE
     	dept.id = t1.dept_id
     ```

     

## 8，事务

### 4.1  概述

> 数据库的事务（Transaction）是一种机制、一个操作序列，包含了==一组数据库操作命令==。
>
> 事务把所有的命令作为一个整体一起向系统提交或撤销操作请求，即这一组数据库命令==要么同时成功，要么同时失败==。
>
> 事务是一个不可分割的工作逻辑单元。

这些概念不好理解，接下来举例说明，如下图有一张表

![image-20220601182534835](https://s2.loli.net/2022/06/01/VLoMOKgN6wrRnDZ.png)

张三和李四账户中各有100块钱，现李四需要转换500块钱给张三，具体的转账操作为

* 第一步：查询李四账户余额
* 第二步：从李四账户金额 -500
* 第三步：给张三账户金额 +500

现在假设在转账过程中第二步完成后出现了异常第三步没有执行，就会造成李四账户金额少了500，而张三金额并没有多500；这样的系统是有问题的。如果解决呢？使用事务可以解决上述问题

![image-20220601182542856](https://s2.loli.net/2022/06/01/i8VSCTwd2vN1L5j.png)

从上图可以看到在转账前开启事务，如果出现了异常回滚事务，三步正常执行就提交事务，这样就可以完美解决问题。

### 4.2  语法

* 开启事务

  ```sql
  START TRANSACTION;
  或者  
  BEGIN;
  ```

* 提交事务

  ```sql
  commit;
  ```

* 回滚事务

  ```sql
  rollback;
  ```

  

### 4.3  代码验证

* 环境准备

  ```sql
  DROP TABLE IF EXISTS account;
  
  -- 创建账户表
  CREATE TABLE account(
  	id int PRIMARY KEY auto_increment,
  	name varchar(10),
  	money double(10,2)
  );
  
  -- 添加数据
  INSERT INTO account(name,money) values('张三',1000),('李四',1000);
  ```

  

* 不加事务演示问题

  ```sql
  -- 转账操作
  -- 1. 查询李四账户金额是否大于500
  
  -- 2. 李四账户 -500
  UPDATE account set money = money - 500 where name = '李四';
  
  出现异常了...  -- 此处不是注释，在整体执行时会出问题，后面的sql则不执行
  -- 3. 张三账户 +500
  UPDATE account set money = money + 500 where name = '张三';
  ```

  整体执行结果肯定会出问题，我们查询账户表中数据，发现李四账户少了500。

  ![image-20220601182553565](https://s2.loli.net/2022/06/01/M15wICJkQGOPgxo.png)

* 添加事务sql如下：

  ```sql
  -- 开启事务
  BEGIN;
  -- 转账操作
  -- 1. 查询李四账户金额是否大于500
  
  -- 2. 李四账户 -500
  UPDATE account set money = money - 500 where name = '李四';
  
  出现异常了...  -- 此处不是注释，在整体执行时会出问题，后面的sql则不执行
  -- 3. 张三账户 +500
  UPDATE account set money = money + 500 where name = '张三';
  
  -- 提交事务
  COMMIT;
  
  -- 回滚事务
  ROLLBACK;
  ```

  上面sql中的执行成功进选择执行提交事务，而出现问题则执行回滚事务的语句。以后我们肯定不可能这样操作，而是在java中进行操作，在java中可以抓取异常，没出现异常提交事务，出现异常回滚事务。

### 4.4  事务的四大特征

*  原子性（Atomicity）: 事务是不可分割的最小操作单位，要么同时成功，要么同时失败

* 一致性（Consistency） :事务完成时，必须使所有的数据都保持一致状态

* 隔离性（Isolation） :多个事务之间，操作的可见性

* 持久性（Durability） :事务一旦提交或回滚，它对数据库中的数据的改变就是永久的

> ==说明：==
>
> mysql中事务是自动提交的。
>
> 也就是说我们不添加事务执行sql语句，语句执行完毕会自动的提交事务。
>
> 可以通过下面语句查询默认提交方式：
>
> ```java
> SELECT @@autocommit;
> ```
>
> 查询到的结果是1 则表示自动提交，结果是0表示手动提交。当然也可以通过下面语句修改提交方式
>
> ```sql
> set @@autocommit = 0;
> ```


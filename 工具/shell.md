# 1. Shell 概述

![image-20230711154032955](https://picgo-img-pic.oss-cn-guangzhou.aliyuncs.com/img/202307111540118.png)

**1）**  **Linux** 提供的 **Shell** 解析器有

```
[atguigu@hadoop101 ~]$ cat /etc/shells

/bin/sh

/bin/bash

/usr/bin/sh

/usr/bin/bash

/bin/tcsh

/bin/csh
```

**2）**  **bash** 和 **sh** 的关系

```
[atguigu@hadoop101 bin]$ ll | grep bash -rwxr-xr-x. 1 root root 941880 5月 11 2016 bash lrwxrwxrwx. 1 root root  4 5月 27 2017 sh -> bash
```



**3）**  **Centos** 默认的解析器是 **bash**

```
[atguigu@hadoop101 bin]$ echo $SHELL
/bin/bash
```

# 2. **Shell** 脚本入门(格式及其执行)

在~目录下先创建一个放shell脚本的目录然后创建脚本文件再使用vim进行编辑

```
[root@localhost ~]# mkdir scripts
[root@localhost ~]# cd scripts
```

**1） **脚本格式

```
脚本以#!/bin/bash 开头（指定解析器）
```

**2）**  第一个 **Shell** 脚本：**helloworld.sh**

（1）需求：创建一个 Shell 脚本，输出 helloworld

（2）**案例实操**：

```
[root@localhost scripts]# touch  helloworld.sh
[root@localhost scripts]# vim helloworld.s
在helloworld.sh中输入如下内容  #!/bin/bash  echo "helloworld"  
```

（3）**脚本的常用执行方式第一种**：**采用 bash 或 sh+脚本的相对路径或绝对路径（不用赋予脚本+x 权限） sh+脚本的相对路径**

```
[root@localhost scripts]# bash helloworld.sh  //在文件的目录下直接执行不用写绝对路径
[root@localhost scripts]# cd ~ //切换到其他路径
[root@localhost ~]# bash scripts/helloworld.sh // 相对路径
[root@localhost ~]# bash /root/scripts/helloworld.sh // 绝对路径
// 由于sh是bash的主链接 所以可以使用sh代替bash来进行执行
[root@localhost ~]# sh scripts/helloworld.sh
[root@localhost scripts]# helloworld //脚本输出结果
```

第二种：采用输入脚本的绝对路径或相对路径执行脚本（必须具有可执行权限+x）

```
①首先要赋予 helloworld.sh 脚本的+x 权限
[root@localhost ~]# chmod +X scripts/helloworld.sh
②执行脚本相对路径
[root@localhost scripts]# ./helloworld.sh
绝对路径
[root@localhost ~]# /root/scripts/helloworld.sh
```

- 注意：**第一种**执行方法，**本质是 bash 解析器帮你执行脚本**，所以脚本本身不需要执行权限。**第二种**执行方法，**本质是脚本需要自己执行**，所以需要执行权限。

【了解】第三种：在脚本的路径前加上“.”或者 source

```
[root@localhost ~]# source ./helloworld.sh // source
[root@localhost ~]# . ./helloworld.sh // .
```

①有以下脚本

```
[atguigu@hadoop101 shells]$ cat test.sh  
#!/bin/bash  A=5  echo $A //脚本内容
```

②分别使用 sh，bash，./ 和 . 的方式来执行，结果如下：

```
[atguigu@hadoop101 shells]$ bash  test.sh
[atguigu@hadoop101 shells]$ echo $A  

[atguigu@hadoop101 shells]$ sh test.sh 
[atguigu@hadoop101 shells]$ echo $A

[atguigu@hadoop101 shells]$ ./test.sh 
[atguigu@hadoop101 shells]$ echo $A

[atguigu@hadoop101 shells]$ . test.sh
[atguigu@hadoop101 shells]$ echo $A 5 // 注意多个5
```

- 原因：前两种方式都是在当前 shell 中打开一个子 shell 来执行脚本内容，当脚本内容结束，则子 shell 关闭，回到父 shell 中。

- 第三种，也就是使用在脚本路径前加“.”或者 source 的方式，可以使脚本内容在当前

shell 里执行，而无需打开子 shell！这也是为什么我们每次要修改完/etc/profile 文件以后，需要 source 一下的原因。

- 开子 shell 与不开子 shell 的区别就在于，环境变量的继承关系，**如在子 shell 中设置的当前变量，父 shell 是不可见的**。

#  **3**.变量

## 3.1 系统预定义变量

**1）**常用系统变量

$HOME、$PWD、$SHELL、$USER 等

**2）**案例实操

```
（1）查看系统变量的值
[atguigu@hadoop101 shells]$ echo $HOME
/home/atguigu
(2)查看所有系统变量
[atguigu@hadoop101 shells]$ env | less
(3)显示当前 Shell 中所有变量：set
[atguigu@hadoop101 shells]$ set
```

## 3.2 自定义变量

**1）**  基本语法

- 定义变量：变量名=变量值，注意，= 号前后不能有空格

  - ```
    [root@localhost ~]# a=15 // 定义变量
    [root@localhost ~]# echo $a //输出a的变量值
    [root@localhost ~]# 15 // 结果
    ```

-  撤销变量：unset 变量名

  - ```
    [root@localhost ~]# unset a  //撤销变量a
    [root@localhost ~]# echo $a  //输出a的变量值
    [root@localhost ~]#          // 结果空白表示删除成功
    ```

- 声明静态变量：readonly 变量，注意：不能 unset

  - ```
    [root@localhost ~]# readonly a=15 //声名静态变量
    [root@localhost ~]# echo $a  //输出a的变量值
    [root@localhost ~]# unset a  //撤销变量a
    -bash: unset: a: 无法反设定: 只读 variable  // 无法撤销警告
    ```

- 当使用bash命令（或者前两种方式运行shell脚本）时代表在子shell中执行命令，此时定义的变量为**局部变量**



- 在 bash 中，变量默认类型都是字符串类型，无法直接进行数值运算（   使用**$[]** 或 **$( ( ) )**   ）

  - ```
    [root@localhost ~]# c=1+2
    [root@localhost ~]# echo $c
    1+2
    
    [root@localhost scripts]# a=$((1+5))  //这种方法可以使变量相加
    [root@localhost scripts]# echo a
    6
    
    [root@localhost scripts]# b=$[1+6]   //这种方法可以使变量相加
    [root@localhost scripts]# echo $b
    7
    ```

    

- 变量的值如果有空格，需要使用双引号或单引号括起来

  - ```
    [root@localhost ~]# string= i am strong
    bash: i: 未找到命令
    [root@localhost ~]# string="i am strong"
    [root@localhost ~]# echo $string
    i am strong
    ```

- 可把变量提升为全局环境变量，可供其他 Shell 程序使用（**export 变量名**）

  - 注意：如果想要使用export提升为全局变量，**在进入子shell后使用export后使用exit退出子shell会使变量消失导致其他的shell无法使用this.子shell的变量**，因此还是得**写shell脚本的方式进行变量赋值**，在主shell中运行shll脚本进行全局变量的提升。

    - ```
      # 1.在子shell中进行export提升全局变量
      [root@localhost ~]# bash  // 进入子shell
      [root@localhost ~]# ps -f  // 查看进程，看子shell是否运行
      UID         PID   PPID  C STIME TTY          TIME CMD
      root      86900  86896  0 10:39 pts/0    00:00:00 -bash
      root      87251  86900  0 10:40 pts/0    00:00:00 bash   // 说明子shell在运行
      root      87335  87251  0 10:40 pts/0    00:00:00 ps -f
      [root@localhost ~]# c=10  // 定义变量c
      [root@localhost ~]# echo $c  // 在子shell中输出变量c的值
      10
      [root@localhost ~]# export c //在子shell中将变量c提升为全局变量
      [root@localhost ~]# exit  // **** 退出子shell进入主shell进行验证
      exit
      [root@localhost ~]# ps -f // 查看是否退出子shell
      UID         PID   PPID  C STIME TTY          TIME CMD
      root      86900  86896  0 10:39 pts/0    00:00:00 -bash
      root      88013  86900  0 10:40 pts/0    00:00:00 ps -f
      [root@localhost ~]# echo $c
      							// 输出空白 说明变量消失c升为全局变量失败(直接就是c变量已经被清除)		
      ```

    - ```
      # 2.在主shell中进行export提升全局变量
      [root@localhost ~]# ps -f  //查看子shell是否运行中 无bash证明无子shell运行
      UID         PID   PPID  C STIME TTY          TIME CMD
      root      92858  92854  0 10:45 pts/0    00:00:00 -bash
      root      93092  92858  0 10:45 pts/0    00:00:00 ps -f
      [root@localhost ~]# c=10 //在主shell中定义变量c的值
      [root@localhost ~]# echo $c // 输出变量c的值
      10
      [root@localhost ~]# export c  //在主shell中将变量c提升为全局变量
      [root@localhost ~]# bash  // 运行子shell并进入子shell
      [root@localhost ~]# ps -f  //查看子shell是否运行
      UID         PID   PPID  C STIME TTY          TIME CMD
      root      92858  92854  0 10:45 pts/0    00:00:00 -bash
      root      93506  92858  0 10:45 pts/0    00:00:00 bash  // 子shell运行中
      root      93562  93506  0 10:45 pts/0    00:00:00 ps -f
      [root@localhost ~]# echo $c //在子shell中输出c的变量值
      10  				// 输出成功，证明在主shell中进行提升全局变量成功(因为没有使用exit退出，变量没有被清空)
      ```

- 总结：使用exit退出系统会使变量消失，解决办法下方给出（现在还未整理），因此定义变量和提升全局变量要使用shell脚本进行(**说到底还是在主shell中进行**)

  - ```
    [root@localhost ~]# B=10
    [root@localhost ~]# echo $B
    10
    [root@localhost ~]# cd scripts
    [root@localhost scripts]# vim helloworld.sh
    
    在 helloworld.sh 文件中增加 echo $B
    #!/bin/bash
    echo "helloworld" echo $B 
    
    [root@localhost scripts]# sh helloworld.sh  
    helloworld
    						// 这里是输出空白
    [root@localhost scripts]# export B
    [root@localhost scripts]# sh helloworld.sh 
    helloworld
    10						// 输出成功 说明export成功
    ```

    

## **3.3** 特殊变量

### 3.3.1 $n

- 基本语法
  1. $n （功能描述：n 为数字，$0 代表该脚本名称，$1-$9 代表第一到第九个参数，十以上的参数，十以上的参数需要用大括号包含，如${10}）
  2. 在shell脚本中**' '(单引号)**不会让shell脚本识别**$n**里面的n参数，如果使用echo'=====$n======'只会原封不动的输出里面的东西。而使用**""(双引号)**就会使shell脚本识别**$n**,认为这是个参数.
  3. 案例实操

```
# 先在~目录下的script创建一个案例parameter.sh 
[root@localhost scripts]# touch parameter.sh
[root@localhost scripts]# vim parameter.sh 

在shell脚本中输入
#!/bin/bash
echo '==========$n=========='  //注意单引号和双引号的区别
echo $0 
echo $1 
echo $2

[root@localhost scripts]# sh parameter.sh   //执行shell脚本 此时还未输入$1，$2的值
=======$n=======
parameter.sh
						// $1 还没有值
						// $2 还没有值	
[root@localhost scripts]# sh parameter.sh ljw god   //执行shell脚本 此时还输入$1，$2的值ljw，god
=======$n=======
parameter.sh 			//$0
ljw						//$1
god						//$2
```

### 3.3.2 $#

- 基本语法
  1. $# （功能描述：获取所有输入参数个数，常用于循环,判断参数的个数是否正确以及加强脚本的健壮性）。
  2.  案例实操

```
[root@localhost scripts]# vim parameter.sh 

在shell脚本中添加
echo '=======$#======='
echo $#					// 统计参数个数

[root@localhost scripts]# sh parameter.sh ljw god
=======$n=======
parameter.sh
ljw
god
=======$#=======
2
```

### 3.3.3 $*、$@

- 基本语法
  1. $* （功能描述：这个变量代表命令行中所有的参数，$*把所有的参数看成一个整体）
  2. $@ （功能描述：这个变量也代表命令行中所有的参数，不过$@把每个参数区分对待）

- 案例实操

  - ```
    [root@localhost scripts]# vim parameter.sh
    
    在shell脚本中添加
    echo '=======$*======='
    echo $*
    echo '=======$@======='
    echo $@
    
    [root@localhost scripts]# sh parameter.sh a b c d e f  // 输入了6个参数
    =======$n=======
    parameter.sh
    a						// 打印第一个参数
    b						// 打印第二个参数
    =======$#=======
    6						// 统计出输入参数的个数			
    =======$*=======
    a b c d e f				// 显示所有参数 相当于a b c d e f
    =======$@=======
    a b c d e f				// 显示所有参数 相当于数组[a,b,c,d,e,f] --> 可以用于循环遍历
    ```

### 3.3.4 $？

- **1）**  基本语法

- $？（功能描述：最后一次执行的命令的返回状态。如果这个变量的值为 0，证明上一个命令正确执行；如果这个变量的值为非 0（具体是哪个数，由命令自己来决定），则证明上一个命令执行不正确了。）

  - 案例

    - ```
      // 脚本执行成功
      [root@localhost scripts]# sh parameter.sh 
      [root@localhost scripts]# echo $?
      0
      
      // 脚本执行失败
      [root@localhost scripts]# parameter.sh 
      -bash: parameter.sh: 未找到命令
      [root@localhost scripts]# echo $?
      127
      ```

      

# 4.运算符

- 基本语法

  - “$((运算式))” 或 “$[运算式]”

    - ```
      [root@localhost scripts]# s=$[(2+3)*4]
      [root@localhost scripts]# echo $s
      20
      ```

  
  - val=`expr 运算式`
  
    - 表达式和运算符之间要有空格。例如 2+2 是不对的，必须写成 2 + 2。
  
    - 使用的是反引号（Esc键下面那个），而不是单引号。
  
    - ```
      a=10
      b=20
       
      val=`expr $a + $b`
      echo "a + b : $val"
       
      val=`expr $a - $b`
      echo "a - b : $val"
       
      val=`expr $a \* $b`    #乘号(*)前边必须加反斜杠(\)
      echo "a * b : $val"
       
      val=`expr $b / $a`
      echo "b / a : $val"
       
      val=`expr $b % $a`
      echo "b % a : $val"
       
      if [ $a == $b ]
      then
         echo "a 等于 b"
      fi
       
      if [ $a != $b ]
      then
         echo "a 不等于 b"
      fi
      ```
  
      

#  5.条件判断

- 基本语法
  1. test condition
  2. [ condition ]（**注意 condition 前后要有空格**）
  3. 注意：**条件非空即为 true，[ atguigu ]返回 true，[ ] 返回 false。**

- 常用判断条件

  - ```
    按照文件类型进行判断
    -e	文件存在（existence）
    -f	文件存在并且是一个常规的文件（file）
    -d 文件存在并且是一个目录（directory）
    ```
  
    
  
  - ```
    按照文件权限进行判断
    -r 有读的权限（read）
    -w	有写的权限（write）
    -x	有执行的权限（execute）
    ```
  
    
  
  - ```
    	-eq 等于（equal）			-ne 不等于（not equal）
    	-lt 小于（less than）		-le 小于等于（less equal）
    	-gt 大于（greater than）	-ge 大于等于（greater equal）
    注：如果是字符串之间的比较，用等号“=”判断相等；用“!=”判断不等。
    ```
  

- 实际案例

  - ```
    注意 [ ] 前后要有空格
    [root@localhost ~]# [ 2 -ge 3 ]
    [root@localhost ~]# echo $?
    1
    ```

  - ```
    // 文件权限
    [root@localhost scripts]# [ -w helloworld.sh ]
    [root@localhost scripts]# echo $?
    0
    ```

  - ```
    // 文件类型
    // 查看home目录下scripts是否存在
    [root@localhost scripts]# [ -e /home/scritps ]
    [root@localhost scripts]# echo $?
    1
    ```

  - ```shell
    多条件判断（&& 表示前一条命令执行成功时，才执行后一条命令，|| 表示上一条命令执行失败后，才执行下一条命令）
    [root@localhost ~]# [ ljw ] && echo ok || echo ontok
    ok
    
    [root@localhost ~]# [ ] && echo ok || echo notok
    notok
    ```

    

 # 6.流程控制

## 6.1 if判断

- 基本语法（2写法）

  - ```shell
    #单分支写法一
    if [ 条件判断式 ];then
    	程序
    fi
    
    #单分支写法二
    if [ 条件判断式 ]
    then
    	程序
    fi
    
    #多分支
    if [ 条件判断式 ]
    then
    	程序
    elif [ 条件判断式 ]
    then
    	程序
    else
    	程序
    fi
    ```

- 注意事项： ①[ 条件判断式 ]，**中括号**和**条件判断式**之间必须有空格 ②**if 后要有空格**

  - ```shell
    [root@localhost scripts]# touch if.sh
    [root@localhost scripts]# touch if.sh
    # 在if.sh中输入
    #!/bin/bash
    if [ $1 -eq 1 ]
    then
            echo "i am the best"
    elif [ $1 -eq 2 ]
    then
            echo "you are the best too!"
    fi
    
    [root@localhost scripts]# sh if.sh 1
    i am the best
    [root@localhost scripts]# sh if.sh 2
    you are the best too!
    ```

    

## 6.2 case语句

- 基本语句

  - ```shell
    case $变量名 in
    "值1"）
    如果变量的值等于值1，则执行程序1
    ;;
    "值2"）
    如果变量的值等于值2，则执行程序2
    ;;
    …省略其他分支…
    *）
    如果变量的值都不是以上的值，则执行此程序
    ;; esac
    ```

  - 注意事项：

    1. case 行尾必须为单词“in”，每一个模式匹配必须以右括号“）”结束。
    2. 双分号“**;;**”表示命令序列结束，相当于 java 中的 break。
    3. 最后的“*）”表示默认模式，相当于 java 中的 default。

- 案例实操

  - ```shell
    [root@localhost scripts]# touch case.sh
    [root@localhost scripts]# vim case.sh 
    #在case.sh中输入
    #!/bin/bash
    case $1 in
    "ljw")
            echo "i am the best"
    ;;
    "god")
            echo "you are the best too"
    ;;
    esac
    
    [root@localhost scripts]# sh case.sh ljw
    i am the best
    [root@localhost scripts]# sh case.sh god
    you are the best too
    ```

    

## 6.3 for循环

- 基本语法1

  - ```shell
    for ((初始值;循环控制条件;变量变量变化))
    do 
    程序
    done
    ```

  - ```shell
    # 案例实操从 1 加到 100
    [root@localhost scripts]# touch for1.sh
    [root@localhost scripts]# vim for1.sh 
    # 在for1.sh中输入
    #!/bin/bash
    sum=0
    for ((i=0;i<=100;i++))
    do
            sum=$[$sum+$i]
    done
    echo "1加到100的值为：" $sum
    
    [root@localhost scripts]# sh for1.sh 
    1加到100的值为： 5050
    ```

    

- 基本语法2

  - ```shell
    for 变量值 in 值1 值2 值3 ...
    do
    程序
    done
    ```

  - ```shell
    # 实操案例2 打印所有输入参数
    [root@localhost scripts]# touch for2.sh
    [root@localhost scripts]# vim for2.sh
    #在for2.sh中输入
    #!/bin/bash
    for a in $1 $2 $3
    do
            echo "输入的参数为："$a
    done
    
    [root@localhost scripts]# sh for2.sh ljw god jjking
    输入的参数为：ljw
    输入的参数为：god
    输入的参数为：jjking
    ```

    - 比较 **$*** 和 **$@** 区别

      - $*和$@都表示传递给函数或脚本的所有参数，不被双引号“”包含时，都以$1 $2 …$n的形式输出所有参数。

      - ```shell
        [root@localhost scripts]# touch for3.sh
        [root@localhost scripts]# vim for3.sh 
        
        #在for3.sh中输入
        #!/bin/bash
        echo '========$*========='
        for i in $*
        do
                echo "没有双引号时的$*输出的参数" $i
        done
        echo '========$@========='
        for j in $@
        do
                 echo "没有双引号时的$@输出的参数" $j
        done
        
        
        [root@localhost scripts]# sh for3.sh ljw god jjking
        ========$*=========
        没有双引号时的ljw god jjking输出的参数 ljw
        没有双引号时的ljw god jjking输出的参数 god
        没有双引号时的ljw god jjking输出的参数 jjking
        ========$@=========
        没有双引号时的ljw god jjking输出的参数 ljw
        没有双引号时的ljw god jjking输出的参数 god
        没有双引号时的ljw god jjking输出的参数 jjking
        ```

        

      - 当它们被双引号“”包含时，**$*会将所有的参数作为一个*整体*，以“$1 $2 …$n”的形式输出所有参数**；**$@会将各个参数*分开*，以“$1” “$2”…“$n”的形式输出所有参数**。

      - ```shell
        [root@localhost scripts]# touch for4.sh
        [root@localhost scripts]# vim for4.sh 
        
        #在for4.sh中输入
        #!/bin/bash
        echo '============$*============='
        for i in "$*"
        #$*中的所有参数看成是一个整体，所以这个 for 循环只会循环一次
        do
                echo '$*有双引号时输出的参数为'$i
        done
        echo '============$*============='
        for j in "$@"
        #$@中的每个参数都看成是独立的，所以“$@”中有几个参数，就会循环几次
        do
                echo '$@有双引号时输出的参数为'$j
        done
        
        [root@localhost scripts]# sh for4.sh ljw god jjking
        ============$*=============
        $*有双引号时输出的参数为ljw god jjking
        ============$*=============
        $@有双引号时输出的参数为ljw
        $@有双引号时输出的参数为god
        $@有双引号时输出的参数为jjking
        ```

    

## 6.4 while循环

- 基本语法

  - ```
    while [ 条件判断式 ]
    do
    	程序
    done
    ```

  - ```shell
    [root@localhost scripts]# touch while.sh
    [root@localhost scripts]# vim while.sh 
    
    # 在while.sh 中输入
    #!/bin/bash
    sum=0
    i=1
    while [ $i -le 100 ]
    do
            sum=$[$sum+$i]
            i=$[$i+1]
    done
    echo $sum
    
    [root@localhost scripts]# sh while.sh 
    5050
    ```

    

# 7.read读取控制台输入

- 基本语法

  - ```
    read	(选项)	(参数)
    ①选项：
    -p：指定读取值时的提示符；
    -t：指定读取值时等待的时间（秒）如果-t 不加表示一直等待
    ②参数变量：指定读取值的变量名
    ```

  - ```shell
    [root@localhost scripts]# read -t 7 -p "enter your name in 7 second:"  NN ; echo $NN
    enter your name in 7 second:hh  //分号前执行的语句
    hh								// 分号后执行的语句
    ```

    

# 8.函数

## 8.1 系统函数

### 8.1.1 basename

- 基本语法

  - ```
    basename [string / pathname] [suffix] 
    （功能描述：basename 命令会删掉所有的前缀包括最后一个（‘/’）字符，然后将字符串显示出来。
    可以理解为取路径里的文件名称选项：suffix 为后缀，如果 suffix 被指定了，basename 会将 pathname 或 string 中的 suffix 去掉。
    ```

  - ```shell
    #实操案例
    # 选取/usr/test2.txt的文件名
    [root@localhost usr]# basename /usr/test2.txt  
    test2.txt
    [root@localhost usr]# basename /usr/test2.txt .txt //指定文件的后缀
    test2
    [root@localhost usr]# 
    ```

    

### 8.1.2 dirname

- 1）基本语法 dirname 文件绝对路径 

  - ```
    （功能描述：从给定的包含绝对路径的文件名中去除文件名（非目录的部分），然后返回剩下的路径（目录的部分）） 
    dirname 可以理解为取文件路径的绝对路径名称
    ```

  - ```shell
    # 实操案例
    # 选取/usr/test2.txt的绝对路径名称
    [root@localhost usr]# dirname /usr/test2.txt
    /usr
    ```



# 8.2 自定义函数

- 基本语法

  - ```
    [ function ] funname[()]
    {
    	Action;
    	[return int;] 
    }
    ```

  - ```shell
    [root@localhost scripts]# touch fun.sh
    [root@localhost scripts]# vim fun.sh 
    #在fun.sh中输入
    
    #!/bin/bash
    function sum()
    {
            s=0
            s=$[$1+$2]
            echo $s
    }
    read -p "请输入第一个数字：" n1;
    read -p "请输入第二个数字：" n2;
    sum $n1 $n2    // 调用sum函数 
    
    [root@localhost scripts]# sh fun.sh
    请输入第一个数字：1
    请输入第二个数字：2
    3
    ```

    


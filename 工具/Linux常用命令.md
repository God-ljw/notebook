#  Linux目录结构

![image-20230624175940074](https://picgo-img-pic.oss-cn-guangzhou.aliyuncs.com/img/202306241759819.png)

![image-20230622191736734](https://picgo-img-pic.oss-cn-guangzhou.aliyuncs.com/img/202306221917211.png)



| 目录      | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| /bin      | 二进制文件，存放一些常用命令                                 |
| **/boot** | 这里存放的是启动 Linux 时使用的一些核心文件，包括一些连接文件以及镜像文件。自己安装的东西别放这里 |
| /dev      | （Device）**存放Linux的外部设备文件**，在Linux中访问设备和访问文件的方式是相同的 |
| **/etc**  | **etc** 是 Etcetera(其他) 的缩写,这个目录用来存放所有的**系统管理所需要的配置文件**和子目录 |
| /home     | 用户的主目录，在 Linux 中，每个用户都有一个自己的目录，一般该目录是以用户的账号命名 |
| /root     | 存该目录为系统管理员，也称作超级权限者的用户主目录。         |
| /sbin     | **sbin**是 Superuser Binaries (超级用户的二进制文件) 的缩写，这里**存放的是系统管理员使用的系统管理程序**。 |
| /temp     | **tmp** 是 temporary(临时) 的缩写这个目录是用来存放一些临时文件的。 |
| **/usr**  | **usr** 是 unix shared resources(共享资源) 的缩写，这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于 windows 下的 program files 目录。 |
| /opt      | **opt** 是 optional(可选) 的缩写，这是给主机**额外安装软件所摆放的目录**。默认是空的 |
| /lib      | 存放系统的动态链接共享库等，类似于Windows里的DLL文件。几乎所有的应该程序的需要用到这些共享库。 |
| /proc     | proc 是 Processes(进程) 的缩写，/proc 是一种伪文件系统（也即虚拟文件系统），存储的是当前内核运行状态的一系列特殊文件，这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。 |
| /var      | 存放运行时需要改变数据的文件，例如日志文件                   |



# 4.2 文件目录操作命令

## 4.2.1 ls（ll）

```
作用: 显示指定目录下的内容
语法: ls [-al] [dir]
说明: 
	-a 显示所有文件及目录 (. 开头ll的隐藏文件也会列出)
	-l 除文件名称外，同时将文件型态(d表示目录，-表示文件)、权限、拥有者、文件大小等信息详细列出
	
注意: 
	由于我们使用ls命令时经常需要加入-l选项，所以Linux为ls -l命令提供了一种简写方式，即ll
	
常见用法: 
	ls -al 	查看当前目录的所有文件及目录详细信息
	ls -al /etc   查看/etc目录下所有文件及目录详细信息
	ll  	查看当前目录文件及目录的详细信息 
	
-a ：全部的档案，连同隐藏档( 开头为 . 的档案) 一起列出来～
-A ：全部的档案，连同隐藏档，但不包括 . 与 .. 这两个目录，一起列出来～
-d ：仅列出目录本身，而不是列出目录内的档案数据
-f ：直接列出结果，而不进行排序 (ls 预设会以档名排序！)
-F ：根据档案、目录等信息，给予附加数据结构，例如：
*：代表可执行档； 
/：代表目录； 
=：代表 socket 档案； 
|：代表 FIFO 档案；
-h ：将档案容量以人类较易读的方式(例如 GB, KB 等等)列出来；
-i ：列出 inode 位置，而非列出档案属性；
-l ：长数据串行出，包含档案的属性等等数据；
-n ：列出 UID 与 GID 而非使用者与群组的名称 (UID与GID会在账号管理提到！)
-r ：将排序结果反向输出，例如：原本档名由小到大，反向则为由大到小；
-R ：连同子目录内容一起列出来；
-S ：以档案容量大小排序！
-t ：依时间排序

--color=never ：不要依据档案特性给予颜色显示；

--color=always ：显示颜色

--color=auto ：让系统自行依据设定来判断是否给予颜色

--full-time ：以完整时间模式 (包含年、月、日、时、分) 输出

--time={atime,ctime} ：输出 access 时间或 改变权限属性时间 (ctime)
而非内容变更时间 (modification time)
例如：

ls [-aAdfFhilRS] 目录名称

ls [--color={none,auto,always}] 目录名称

ls [--full-time] 目录名称
```



**操作示例:**

![image-20210810193149925](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121629095.png) 

 ![image-20210810193302047](https://picgo-img-pic.oss-cn-guangzhou.aliyuncs.com/img/202409122310071.png)



![image-20210810193605487](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121629817.png) 



![image-20210810194129919](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121629121.png) 



## 4.2.2 cd

```
作用: 用于切换当前工作目录，即进入指定目录
语法: cd [dirName]
	
特殊说明: 
	~	表示用户的home目录
	. 	表示目前所在的目录
	.. 	表示目前目录位置的上级目录
	
举例: 
	cd 	..		切换到当前目录的上级目录
	cd 	~		切换到用户的home目录
	cd 	/usr/local	切换到/usr/local目录
```

> 备注: 
>
> ​	用户的home目录 
>
> ​	root用户	/root
>
> ​	其他用户	/home/xxx



操作示例: 

![image-20210810230949775](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121629622.png) 

![image-20210810231024129](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121629758.png) 

cd .. 切换到当前目录位置的上级目录; 可以通过 cd ../.. 来切换到上级目录的上级目录。



## 4.2.3 cat

```
作用: 用于显示文件内容
语法: cat [-n] fileName

说明:
	-n: 由1开始对所有输出的行数编号

举例:
	cat /etc/profile		查看/etc目录下的profile文件内容
```



**操作演示:** 

![image-20210810231651338](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121630386.png) 

cat 指令会一次性查看文件的所有内容，如果文件内容比较多，这个时候查看起来就不是很方便了，这个时候我们可以通过一个新的指令more。



## 4.2.4 more

```
作用: 以分页的形式显示文件内容
语法: more fileName

操作说明:
    回车键 	向下滚动一行
    空格键 	向下滚动一屏
    b 		返回上一屏
    q或者Ctrl+C	退出more
	
举例：
	more /etc/profile		以分页方式显示/etc目录下的profile文件内容
```



**操作示例：**

<img src="https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121630532.png" alt="image-20210810232212430" style="zoom:80%;" /> 

当我们在查看一些比较大的文件时，我们可能需要经常查询文件尾部的数据信息，那这个时候如果文件很大，我们要一直向下翻页，直到最后一页，去看最新添加的数据，这种方式就比较繁琐了，此时，我们可以借助于tail指令。



## 4.2.5 tail

```
作用: 查看文件末尾的内容
语法: tail [-f] fileName

说明:
	-f : 动态读取文件末尾内容并显示，通常用于日志文件的内容输出
	
举例: 
tail /etc/profile		显示/etc目录下的profile文件末尾10行的内容
tail -20 /etc/profile	显示/etc目录下的profile文件末尾20行的内容
tail -f /itcast/my.log	动态读取/itcast目录下的my.log文件末尾内容并显示
```



**操作示例：** 

A. 默认查询文件尾部10行记录

![image-20210810232758510](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121630575.png) 

B. 可以通过指定参数设置查询尾部指定行数的数据

![image-20210810232947018](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121630278.png) 

C. 动态读取文件尾部的数据

![image-20210810233514284](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121630694.png) 

在窗口1中执行指令 `tail -f 1.txt` 动态查看文件尾部的数据。然后在顶部的标签中右键选择 "复制标签"，打开新的窗口2 , 此时再新打开的窗口2中执行指令 `echo 1 >> 1.txt` , 往1.txt文件尾部追加内容，然后我们就可以在窗口1中看到最新的文件尾部的数据。

如果我们不想查看文件尾部的数据了，可以直接使用快捷键 Ctrl+C ， 结束当前进程。



## 4.2.6 mkdir

```
作用: 创建目录
语法: mkdir [-p] dirName

说明: 
	-p: 确保目录名称存在，不存在的就创建一个。通过此选项，可以实现多层目录同时创建
	-m, --mode=模式，设定权限<模式> (类似 chmod)，而不是 rwxrwxrwx 减 umask
	-v, --verbose 每次创建新目录都显示信息

举例: 
    mkdir itcast  在当前目录下，建立一个名为itcast的子目录
    mkdir -p itcast/test   在工作目录下的itcast目录中建立一个名为test的子目录，若itcast目录不存在，则建立一个
```



**操作演示:**

![image-20210810234541073](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121630238.png) 





## 4.2.7 rmdir

```
作用: 删除空目录
语法: rmdir [-p] dirName

说明:
	-p: 当子目录被删除后使父目录为空目录的话，则一并删除
	-v --verbose 显示指令执行过程
举例:
    rmdir itcast   删除名为itcast的空目录
    rmdir -p itcast/test   删除itcast目录中名为test的子目录，若test目录删除后itcast目录变为空目录，则也被删除
    rmdir itcast*   删除名称以itcast开始的空目录
```



**操作演示:** 

A. 删除空目录

![image-20210810235044921](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121630160.png) 



B. 删除非空目录

![image-20210810235221722](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121630731.png) 



C. 使用*通配符删除目录

![image-20210810235305140](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121630287.png) 

> *: 是一个通配符，代表任意字符； 
>
> rmdir  itcast* : 删除以itcast开头的目录
>
> rmdir  *itcast : 删除以itcast结尾的目录





## 4.2.8 rm

```
作用: 删除文件或者目录
语法: rm [-rf] name

说明: 
    -r: 将目录及目录中所有文件（目录）逐一删除，即递归删除
    -f: 无需确认，直接删除
	-i:互动模式，在删除前会询问用户是否操作
举例: 
    rm -r itcast/     删除名为itcast的目录和目录中所有文件，删除前需确认
    rm -rf itcast/    无需确认，直接删除名为itcast的目录和目录中所有文件
    rm -f hello.txt   无需确认，直接删除hello.txt文件

```



**操作示例:** 

![image-20210810235809473](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121630661.png) 



==注意: 对于 rm -rf xxx 这样的指令，在执行的时候，一定要慎重，确认无误后再进行删除，避免误删。==



## 4.2.9 echo 

```
作用：将一些数据移到文件中。例如，如果要将文本 “Hello，我的名字叫 John” 添加到名为 name.txt 的文件中，则可以键入 
例子：echo Hello, my name is John >> name.txt
	echo '内容' >> 日志文件.log 
```

## 4.3.0 less

```
作用：less 可以使用 [pageup] [pagedown] 等按键的功能来往前往后翻看文件，更容易用来查看一个文件的内容！除此之外，在 less 里头可以拥有更多的搜索功能，不止可以向下搜，也可以向上搜。

语法：less [参数] 文件 
参数：
-b <缓冲区大小> 设置缓冲区的大小
-e 当文件显示结束后，自动离开
-f 强迫打开特殊文件，例如外围设备代号、目录和二进制文件
-g 只标志最后搜索的关键词
-i 忽略搜索时的大小写
-m 显示类似more命令的百分比
-N 显示每行的行号
-o <文件名> 将less 输出的内容在指定文件中保存起来
-Q 不使用警告音
-s 显示连续空行为一行
-S 行过长时间将超出部分舍弃
-x <数字> 将"tab"键显示为规定的数字空格
/字符串：向下搜索"字符串"的功能
?字符串：向上搜索"字符串"的功能
n：重复前一个搜索（与 / 或 ? 有关）
N：反向重复前一个搜索（与 / 或 ? 有关）
b 向上翻一页
d 向后翻半页
h 显示帮助界面
Q 退出less 命令
u 向前滚动半页
y 向前滚动一行
空格键 滚动一页
回车键 滚动一行
[pagedown]： 向下翻动一页
[pageup]： 向上翻动一页

```

![image-20230211121444399](https://cdn.jsdelivr.net/gh/God-ljw/picbed@main/img3/202302111214461.png)

## 4.3.1 diff

```
作用：比较两个文件或目录不同
语法：diff[参数][文件1或目录1][文件2或目录2]

参数：
-<行数> 　指定要显示多少行的文本。此参数必须与-c或-u参数一并使用。
-a或--text 　diff预设只会逐行比较文本文件。
-b或--ignore-space-change 　不检查空格字符的不同。
-B或--ignore-blank-lines 　不检查空白行。
-c 　显示全部内文，并标出不同之处。
-C<行数>或--context<行数> 　与执行"-c-<行数>"指令相同。
-d或--minimal 　使用不同的演算法，以较小的单位来做比较。
-D<巨集名称>或ifdef<巨集名称> 　此参数的输出格式可用于前置处理器巨集。
-e或--ed 　此参数的输出格式可用于ed的script文件。
-f或-forward-ed 　输出的格式类似ed的script文件，但按照原来文件的顺序来显示不同处。
-H或--speed-large-files 　比较大文件时，可加快速度。
-I<字符或字符串>或--ignore-matching-lines<字符或字符串> 　若两个文件在某几行有所不同，而这几行同时都包含了选项中指定的字符或字符串，则不显示这两个文件的差异。
-i或--ignore-case 　不检查大小写的不同。
-l或--paginate 　将结果交由pr程序来分页。
-n或--rcs 　将比较结果以RCS的格式来显示。
-N或--new-file 　在比较目录时，若文件A仅出现在某个目录中，预设会显示：
Only in目录：文件A若使用-N参数，则diff会将文件A与一个空白的文件比较。
-p 　若比较的文件为C语言的程序码文件时，显示差异所在的函数名称。
-P或--unidirectional-new-file 　与-N类似，但只有当第二个目录包含了一个第一个目录所没有的文件时，才会将这个文件与空白的文件做比较。
-q或--brief 　仅显示有无差异，不显示详细的信息。
-r或--recursive 　比较子目录中的文件。
-s或--report-identical-files 　若没有发现任何差异，仍然显示信息。
-S<文件>或--starting-file<文件> 　在比较目录时，从指定的文件开始比较。
-t或--expand-tabs 　在输出时，将tab字符展开。
-T或--initial-tab 　在每行前面加上tab字符以便对齐。
-u,-U<列数>或--unified=<列数> 　以合并的方式来显示文件内容的不同。
-v或--version 　显示版本信息。
-w或--ignore-all-space 　忽略全部的空格字符。
-W<宽度>或--width<宽度> 　在使用-y参数时，指定栏宽。
-x<文件名或目录>或--exclude<文件名或目录> 　不比较选项中所指定的文件或目录。
-X<文件>或--exclude-from<文件> 　您可以将文件或目录类型存成文本文件，然后在=<文件>中指定此文本文件。
-y或--side-by-side 　以并列的方式显示文件的异同之处。

例子：diff aaa bbb
```

## 4.3.2 touch

```
作用：创建新的空白文件
例子：touch/home/username/test.txt
```

## 4.3.3 head

```
作用：用于查看任何文本文件的第一行。默认情况下，它将显示前十行，但是您可以根据自己的喜好更改此数字
例如：head -n 5 text.txt
```



# 4.3 拷贝移动命令

## 4.3.1 cp

```
作用: 用于复制文件或目录
语法: cp [-r] source dest

说明: 
	-r: 如果复制的是目录需要使用此选项，此时将复制该目录下所有的子目录和文件
	-a ：将文件的特性一起复制
	-p ：连同文件的属性一起复制，而非使用默认方式，与-a相似，常用于备份
	-i ：若目标文件已经存在时，在覆盖时会先询问操作的进行
	-u ：目标文件与源文件有差异时才会复制
举例: 
    cp hello.txt itcast/            将hello.txt复制到itcast目录中
    cp hello.txt ./hi.txt           将hello.txt复制到当前目录，并改名为hi.txt  .代表当前目录
    cp -r itcast/ ./itheima/    	将itcast目录和目录下所有文件复制到itheima目录下
    cp -r itcast/* ./itheima/ 	 	将itcast目录下所有文件复制到itheima目录下
```



**操作示例:** 

![image-20210811180508369](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121630491.png) 

![image-20210811180638556](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121630156.png) 

![image-20210811180914417](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121630764.png) 

如果拷贝的内容是目录，需要加上参数 -r 





## 4.3.2 mv

```
作用: 为文件或目录改名、或将文件或目录移动到其它位置
语法: mv source dest
说明：
-f ：force强制的意思，如果目标文件已经存在，不会询问而直接覆盖
-i ：若目标文件已经存在，就会询问是否覆盖
-u ：若目标文件已经存在，且比目标文件新，才会更新

举例: 
    mv hello.txt hi.txt                 将hello.txt改名为hi.txt
    mv hi.txt itheima/                  将文件hi.txt移动到itheima目录中
    mv hi.txt itheima/hello.txt   		将hi.txt移动到itheima目录中，并改名为hello.txt
    mv itcast/ itheima/                 如果itheima目录不存在，将itcast目录改名为itheima
    mv itcast/ itheima/                 如果itheima目录存在，将itcast目录移动到itheima目录中
```



**操作示例:** 

mv 命令既能够改名，又可以移动，具体是改名还是移动,系统会根据我们输入的参数进行判定(如果第二个参数dest是一个已存在的目录,将执行移动操作,其他情况都是改名)

![image-20210811184240003](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121630117.png)





# 4.4 打包压缩命令

```
作用: 对文件进行打包、解包、压缩、解压
语法: tar  [-zcxvf]  fileName  [files]
    包文件后缀为.tar表示只是完成了打包，并没有压缩
    包文件后缀为.tar.gz表示打包的同时还进行了压缩

说明:
    -z: z代表的是gzip，通过gzip命令处理文件，gzip可以对文件压缩或者解压
    -c: c代表的是create，即创建新的包文件 对一堆文件进行打包 打成一个包 
    -x: x代表的是extract，实现从包文件中还原文件
    -v: v代表的是verbose，显示命令的执行过程
    -f: f代表的是file，用于指定包文件的名称

举例：
    打包
        tar -cvf hello.tar ./*		  		将当前目录下所有文件打包，打包后的文件名为hello.tar
        tar -zcvf hello.tar.gz ./*		  	将当前目录下所有文件打包并压缩，打包后的文件名为hello.tar.gz
		
    解包
        tar -xvf hello.tar		  			将hello.tar文件进行解包，并将解包后的文件放在当前目录
        tar -zxvf hello.tar.gz		  		将hello.tar.gz文件进行解压，并将解压后的文件放在当前目录
        tar -zxvf hello.tar.gz -C /usr/local     将hello.tar.gz文件进行解压，并将解压后的文件放在/usr/local目录

```

```
gzip 命令压缩文件或文件夹为 .gz文件：



gzip[参数][文件或者目录]

-a or --ascii 　使用ASCII文字模式。

-c or --stdout or --to-stdout 　把压缩后的文件输出到标准输出设备，不去更动原始文件。

-d or --decompress or ----uncompress 　解开压缩文件。

-f or --force 　强行压缩文件。不理会文件名称 or 硬连接是否存在以及该文件是否为符号连接。

-h or --help 　在线帮助。

-l or --list 　列出压缩文件的相关信息。

-L or --license 　显示版本与版权信息。

-n or --no-name 　压缩文件时，不保存原来的文件名称及时间戳记。

-N or --name 　压缩文件时，保存原来的文件名称及时间戳记。

-q or --quiet 　不显示警告信息。

-r or --recursive 　递归处理，将指定目录下的所有文件及子目录一并处理。

-S<压缩字尾字符串> or ----suffix<压缩字尾字符串> 　更改压缩字尾字符串。

-t or --test 　测试压缩文件是否正确无误。

-v or --verbose 　显示指令执行过程。

-V or --version 　显示版本信息。

-num 用指定的数字num调整压缩的速度，-1 or --fast表示最快压缩方法（低压缩比），-9 or --best表示最慢压缩方法（高压缩比）。系统缺省值为6。
```

**操作示例:** 

A. 打包

![image-20210811185728541](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121631535.png) 



B. 打包并压缩

![image-20210811190035256](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121630823.png) 



C. 解包

![image-20210811190307630](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121631321.png) 



D. 解压

![image-20210811190450820](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121631461.png) 

解压到指定目录,需要加上参数 -C

![image-20210811190626414](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121631760.png) 





# 4.5 文本编辑命令

文本编辑的命令，主要包含两个: vi 和 vim，两个命令的用法类似，我们课程中主要讲解vim的使用。



## 4.5.1 vi&vim介绍

作用: vi命令是Linux系统提供的一个文本编辑工具，可以对文件内容进行编辑，类似于Windows中的记事本

语法: vi fileName

说明: 
  1). vim是从vi发展来的一个功能更加强大的文本编辑工具，编辑文件时可以对文本内容进行着色，方便我们对文件进行编辑处理，所以实际工作中vim更加常用。
  2). 要使用vim命令，需要我们自己完成安装。可以使用下面的命令来完成安装：`yum install vim`



## 4.5.2 vim安装

命令： yum install vim

![image-20210811192217715](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121631209.png) 

安装过程中，会有确认提示，此时输入 y，然后回车，继续安装：

![image-20210811192256269](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121631924.png) 

![image-20210811192350907](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121631552.png) 



## 4.5.3 vim使用

作用: 对文件内容进行编辑，vim其实就是一个文本编辑器
语法: vim fileName
说明: 
	1). 在使用vim命令编辑文件时，如果指定的文件存在则直接打开此文件。如果指定的文件不存在则新建文件。
	2). vim在进行文本编辑时共分为三种模式，分别是 命令模式（Command mode），插入模式（Insert mode）和底行模式（Last line mode）。这三种模式之间可以相互切换。我们在使用vim时一定要注意我们当前所处的是哪种模式。



三种模式: 
    - 命令模式
      A. 命令模式下可以查看文件内容、移动光标（上下左右箭头、gg、G）
      B. 通过vim命令打开文件后，默认进入命令模式
      C. 另外两种模式需要首先进入命令模式，才能进入彼此

      | 命令模式指令 | 含义                              |
      | ------------ | --------------------------------- |
      | gg           | 定位到文本内容的第一行            |
      | G            | 定位到文本内容的最后一行          |
      | dd           | 删除光标所在行的数据              |
      | ndd          | 删除当前光标所在行及之后的n行数据 |
      | u            | 撤销操作                          |
      | shift+zz     | 保存并退出                        |
      | i 或 a 或 o  | 进入插入模式                      |


​      

   - 插入模式
     A. 插入模式下可以对文件内容进行编辑
     B. 在命令模式下按下[i,a,o]任意一个，可以进入插入模式。进入插入模式后，下方会出现【insert】字样
     C. 在插入模式下按下ESC键，回到命令模式

     

   - 底行模式
     A. 底行模式下可以通过命令对文件内容进行查找、显示行号、退出等操作
     B. 在命令模式下按下[:,/]任意一个，可以进入底行模式
     C. 通过/方式进入底行模式后，可以对文件内容进行查找
     D. 通过:方式进入底行模式后，可以输入wq（保存并退出）、q!（不保存退出）、set nu（显示行号）

     | 底行模式命令 | 含义                                 |
     | ------------ | ------------------------------------ |
     | :wq          | 保存并退出                           |
     | :q!          | 不保存退出                           |
     | :set nu      | 显示行号                             |
     | :set nonu    | 取消行号显示                         |
     | :n           | 定位到第n行, 如 :10 就是定位到第10行 |

​     <img src="https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121631455.png" alt="image-20210811194627474" style="zoom: 80%;" /> 

​	

**操作示例:** 

![image-20210811200005097](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121631928.png) 



| 语法         | 功能描述                      |
| ------------ | ----------------------------- |
| yy           | 复制光标当前一行              |
| y或数字y     | 复制一段（从第几行到第几行）  |
| p            | 箭头移动到目的的行粘贴        |
| u            | 撤销上一步                    |
| dd           | 删除光标当前行                |
| d数字d       | 删除光标（含）后多少行        |
| x            | 剪切一个字母，相对于del       |
| X            | 剪切一个字母，相当于Backspace |
| yw           | 复制一个词                    |
| dw           | 删除一个词                    |
| shift+6(^)   | 移动到行尾                    |
| shift+4($)   | 移动到页头，数字              |
| 1+shift+g    | 移动到页头、数字              |
| shift+g      | 移动到页尾                    |
| 数字+shift+g | 移动到目标行                  |



# 4.6 查找命令

## 文件或目录的三种时间

```
Linux下的文件或目录有三种时间：

访问时间（Atime）：记录该文件被访问的最后一次的时间，即Atime。

修改时间（Mtime）：当对这个文件内容进行修改后，Modify显示的时间就会更新一次，即Mtime。

状态改变时间（Ctime）：当文件的内容、更改文件权限、链接属性时随文件的Inode更改而改变的时间，即Ctime
```

```
touch不仅可以创建文件，还可以修改文件的时间。
格式：touch 参数  文件名
参数：
-a:或--time=atime或--time=access或--time=use
-c:或--no-creat，如果文件不存在，也不创建任何文档
-d:使用指定的日期时间，可以使用不同的格式
-m:或--time=mtime或--time=modify，改变修改时间
-r:把指定的文件日期更设成和参考文档或目录日期相同的时间
-t:使用指定的日期时间，格式与date指令相同

touch -d  时间  文件名
touch -d "时间"  文件名
```



## 4.6.0 stat

```
stat命令用于显示文件的状态信息。stat命令的输出信息比ls命令的输出信息要更详细
File：显示文件名
Size：显示文件大小
Blocks：文件使用的数据块总数
IO Block：IO块大小
regular file：文件类型（常规文件）
Device：设备编号
Inode：Inode号
Links：链接数
Access：文件的权限
Gid、Uid：文件所有权的Gid和Uid
access time：表示我们最后一次访问（仅仅是访问，没有改动）文件的时间
modify time：表示我们最后一次修改文件的时间
change time：表示我们最后一次对文件属性改变的时间，包括权限，大小，属性等等
Birth time : 文件创建时间，crtime，不过据查此属性linux已废弃，目前状态显示结果均为-
```

```
[root@localhost log]# stat ~/scripts/for4.sh

  文件："/root/scripts/for4.sh"
  大小：406             块：8          IO 块：4096   普通文件
设备：fd00h/64768d      Inode：17154101    硬链接：1
权限：(0644/-rw-r--r--)  Uid：(    0/    root)   Gid：(    0/    root)
环境：unconfined_u:object_r:admin_home_t:s0
最近访问：2023-07-20 09:51:41.591914242 +0800
最近更改：2023-07-18 15:32:47.289323561 +0800
最近改动：2023-07-18 15:32:47.290323565 +0800
创建时间：-
```



## 4.6.1 find

```
作用: 在指定目录下查找文件
语法: find dirName -option fileName
举例:
    find  .  –name "*.java"			在当前目录及其子目录下查找.java结尾文件
    find  /itcast  -name "*.java"	在/itcast目录及其子目录下查找.java结尾的文件
    
# 与用户或用户组名有关的参数：

-user name : 列出文件所有者为name的文件

-group name : 列出文件所属用户组为name的文件

-uid n : 列出文件所有者为用户ID为n的文件

-gid n : 列出文件所属用户组为用户组ID为n的文件

# 例如：

find /home/hadoop -user hadoop # 在目录/home/hadoop中找出所有者为hadoop的文件


# 与文件权限及名称有关的参数：

-name filename ：找出文件名为filename的文件
-size [+-]SIZE ：找出比SIZE还要大（+）或小（-）的文件
-tpye TYPE ：查找文件的类型为TYPE的文件，TYPE的值主要有：一般文件（f)、设备文件（b、c）、
目录（d）、连接文件（l）、socket（s）、FIFO管道文件（p）；
-perm mode ：查找文件权限刚好等于mode的文件，mode用数字表示，如0755；
-perm -mode ：查找文件权限必须要全部包括mode权限的文件，mode用数字表示
-perm +mode ：查找文件权限包含任一mode的权限的文件，mode用数字表示

# 例如：
find / -name passwd # 查找文件名为passwd的文件
find . -perm 0755 # 查找当前目录中文件权限的0755的文件
find . -size +12k # 查找当前目录中大于12KB的文件，注意c表示byte
```

```
搜索在过去1天内未被使用过的执行文件
[root@localhost ~]# find ~/scripts -type f -atime 1
/root/scripts/for1.sh
/root/scripts/for2.sh
/root/scripts/for3.sh
/root/scripts/while.sh
/root/scripts/fun.sh


搜索在10天内被创建或者修改过的文件
[root@localhost ~]# find ~/scripts -type f -mtime -10
/root/scripts/helloworld.sh
/root/scripts/parameter.sh
/root/scripts/if.sh
/root/scripts/case.sh
/root/scripts/for1.sh
/root/scripts/for2.sh
/root/scripts/for3.sh
/root/scripts/for4.sh
/root/scripts/while.sh
/root/scripts/fun.sh
```

**操作示例:** 

![image-20210811200438459](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121631712.png) 





## 4.6.2 grep

```
作用: 从指定文件中查找指定的文本内容
语法: grep word fileName
举例: 
    grep Hello HelloWorld.java	查找HelloWorld.java文件中出现的Hello字符串的位置
    grep hello *.java			查找当前目录中所有.java结尾的文件中包含hello字符串的位置
```



**操作示例:** 

![image-20210811200737596](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202209121631246.png)



## 4.6.3 pwd

```
作用：pwd命令，作用为查看”当前工作目录“的完整路径
语法：pwd -P # 显示出实际路径，而非使用连接（link）路径；pwd显示的是连接路径
```

## 4.6.4 df

```
作用： 命令用于显示目前在 Linux 系统上的文件系统磁盘使用情况统计
语法：df [选项]... [FILE]...

文件-a, --all 包含所有的具有 0 Blocks 的文件系统
文件--block-size={SIZE} 使用 {SIZE} 大小的 Blocks
文件-h, --human-readable 使用人类可读的格式(预设值是不加这个选项的...)
文件-H, --si 很像 -h, 但是用 1000 为单位而不是用 1024
文件-i, --inodes 列出 inode 资讯，不列出已使用 block
文件-k, --kilobytes 就像是 --block-size=1024
文件-l, --local 限制列出的文件结构
文件-m, --megabytes 就像 --block-size=1048576
文件--no-sync 取得资讯前不 sync (预设值)
文件-P, --portability 使用 POSIX 输出格式
文件--sync 在取得资讯前 sync
文件-t, --type=TYPE 限制列出文件系统的 TYPE
文件-T, --print-type 显示文件系统的形式
文件-x, --exclude-type=TYPE 限制列出文件系统不要显示 TYPE
文件-v (忽略)
文件--help 显示这个帮手并且离开
文件--version 输出版本资讯并且离开

例子：df test i
	df -i
```

## 4.6.5 du

```
作用：显示目录或文件的大小
语法：du [-abcDhHklmsSx][-L <符号连接>][-X <文件>][--block-size][--exclude=<目录或文件>][--max-depth=<目录层数>][--help][--version][目录或文件]

-a或-all 显示目录中个别文件的大小。
-b或-bytes 显示目录或文件大小时，以byte为单位。
-c或--total 除了显示个别目录或文件的大小外，同时也显示所有目录或文件的总和。
-D或--dereference-args 显示指定符号连接的源文件大小。
-h或--human-readable 以K，M，G为单位，提高信息的可读性。
-H或--si 与-h参数相同，但是K，M，G是以1000为换算单位。
-k或--kilobytes 以1024 bytes为单位。
-l或--count-links 重复计算硬件连接的文件。
-L<符号连接>或--dereference<符号连接> 显示选项中所指定符号连接的源文件大小。
-m或--megabytes 以1MB为单位。
-s或--summarize 仅显示总计。
-S或--separate-dirs 显示个别目录的大小时，并不含其子目录的大小。
-x或--one-file-xystem 以一开始处理时的文件系统为准，若遇上其它不同的文件系统目录则略过。
-X<文件>或--exclude-from=<文件> 在<文件>指定目录或文件。
--exclude=<目录或文件> 略过指定的目录或文件。
--max-depth=<目录层数> 超过指定层数的目录后，予以忽略。
--help 显示帮助。
--version 显示版本信息。

例子：du -h test
```

```
df 是从总体上统计系统各磁盘的占用情况，不能统计具体的文件夹或文件的大小。
du 既可以从总体上统计，又可以统计具体的某个文件夹或文件的大小。
```

## 4.6.6 awk

```
与sed一样，均是一行一行的读取，处理
sed作用于一整行的处理，而awk将一行分成数个字段来处理
/*awk ‘BEGIN {commands} pattern {commands}END{commands}' file1
BEGIN:处理数据前执行的命令
END：处理数据后执行的命令
pattern：模式，每一行都执行的命令
BEGIN和END里的命令只是执行一次
pattern里的命令会匹配每一行去处理
*/

[root@localhost 7.4]# awk -F ":" '{print $1,$2,$5}' /etc/passwd | head -5
root x root
bin x bin
daemon x daemon
adm x adm
lp x lp
```



## 4.6.7 sed

```
sed是从文件或者管道中读取一行，放在模式空间中，进行处理，处理完输出一行，在读取一行，再处理一行
格式：
sed -e '操作' 文件1 文件2
sed -n -e '操作' 文件1 文件2
sed -f 脚本文件 文件1 文件2
sed -e -i '操作' 文件1 文件2

-e或--expression：表示用指定命令来处理输入的文本文件，只有一个操作命令时可以省略，一般在执行多个操作命令使用。
-f或--file：表示用指定脚本文件来处理输入的文本文件。
-h或--help：显示帮助。
-n或s --quiet：禁止sed编辑器输出，但可以与p命令一起使用完成输出。
-i：直接修改文本文件。
-r，-E：使用扩展正则表达式
-s：将多个文件视为独立文件，而不是单个连续的长文件流。

常用操作：
s：替换指定字符（替换）。
d：删除指定的行（删除）。
a：在指定的行上一行增加一行指定内容（增加）。
i：在指定的上一行插入一行指定内容（插入）。
c：将选定行内容替换为指定内容（替换）。
y：字符转换，转换之后的字符长度必须相同。
p：打印，如果同时指定行，表示打印指定行，如果不指定行，则表示打印所有内容；如果有非打印字符，则以Ascii码输出。通常与_n选项一起使用。
=：打印行号。
l：答应数据流中的文本和不可打印的ASCII字符。

使用sed命令查看/etc/shdaow
sed ' ' /etc/shadow

查看指定行
sed -n '3p' /etc/shadow

查看/etc/shadow的3到6行的内容
sed -n '3,6p' /etc/shadow
```

## 4.6.8 wc

```
统计指定文件中的字节数、字数、行数，并将统计结果显示输出
常见参数如下：
-c 统计字节数。
-l 统计行数。
-m 统计字符数。这个标志不能与 -c 标志一起使用。
-w 统计字数。注意，这里的字指的是由空格，换行符等分隔的字符串。

wc [-clw][--help][--version][文件...]

$ wc testfile           # testfile文件的统计信息  
3 92 598 testfile       # testfile文件的行数为3、单词数92、字节数598 

```

# 4.7 关机重启

## sync

```
sync 将数据由内存同步到硬盘中

Linux 系统中为了提高磁盘的读写效率，对磁盘采取了 “预读迟写”操作方式。当用户保存文件时，Linux 核心并不一定立即将保存数据写入物理磁盘中，而是将数据保存在缓冲区中，等缓冲区满时再写入磁盘，这种方式可以极大的提高磁盘写入数据的效率。但是，也带来了安全隐患，如果数据还未写入磁盘时，系统掉电或者其他严重问题出现，则将导致数据丢失。使用 sync 指令可以立即将缓冲区的数据写入磁盘

```

## halt

```
halt 停机，关闭系统，但不断电
```

## poweroff

```
poweroff 关机，断电
相当于 shutdoun -h now
```

## reboot

```
reboot 就是重启，等同于shutdown -r now
```

## shutdown

```
shutdown[选项] 时间
```

| 选项 | 功能                                                         |
| ---- | ------------------------------------------------------------ |
| -H   | 相当于--halt，停机                                           |
| -r   | -r=reboot重启                                                |
| -h   | 关机后关闭电源〔halt〕                                       |
| -k   | 并不真正关机﹐只是送警告信号给每位登录者〔login〕            |
| -n   | 不用init﹐而是自己来关机。不鼓励使用这个选项﹐而且该选项所产生的后果往往不总是你所预期得到的 |
| -t   | 在改变到其它runlevel之前﹐告诉init多久以后关机               |
| -c   | 取消按预定时间关闭系统                                       |

| 参数 | 功能                             |
| ---- | -------------------------------- |
| now  | 立刻关机                         |
| 时间 | 等待多久后关机（时间单位是分钟） |

```
shutdown -h 1 'This server will shutdown after 1 mins'
```

## logout

```
logout指令让用户退出系统，其功能和login指令相互对应
```



## runlevel(查看系统运行级别)

```
runlevel 查看系统运行级别
```



## telinit**

```
telinit [参数][运行级别]
常用参数 -q 减少输出，只有错误信息； -v 增加输出，包括信息性消息

telinit命令可以更改Linux系统的运行级别。运行级别可以是0~6之间的一个数字，其中0是关闭系统，1是进入单用户模式，2~5是多用户运行级别，6是重新启动系统。s或S表示单用户模式
0：代表系统停机状态
1：代表单用户工作状态，此状态仅 root 用户可登录。用于系统维护，禁止远程登录，相当于 Windows 下的安全模式。
2：指代多用户状态（无NFS），没有网络服务。
3：指代完整的多用户模式（有NFS），有网络服务，登录后进入控制台命令行模式。
4：指代系统未使用，保留一般不用，在一些特殊情况下可以用它来做一些事情。例如在笔记本电脑的电池用尽时，可以切换到这个模式来做一些设置。
5：指代图形化模式，登陆后进入图形GUI模式或GNOME、KDE图形化界面，如X Window系统
6：指代系统正常关闭并重启，默认运行级别不能设为6，否则不能正常启动，就会一直开机重启开机重启
```

# 4.8 系统信息显示

## arch

```
arch命令主要用于显示当前主机的硬件结构类型，arch命令输出的结果有：i386、i486、mips、alpha等。

此命令的适用范围：RedHat、RHEL、Ubuntu、CentOS、SUSE、openSUSE、Fedora。
```

```
arch [参数]
常用参数 --help 显示此命令的帮助信息 --version 显示零零版本信息
```

## uname

```
用于显示系统相关信息，比如主机名、内核版本号、硬件架构等。

如果未指定任何选项，其效果相当于执行”uname -s”命令，即显示系统内核的名字
```

```
uname [参数]
-a	显示系统所有相关信息
-m	显示计算机硬件架构
-n	显示主机名称
-r	显示内核发行版本号
-s	显示内核名称
-v	显示内核版本
-p	显示主机处理器类型
-o	显示操作系统名称
-i	显示硬件平台
```

## hdparm

```
hdparm命令用于检测，显示与设定IDE或SCSI硬盘的参数
```

```
hdparm [参数]
-a	设定读取文件时，预先存入块区的分区数
-f	将内存缓冲区的数据写入硬盘，并清空缓冲区
-g	显示硬盘的磁轨，磁头，磁区等参数
-I 	直接读取硬盘所提供的硬件规格信息
-X	设定硬盘的传输模式
```

```
hdparm -i /dev/hda 罗列一个磁盘的架构特性 
hdparm -tT /dev/sda 在磁盘上执行测试性读取操作 
```

## dmidecode

```
以一种可读的方式显示机器的DMI(Desktop Management Interface)信息, 其输出的信息包括 BIOS、系统、主板、处理器、内存、缓存等等, 既可以得到当前的配置，也可以得到系统支持的最大配置，比如说支持的最大内存数等。
```

```
dmidecode [参数]
-d	从设备文件读取信息，输出内容与不加参数标准输出相同
-h	显示帮助信息
-s	只显示指定DMI字符串的信息
-t	只显示指定条目的信息
-u	显示未解码的原始条目内容
-- -dump-bin file	将DMI数据转储到一个二进制文件中
-- -from-dump FILE	从一个二进制文件读取DMI数据
-V	显示版本信息
```

```
dmidecode -q 显示硬件系统部件 - (SMBIOS / DMI)
```

## lspci

```
lspci命令用于显示当前主机的所有PCI总线信息，以及所有已连接的PCI设备信息。 现在主流设备如网卡储存等都采用PCI总线
```

```
lspci [参数]
-n	以数字方式显示PCI厂商和设备代码
-t	以树状结构显示PCI设备的层次关系
-b	以总线为中心的视图
-s	仅显示指定总线插槽的设备和功能块信息
-i	指定PCI编号列表文件，不使用默认文件
-m	以机器可读方式显示PCI设备信息
```

```
lspci -tv 罗列 PCI 设备 
```

## lsusb

```
lsusb命令用于显示本机的USB设备列表，以及USB设备的详细信息。

lsusb命令显示的USB设备信息来自“/proc/bus/usb”目录下的对应文件
```

```
lsusb [参数]
-v	显示USB设备的详细信息
-s<总线：设备号>	仅显示指定的总线和（或）设备号的设备
-d<厂商：产品>	仅显示指定厂商和产品编号的设备
-t	以树状结构显示无理USB设备的层次
-V	显示命令的版本信息
```

```
lsusb -tv 显示 USB 设备
```

## lsattr

```

```



## date

```
date显示当前时间
date [选项] [+输出形式]
-d datestr	显示 datestr 中所设定的时间 (非系统时间)
-s datestr	将系统时间设为 datestr 中所设定的时间
-u	显示目前的格林威治时间
--help	显示帮助信息
--version	显示版本编号
```

```
基本语法
1.date
2.date +%Y
3.date +%m
4.date +%d
5.date "+%Y-%m-%d%H:%M:%S"
```

```
//显示当前时间信息
[root@localhost ~]# date
2023年 07月 20日 星期四 14:21:39 CST

//显示当前时间年月日
[root@localhost ~]# date +%Y-%m-%d
2023-07-20

//显示当前时间年月日时分秒
[root@localhost ~]# date +%Y-%m-%d-%H:%m:%S
2023-07-20-14:07:25
```

```
date显示非当前时间
1）基本语法
1.date -d '1 days ago'
2.date -d '-1 days ago'
```

```
设置系统当前时间
[root@hadoop101 ~]# date -s "2017-06-19 20:52:18"
```

## cal

```
命令用来显示当前日历，或者指定日期的公历
cal [参数] [月份] [年份]
常用参数：
-l	单月分输出日历
-3	显示最近三个月的日历
-s	将星期天作为月的第一天
-m	将星期一作为月的第一天
-j	显示在当年中的第几天（儒略日）
-y	显示当年的日历
```

```
// 显示近期三个月的日历
[root@hadoop101 ~]# cal -3

// 显示指定年月的日历，如显示2020年2月的日历
[root@hadoop101 ~]# cal 2 2020
```



# 进程相关命令

## *各种ID的解释*

```
pid: 进程ID。
lwp: 线程ID。在用户态的命令(比如ps)中常用的显示方式。
tid: 线程ID，等于lwp。tid在系统提供的接口函数中更常用，比如syscall(SYS_gettid)和syscall(__NR_gettid)。
tgid: 线程组ID，也就是线程组leader的进程ID，等于pid。
------分割线------
pgid: 进程组ID，也就是进程组leader的进程ID。
pthread id: pthread库提供的ID，生效范围不在系统级别，可以忽略。
sid: session ID for the session leader。
tpgid: tty process group ID for the process group leader。
----------------------------
列序号    列含义    列含义说明
1    UID    用户标识ID
2    PID    进程ID
3    PPID    父进程ID
4    C    CPU占用率
5    STIME    进程开始时间
6    TTY    启动此进程的TTY（终端设备）
7    TIME    此进程运行的总时间
8    CMD    完整的命令名(带启动参数)
```



## ps

```
作用：查询系统进程
语法：ps -[]
-A ：所有的进程均显示出来
-a ：不与terminal有关的所有进程
-u ：有效用户的相关进程
-x ：一般与a参数一起使用，可列出较完整的信息
-l ：较长，较详细地将PID的信息列出

ps -ef | grep []
-e 显示所有进程
-f 全格式
```

**说明:** 

- ==ps==命令是linux下非常强大的进程查看命令，通过ps -ef可以查看当前运行的所有进程的详细信息

- =="|"== 在Linux中称为管道符，可以将前一个命令的结果输出给后一个命令作为输入

- 使用ps命令查看进程时，经常配合管道符和查找命令 grep 一起使用，来查看特定进程

## | 管道符

=="|"== 在Linux中称为管道符，可以将前一个命令的结果输出给后一个命令作为输入

## kill

```
作用：kill 命令用于终止进程
语法：kill -signal PID（端口号）

1：SIGHUP，启动被终止的进程
2：SIGINT，相当于输入ctrl+c，中断一个程序的进行
9：SIGKILL，强制中断一个进程的进行
15：SIGTERM，以正常的结束进程方式来终止进程
17：SIGSTOP，相当于输入ctrl+z，暂停一个进程的进行

例如 kill -9 6379
```

##  killall

```
作用：Linux killall 用于杀死一个进程，与 kill 不同的是它会杀死指定名字的所有进程
语法：killall [-iIe] [command name]
参数说明：

name ： 进程名
选项包含如下几个参数：
-e | --exact ： 进程需要和名字完全相符
-I | --ignore-case ：忽略大小写
-g | --process-group ：结束进程组
-i | --interactive ：结束之前询问
-l | --list ：列出所有的信号名称
-q | --quite ：进程没有结束时，不输出任何信息
-r | --regexp ：将进程名模式解释为扩展的正则表达式。
-s | --signal ：发送指定信号
-u | --user ：结束指定用户的进程
-v | --verbose ：显示详细执行过程
-w | --wait ：等待所有的进程都结束
-V |--version ：显示版本信息
--help ：显示帮助信息

例子# killall -9 php-fpm          //结束所有的 php-fpm 进程
```

## crontab

```
作用：crontab命令是启动linux定时任务的服务
语法：crontab [ -u user ] file 或 crontab [ -u user ] { -l | -r | -e }
说明：

crontab 是用来让使用者在固定时间或固定间隔执行程序之用，换句话说，也就是类似使用者的时程表。

-u user 是指设定指定 user 的时程表，这个前提是你必须要有其权限(比如说是 root)才能够指定他人的时程表。如果不使用 -u user 的话，就是表示设定自己的时程表。

参数说明：

-e : 执行文字编辑器来设定时程表，内定的文字编辑器是 VI，如果你想用别的文字编辑器，则请先设定 VISUAL 环境变数来指定使用那个文字编辑器(比如说 setenv VISUAL joe)
-r : 删除目前的时程表
-l : 列出目前的时程表

时间格式如下：
f1 f2 f3 f4 f5 program

其中 f1 是表示分钟，f2 表示小时，f3 表示一个月份中的第几日，f4 表示月份，f5 表示一个星期中的第几天。program 表示要执行的程序。
当 f1 为 * 时表示每分钟都要执行 program，f2 为 * 时表示每小时都要执行程序，其馀类推
当 f1 为 a-b 时表示从第 a 分钟到第 b 分钟这段时间内要执行，f2 为 a-b 时表示从第 a 到第 b 小时都要执行，其馀类推
当 f1 为 */n 时表示每 n 分钟个时间间隔执行一次，f2 为 */n 表示每 n 小时个时间间隔执行一次，其馀类推
当 f1 为 a, b, c,... 时表示第 a, b, c,... 分钟要执行，f2 为 a, b, c,... 时表示第 a, b, c...个小时要执行，其馀类推


service cron start # 启动cronjob
service cron stop # 停止cronjob
service cron restart # 重启cronjob
crontab -e # 编辑cronjob任务

```

## free

```
作用：free 命令用于显示Linux系统中空闲的、已用的物理内存及swap内存,及被内核使用的buffer
语法：free [参数]
-b 　以Byte为单位显示内存使用情况。
-k 　以KB为单位显示内存使用情况。
-m 　以MB为单位显示内存使用情况。
-g 以GB为单位显示内存使用情况。
-o 　不显示缓冲区调节列。
-s<间隔秒数> 　持续观察内存使用状况。
-t 　显示内存总和列。
-V 　显示版本信息。

例如：free -m; free -g
```

## top

```
作用：命令是Linux下常用的性能分析工具，能够实时显示系统中各个进程的资源占用状况，类似于Windows的任务管理器
【查看进程对应的cpu和内存占用率】
语法：top【参数】

-b 批处理
-c 显示完整的治命令
-I 忽略失效过程
-s 保密模式
-S 累积模式
-i<时间> 设置间隔时间
-u<用户名> 指定用户名
-p<进程号> 指定进程
-n<次数> 循环显示的次数

例如：top -bc -n 1
```

## jobs

```
作用：显示所有当前作业及其状态。作业基本上是由 Shell 启动的进程。
```

## fdish

```
# 语法：fdisk [-l] 
# 选项：-l : 系统将会把整个系统内能够搜寻到的装置的分区均列出来
fdisk -l
```

## vmstat



## uptime



# 权限相关命令

## Linux权限

前面我们已经把Shell脚本准备好了，但是Shell脚本要想正常的执行，还需要给Shell脚本分配执行权限。 由于linux系统是一个多用户的操作系统，并且针对每一个用户，Linux会严格的控制操作权限。接下来，我们就需要介绍一下Linux系统的权限控制。

> 1). ==chmod==（英文全拼：change mode）命令是控制用户对文件的权限的命令
>
> 2). Linux中的权限分为三种 ：读(r)、写(w)、执行(x)
>
> 3). Linux文件权限分为三级 : 文件所有者（Owner）、用户组（Group）、其它用户（Other Users）
>
> 4). 只有文件的所有者和超级用户可以修改文件或目录的权限
>
> 5). 要执行Shell脚本需要有对此脚本文件的执行权限(x)，如果没有则不能执行



Linux系统中权限描述如下: 

![image-20210815162945754](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171643740.png) 



解析当前脚本的权限情况: 

<img src="https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171643741.png" alt="image-20210815162135509" style="zoom:80%;" /> 

```
文件类型
d 代表是文件夹
- 开头的代表是文件（包括硬链接文件，硬链接文件相当于原文件的备份，可以与原文件做到同步更新）
l 开头的代表是软链接文件，软链接文件相当于原文件的快捷方式
c 开头的代表是字符设备文件，例：鼠标、键盘
b 开头的代表是块设备文件，例：硬盘
```

## sudo

```
作用：sudo 用来以其他身份来执行命令，预设的身份为root
语法：sudo(选项)(参数)
参数：
-b：在后台执行指令；
-h：显示帮助；
-H：将HOME环境变量设为新身份的HOME环境变量；
-k：结束密码的有效期限，也就是下次再执行sudo时便需要输入密码；。
-l：列出目前用户可执行与无法执行的指令；
-p：改变询问密码的提示符号；
-s：执行指定的shell；
-u<用户>：以指定的用户作为新的身份。若不加上此参数，则预设以root作为新的身份；
-v：延长密码有效期限5分钟；
-V ：显示版本信息。

例如 sudo groupdel testgrp
```

## su

```
su命令用于切换当前用户身份到指定用户或者以指定用户的身份执行命令或程序
su 用户名称 （功能描述：切换用户，只能获得用户的执行权限，不能获得环境变量）su - 用户名称 （功能描述：切换到用户并获得该用户的环境变量及执行权限）
# 语法：su [-] 用户名
su username
su - username # 到用户的home目录下
[root@linux265 ~]# su linux265
```

## chomd

作用：chmod命令是控制用户对文件的权限的命令

chmod命令可以使用八进制数来指定权限(0 - 代表无 , 1 - 执行x , 2 - 写w , 4 - 读r):

| 值   | 权限           | rwx  |
| ---- | -------------- | ---- |
| 7    | 读 + 写 + 执行 | rwx  |
| 6    | 读 + 写        | rw-  |
| 5    | 读 + 执行      | r-x  |
| 4    | 只读           | r--  |
| 3    | 写 + 执行      | -wx  |
| 2    | 只写           | -w-  |
| 1    | 只执行         | --x  |
| 0    | 无             | ---  |



**举例:**

```
chmod 777 bootStart.sh   为所有用户授予读、写、执行权限
chmod 755 bootStart.sh   为文件拥有者授予读、写、执行权限，同组用户和其他用户授予读、执行权限
chmod 210 bootStart.sh   为文件拥有者授予写权限，同组用户授予执行权限，其他用户没有任何权限

sudo chmod [u所属用户  g所属组  o其他用户  a所有用户]  [+增加权限  -减少权限]  [r  w  x]  目录名

例如：有一个文件 filename，权限为-rw-r----x ,将权限值改为-rwxrw-r-x，用数值表示为765

sudo chmod u+x g+w o+r filename 也可以用数值表示 sudo chmod 765 filename
```



==注意:==

三个数字分别代表不同用户的权限

- 第1位表示文件拥有者的权限
- 第2位表示同组用户的权限
- 第3位表示其他用户的权限

## chown

```
作用：chown命令用于设置文件所有者和文件关联组的命令
语法：chown [-cfhvR] [--help] [--version] user[:group] file...
参数 :
user : 新的文件拥有者的使用者 ID
group : 新的文件拥有者的使用者组(group)
-c : 显示更改的部分的信息
-f : 忽略错误信息
-h :修复符号链接
-v : 显示详细的处理信息
-R : 处理指定目录以及其子目录下的所有文件
--help : 显示辅助说明
--version : 显示版本

# 将目录/root/nginx-1.22.0及其下面的所有文件、子目录的文件主改成 root：
#原始文件所属主和所属组
drwxr-xr-x. 9 hwljw hwljw     186 9月  26 2022 nginx-1.22.0
chown -R root ./nginx-1.22.0  #修改所属主
ls -ltr
drwxr-xr-x. 9 hwljw hwljw     186 9月  26 2022 nginx-1.22.0
chown -R root:root ./nginx-1.22.0  #修改所属主和所属组
ls -ltr
drwxr-xr-x. 9 root  root      186 9月  26 2022 nginx-1.22.0
```

## chgrp

```
作用：改变文件所属组
chgrp [-cfhRv][--help][--version][所属群组][文件或目录...] 或 chgrp [-cfhRv][--help][--reference=<参考文件或目录>][--version][文件或目录...]
参数：
-c 当发生改变时输出调试信息
-f 不显示错误信息
-R 处理指定目录以及其子目录下的所有文件
-v 运行时显示详细的处理信息
--dereference 作用于符号链接的指向，而不是符号链接本身
--no-dereference 作用于符号链接本身

例如：chgrp --reference=log2012.log log2013.log

# 将目录/home/chgrp.txt文件所属组改成root：
drwxrwxr-x. 2 ljw   ljw     6 9月  12 23:19 chgrp.txt
chgrp root /home/chgrp.txt/
ls -ltr
drwxrwxr-x. 2 ljw   root    6 9月  12 23:19 chgrp.txt 
```

## chattr

```
chattr命令用于改变文件属性。

这项指令可改变存放在ext2文件系统上的文件或目录属性，这些属性共有以下8种模式：

a：让文件或目录仅供附加用途。
A：当一个具有“A”属性的文件被访问时，它的atime记录不会被修改
b：不更新文件或目录的最后存取时间。
c：将文件或目录压缩后存放。
d：将文件或目录排除在倾倒操作之外。
i：不得任意更动文件或目录。
s：保密性删除文件或目录。
S：即时更新文件或目录。
u：预防意外删除。
i：不得任意更动文件或目录；
j：如果文件系统安装有“data=order”或“data=writeback”选项，则具有“j”属性的文件在写入文件本身之前将其所有数据写入ext 3日志；
```

```
chattr +a file1 只允许以追加方式读写文件 
chattr +c file1 允许这个文件能被内核自动压缩/解压 
chattr +d file1 在进行文件系统备份时，dump程序将忽略这个文件 
chattr +i file1 设置成不可变的文件，不能被删除、修改、重命名或者链接 
chattr +s file1 允许一个文件被安全地删除 
chattr +S file1 一旦应用程序对这个文件执行了写操作，使系统立刻把修改的结果写到磁盘 
chattr +u file1 若文件被删除，系统会允许你在以后恢复这个被删除的文件 
```

## useradd

```
作用：建立用户账号
语法：useradd [-mMnr][-c <备注>][-d <登入目录>][-e <有效期限>][-f <缓冲天数>][-g <群组>][-G <群组>][-s <shell>][-u <uid>][用户帐号]

参数说明：

-c<备注> 　加上备注文字。备注文字会保存在passwd的备注栏位中。
-d<登入目录> 　指定用户登入时的起始目录。
-D 　变更预设值．
-e<有效期限> 　指定帐号的有效期限。
-f<缓冲天数> 　指定在密码过期后多少天即关闭该帐号。
-g<群组> 　指定用户所属的群组。
-G<群组> 　指定用户所属的附加群组。
-m 　制定用户的登入目录。
-M 　不要自动建立用户的登入目录。
-n 　取消建立以用户名称为名的群组．
-r 　建立系统帐号。
-s<shell>　 　指定用户登入后所使用的shell。
-u<uid> 　指定用户ID。
```

![image-20230211120110091](https://cdn.jsdelivr.net/gh/God-ljw/picbed@main/img3/202302111201711.png)

## usermod

```
作用：修改用户信息
语法：usermod [options] LOGIN
-c #后面接账号的说明，即/etc/passwd第五栏的说明栏，可以加入一些账号的说明

-d #后面接账号的家目录，即修改/etc/passwd的第六栏
-e #后面接日期，格式是YYYY-MM-DD也就是在/etc/shadow内的第八栏
-f #后面接天数，修改shadow的第七栏
-g #后面接主群组，修改/etc/passwd的第四个字段，即是GID的字段
-G #后面接附加群组，修改这个使用者能够支持的群组，修改的是/etc/group
-a #与 -G 合用，可增加附加群组的支持而非设定
-l #后面接账号名称。修改账号名称，/etc/passwd的第一栏
-s #后面接Shell的文件，例如/bin/bash或/bin/csh等等
-u #后面接 UID 数字，修改用户的UID /etc/passwd第三栏
-L #暂时将用户的密码冻结，让他无法登入。其实就是在/etc/shadow的密码栏前面加上了“!”
-U #将/etc/shadow 密码栏的“!”去掉

例如：sudo usermod -c aaa -e 2020-12-12 hadopp2
```

## userdel

```
作用：删除用户
语法：userdel [options] LOGIN
-f # 强制删除，包括用户的一切相关内容，这个参数是危险的参数，不建议大家使用。详细说明看MAN

-r # 删除用户的家目录和用户的邮件池
例如：sudo userdel -f -r hadoop2
```

## groupadd

```
作用：将新组加入系统
语法：groupadd [－g gid] [－o]] [－r] [－f] groupname
－g gid：指定组ID号。
－o：允许组ID号，不必惟一。
－r：加入组ID号，低于499系统账号。
－f：加入已经有的组时，发展程序退出

例如：sudo groupadd -g 333 testgrp
```

## groupdel

```
作用：删除不需要的组，如果指定的包含用户，则必须线删除组里面的用户>以后，才能删除组
groupdel [options] GROUP

例如：sudo groupdel testgrp
```

## passwd

```
作用：设置用户密码
语法：passwd [-k] [-l] [-u [-f]] [-d] [-S] [username]

必要参数：

-d 删除密码
-f 强迫用户下次登录时必须修改口令
-w 口令要到期提前警告的天数
-k 更新只能发送在过期之后
-l 停止账号使用
-S 显示密码信息
-u 启用已被停止的账户
-x 指定口令最长存活期
-g 修改群组密码
指定口令最短存活期
-i 口令过期后多少天停用账户


```



![image-20230211121005336](https://cdn.jsdelivr.net/gh/God-ljw/picbed@main/img3/202302111210393.png)

## groups

```
作用：显示用户所属组
例如：groups hadoop
```

## whoami

```
# 语法：whoami -全称是"Who am I?”，意为“我是谁？”
whoami # 查看当前登录用户
```



# 网络相关命令

## Vmware的三种网络连接方式

- 桥接模式：
  - 虚拟机直接连接外部物理网络的模式，主机起到了网桥的作用。这种模式下，虚拟机可以直接访问外部网络，并且对外部网络是可见的。
- NAT模式：VMnet8
  - 虚拟机和主机构建一个专用网络，并通过虚拟机网络地址转换（NAT）设备对IP进行转换。虚拟机通过共享主机IP可以访问外部网络，但外部网络无法访问虚拟机。
  - 虚拟机的子网是192.168.100.0，NAT的网关是192.168.100.2，主机的网关是192.168.100.1。
- 仅主机模式：VMnet1
  - 虚拟机只与主机共享一个专用网络，与外部网络无法通信。



## 1.使用nmcli修改网络

- 首先列出当前可用的网络连接。

  - ```
    nmcli connection show
    # 在输出结果中找到您想要修改的网络连接的名字，比如`ens33`等
    ```

- 使用`nmcli`命令来修改该网络连接的参数。DHCP IP(自动）

  - ```
    nmcli connection modify "ens33" ipv4.method auto
    ```

- 将网络连接设置为静态IP

  - ```
    nmcli connection modify "ens33" ipv4.method manual ipv4.addresses 192.168.100.100/24 ipv4.gateway 192.168.100.2 ipv4.dns 192.168.100.2
    # `ipv4.addresses`是您要设置的静态IP地址，`ipv4.gateway`是您的网关IP地址，`ipv4.dns`是您的DNS服务器IP地址。
    
    ```

- 重新启用该网络连接

  - ```
    nmcli connection up "ens33"
    ```

## 网络设置（nmcli）

- "nmcli"是一个在命令行下用于管理和配置NetworkManager的工具。
- 以下是一些常用的nmcli命令：
  - 显示NetworkManager的状态：nmcli general status
  - 连接WiFi网络：nmcli device wifi connect SSID-Name password WIFI_Password
  - 显示当前连接的网络：nmcli connection show --active
  - 添加VPN连接：nmcli connection import type openvpn file myvpn-config.ovpn
  - 显示所有可用连接：nmcli connection show
  - 断开当前连接：nmcli connection down Connection-Name
  - 启用和禁用网络适配器：nmcli radio wifi on/off
  - 显示当前连接的详细信息：nmcli connection show Connection-Name

## ping

```
作用：确定主机与外部链接状态
语法：ping [参数] [主机名或IP地址]

-d 使用Socket的SO_DEBUG功能。
-f 极限检测。大量且快速地送网络封包给一台机器，看它的回应。
-n 只输出数值。
-q 不显示任何传送封包的信息，只显示最后的结果。
-r 忽略普通的Routing Table，直接将数据包送到远端主机上。通常是查看本机的网络接口是否有问题。
-R 记录路由过程。
-v 详细显示指令的执行过程。
<p>-c 数目：在发送指定数目的包后停止。
-i 秒数：设定间隔几秒送一个网络封包给一台机器，预设值是一秒送一次。
-I 网络界面：使用指定的网络界面送出数据包。
-l 前置载入：设置在送出要求信息之前，先行发出的数据包。
-p 范本样式：设置填满数据包的范本样式。
-s 字节数：指定发送的数据字节数，预设值是56，加上8字节的ICMP头，一共是64ICMP数据字节。
-t 存活数值：设置存活数值TTL的大小。

例子：Ping -v -i 1 www.baidu.com
```

## ifconfig

```
作用：用来查看和配置网络设备。当网络环境发生改变时可以通过此命令对网络进行相应配置
语法：ifconfig[网络设备][参数]
up 启动指定网络设备/网卡。

down 关闭指定网络设备/网卡。该参数可以有效地阻止通过指定接口的IP信息流，如果想永久地关闭一个接口，我们还需要从核心路由表中将该接口的路由信息全部删除。

arp 设置指定网卡是否支持ARP协议。
-promisc 设置是否支持网卡的promiscuous模式，如果选择此参数，网卡将接收网络中发给它所有的数据包
-allmulti 设置是否支持多播模式，如果选择此参数，网卡将接收网络中所有的多播数据包
-a 显示全部接口信息
-s 显示摘要信息（类似于 netstat -i）
add 给指定网卡配置IPv6地址
del 删除指定网卡的IPv6地址
<硬件地址> 配置网卡最大的传输单元
mtu<字节数> 设置网卡的最大传输单元 (bytes)
netmask<子网掩码> 设置网卡的子网掩码。掩码可以是有前缀0x的32位十六进制数，也可以是用点分开的4个十进制数。如果不打算将网络分成子网，可以不管这一选项；如果要使用子网，那么请记住，网络中每一个系统必须有相同子网掩码。

tunel 建立隧道
dstaddr 设定一个远端地址，建立点对点通信
-broadcast<地址> 为指定网卡设置广播协议
-pointtopoint<地址> 为网卡设置点对点通讯协议
multicast 为网卡设置组播标志
address 为网卡设置IPv4地址
txqueuelen<长度> 为网卡设置传输列队的长度

例子：ifconfig -a
```



## ssh

```
作用：用于远程登陆上Linux主机
语法：ssh[-l login_name][-p port][user@]hostname
例子：ssh nwu@download3.internal.ng.movoto.net
```

## scp

```
作用：scp命令是secure copy的简写，用于在Linux下进行远程拷贝文件的命令，和它类似的命令有cp，不过cp只是在本机进行拷贝不能跨服务器，而且scp传输是加密的

语法：scp [参数] [原路径] [目标路径]
-1 强制scp命令使用协议ssh1
-2 强制scp命令使用协议ssh2
-4 强制scp命令只使用IPv4寻址
-6 强制scp命令只使用IPv6寻址
-B 使用批处理模式（传输过程中不询问传输口令或短语）
-C 允许压缩。（将-C标志传递给ssh，从而打开压缩功能）
-p 保留原文件的修改时间，访问时间和访问权限。
-q 不显示传输进度条。
-r 递归复制整个目录。
-v 详细方式显示输出。scp和ssh(1)会显示出整个过程的调试信息。这些信息用于调试连接，验证和配置问题。
-c cipher 以cipher将数据传输进行加密，这个选项将直接传递给ssh。
-F ssh_config 指定一个替代的ssh配置文件，此参数直接传递给ssh。
-i identity_file 从指定文件中读取传输时使用的密钥文件，此参数直接传递给ssh。
-l limit 限定用户所能使用的带宽，以Kbit/s为单位。
-o ssh_option 如果习惯于使用ssh_config(5)中的参数传递方式，
-P port 注意是大写的P, port是指定数据传输用到的端口号
-S program 指定加密传输时所使用的程序。此程序必须能够理解ssh(1)的选项。
```

## telnet

```
telnet 命令用来远程登录操作
telnet[参数][主机]
例子：telnet www.baidu.com
```

## wget

```
作用：从远程下载工具的工具
例子：wget http://cn.wordpress.org/wordpress-4.9.4-zh_CN.tar.gz
```

![image-20230211185758421](https://cdn.jsdelivr.net/gh/God-ljw/picbed@main/img3/202302111858553.png)

## netstat

```
作用：netstat命令用来打印Linux中网络系统的状态信息，可让你得知整个Linux系统的网络情况。
参数说明: 

​		-l或--listening：显示监控中的服务器的Socket；
​		-n或--numeric：直接使用ip地址，而不通过域名服务器；
​		-p或--programs：显示正在使用Socket的程序识别码和程序名称；
​		-t或--tcp：显示TCP传输协议的连线状况；
​		-u或--udp：显示UDP传输协议的连线状况；

netstat -tunlp					查看已经启动的服务
netstat -tunlp | grep mysql		查看mysql的服务信息

ps –ef | grep mysql				查看mysql进程
```

## ftp



# 程序运行命令

## nohup

```
作用：英文全称 no hang up（不挂起），用于不挂断地运行指定命令，退出终端不会影响程序的运行
语法格式： nohup Command [ Arg … ] [&]

nohup java -jar boot工程.jar &> hello.log 

上述指令的含义为： 后台运行 java -jar 命令，并将日志输出到hello.log文件
```

## systemctl（开机自动启动）

```
作用：systemctl指令来查看程序的状态、启动程序、停止程序。
例如：
systemctl status mysqld		查看mysql服务状态
systemctl start mysqld		启动mysql服务
systemctl stop mysqld		停止mysql服务
systemctl enable mysqld     设置开机自动启动mysql
```

## mount(文件挂载)

```
	Linux系统中一切皆文件，所有文件都放置在以根目录为树根的树形目录结构中。在Linux看来，任何硬件设备也都是文件，它们各有自己的一套文件系统，也就是文件目录结构。
　　说到这里就产生了一个问题，当在Linux系统中使用这些硬件设备时，只有将Linux本身的文件目录与硬件设备的文件目录合二为一，硬件设备才能为我们所用。
　　合二为一的过程称为挂载。
```

```
mount命令用于加载文件系统到指定的加载点
mount [参数]
-t	指定挂载类型
-l	显示已加载的文件系统列表
-h	显示帮助信息并退出
-V	显示程序版本
-n	加载没有写入文件“/etc/mtab”中的文件系统
-r	将文件系统加载为只读模式
-a	加载文件“/etc/fstab”中描述的所有文件系统
```

```
mount /dev/fd0 /mnt/floppy 挂载一个软盘 
mount /dev/cdrom /mnt/cdrom 挂载一个cdrom或dvdrom 
mount /dev/hdc /mnt/cdrecorder 挂载一个cdrw或dvdrom 
mount /dev/hdb /mnt/cdrecorder 挂载一个cdrw或dvdrom 
mount -o loop file.iso /mnt/cdrom 挂载一个文件或ISO镜像文件 
mount -t vfat /dev/hda5 /mnt/hda5 挂载一个Windows FAT32文件系统 
mount /dev/sda1 /mnt/usbdisk 挂载一个usb 捷盘或闪存设备 
mount -t smbfs -o username=user,password=pass //WinClient/share /mnt/share 挂载一个windows网络共享 
```

## umount

```
是卸载已安装的文件系统、目录或文件。利用设备名或挂载点都能umount文件系统，不过最好还是通过挂载点卸载，一面使用绑定挂在（一个设备，多个挂载点）时产生混乱
```

```
umount [参数]
-a	卸载/etc/mtab中记录的所有文件系统
-h	显示帮助
-n	卸载时不要将信息存入/etc/mtab文件中
-r	若无法成功卸载，则尝试以只读的方式重新挂入文件系统
-t	文件系统类型：仅卸载选项中所指定的文件系统
-v	执行时显示详细的信息
-V	显示版本信息
```



# 软件安装

| 安装方式         | 特点                                                         |
| ---------------- | ------------------------------------------------------------ |
| 二进制发布包安装 | 软件已经针对具体平台编译打包发布，只要解压，修改配置即可     |
| rpm安装          | 软件已经按照redhat的包管理规范进行打包，使用rpm命令进行安装，==不能自行解决库依赖问题== |
| yum安装          | 一种在线软件安装方式，本质上还是rpm安装，自动下载安装包并安装，安装过程中自动解决库依赖问题(安装过程需要联网) |
| 源码编译安装     | 软件以源码工程的形式发布，需要自己编译打包                   |



## rpm

安装 `rpm -i jdk-XXX_linux-x64_bin.rpm`

查找 `rpm -qa | grep jdk`

列表 `rpm -qa | more`

> ubuntu dpkg 方式
>
> 查找dpkg -I | grep jdk
>
> 列表dpkg -I | more
>
> 安装dpkg -i jdk-XXX_linux-x64_bin.deb

### yum方式

搜索 `yum search jdk`

安装 `yum install java-11-openjdk.x86_64`

删除 `yum erase java-11 -openjdk.x86 64`

配置文件 `/etc/yum.repos.d/CentOS-Base.repo`

> ubuntu apt-get 方式
>
> 搜索 apt・cache search jdk
>
> 安装apt-get install openjdk-9-jdk
>
> 删除apt-get purge openjdk-9-jdk
>
> 配置文件/etc/apt/sources. Iist

| 命令 | 选项              | 操作         | 描述                          | 包名     |
| ---- | ----------------- | ------------ | ----------------------------- | -------- |
|      |                   | search       | 搜索软件包信息                |          |
|      |                   | install      | 安装软件包                    |          |
|      | -h（帮助）        | remove       | 删除软件包                    |          |
| yum  | -y（安装全程yes） | list         | 显示软件包信息                | filename |
|      | -q（不显示安装）  | update       | 更新软件包                    |          |
|      |                   | check-update | 检查是否有可用的更新软件包    |          |
|      |                   | clean        | 清理 yum 过期的缓存           |          |
|      |                   | deplist      | 显示 yum 软件包的所有依赖关系 |          |



`yum install epel-release -y` 是一个用于在基于 Red Hat 的 Linux 发行版（如 CentOS 或 Fedora）上安装 EPEL (Extra Packages for Enterprise Linux) 仓库的命令。

# 防火墙命令

| 操作                         | 指令                                                         | 备注                 |
| ---------------------------- | ------------------------------------------------------------ | -------------------- |
| 查看防火墙状态               | systemctl status firewalld / firewall-cmd --state            |                      |
| 暂时关闭防火墙               | systemctl stop firewalld                                     |                      |
| 永久关闭防火墙(禁用开机自启) | systemctl disable firewalld                                  | ==下次启动,才生效==  |
| 暂时开启防火墙               | systemctl start firewalld                                    |                      |
| 永久开启防火墙(启用开机自启) | systemctl enable firewalld                                   | ==下次启动,才生效==  |
| 开放指定端口                 | firewall-cmd --zone=public --add-port=8080/tcp --permanent   | ==需要重新加载生效== |
| 关闭指定端口                 | firewall-cmd --zone=public --remove-port=8080/tcp --permanent | ==需要重新加载生效== |
| 立即生效(重新加载)           | firewall-cmd --reload                                        |                      |
| 查看开放端口                 | firewall-cmd --zone=public --list-ports                      |                      |

#  设置静态IP 

我们目前安装的Linux操作系统，安装完毕之后并没有配置IP地址，默认IP地址是动态获取的，那如果我们使用该Linux服务器部署项目，IP动态获取的话，也就意味着，IP地址可能会发生变动，那我们访问项目的话就会非常繁琐，所以作为服务器，我们一般还需要把IP地址设置为静态的。 

1). 设置静态IP

设置静态ip，我们就需要修改 /etc/sysconfig/network-scripts/ifcfg-ens33 配置文件，内容如下： 

```properties
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static
IPADDR="192.168.138.100"        # 设置的静态IP地址
NETMASK="255.255.255.0"         # 子网掩码
GATEWAY="192.168.138.2"         # 网关地址
DNS1="192.168.138.2"            # DNS服务器
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=afd0baa3-8bf4-4e26-8d20-5bc426b75fd6
DEVICE=ens33
ONBOOT=yes
ZONE=public
```

![image-20210815171934667](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171643745.png) 

上述我们所设置的网段为138，并不是随意指定的，需要和我们虚拟机中的虚拟网络编辑器中的NAT模式配置的网关保持一致。

![image-20210815172303896](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171643746.png) 



2). 重启网络服务

ip地址修改完毕之后，需要重启网络服务，执行如下指令： 

```
systemctl restart network
```

![image-20210815172448448](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171643748.png) 

==注意：重启完网络服务后ip地址已经发生了改变，此时FinalShell已经连接不上Linux系统，需要创建一个新连接才能连接到Linux。==



再次连接上Linux之后，我们再次查看IP地址，就可以看到我们所设置的静态IP：

![image-20210815172832108](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171643749.png) 


















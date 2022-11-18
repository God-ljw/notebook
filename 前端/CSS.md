# 二、CSS基本语法
### 一、css简介
1、cascading style sheets 层叠样式表，主要作用是呈现样式
​		*层叠性
​			给同一个元素添加相同的css属性，属性值之间会发生层叠问题。
​		*样式表

### 二、css语法
1、格式：选择器{声明}
​		*声明由  css属性：属性值；组成
​2、css属性：
​		*width 宽度
​		*height 高度
​		*background-color 背景颜色
​			*red 红色  blue 蓝色  green绿色  orange橙色
​3、css注释：

```css
/* css注释 */
```
### 三、样式表
1、内部样式表 head>style，在style标签里面书写css语法格式
	*作用域：当前页面
​2、外部样式表
​	（1）建立外部样式表：css文件夹-新建css文件，在该css文件里写css语法格式
​	（2）在页面中链接该css文件，通过head>link[rel="stylesheet" href="css文件路径"]
​	*作用域：所有链接到该css文件的页面
3、内联（行内）样式表
​	[style="声明"]
​	声明由css属性：属性值；组成
​	*作用域：当前元素
​	*优先级：就近原则（内联样式的优先级最高，内部样式与外部样式的优先级是一样大的，谁离该元素近谁就起作用）

### 四、选择器
1、标签选择器（元素选择器、类型选择器）：将标签名字作为选择器

```css
div{width:100px;}
```
2、类选择器（class选择器）：将.类名作为选择器

```css
.box{width:100px;}
```
3、id选择器：将#id名作为选择器

```css
#container{width:100px;}
```
4、通配符选择器

```css
*{color:black;}
```
5、群组（并集）选择器：将选择器用逗号隔开，表示这些选择器同时被获取到
```css
.box,#container{width:100px;}
```
6、后代选择器：将选择器用空格隔开

```html
#header .box1{width:100px;}
<div id="header">
	<div class="box1"></div>
</div>
```
7、伪类选择器
​	（1）:link 锚链接未被访问前的样式
​	（2）:visited 锚链接被访问后的样式
​	（3）:hover 鼠标悬停在元素上，才触发
​	（4）:active 鼠标点击元素时，触发样式
书写顺序：lv-ha
8、交集选择器

```css
div.box{width:100px;}
```

### 五、选择器的优先级及权重
​	（1）基本选择器的优先级
​		*前提：给同一个元素添加相同的css属性，才有优先级的比较
​	!important**/行内样式>id选择器>类选择器/伪类选择器>标签选择器>通配符选择器**
​	（2）选择器的权重比较
​		0000原则：
​			*第一个0代表!important或者内联样式
​			*第二个0代表id选择器的个数
​			*第三个0表示类选择器的个数
​			*第四个0表示标签选择器的个数
​		继承的权重最低，为0000

### nth-child(n)如何使用

nth-child(-n+4)
选中从第1个到第4个子元素。使用 :nth-child(-n+4) ，就相当让你选中第9个和其之前的所有子元素.
例如：

```css
ul>li:nth-child(n+4) span{
	background:deeppink;
}

```

### 7光标小样式

 给光标加小手样式 

 cursor:光标 pointer:指示器 

  cursor: pointer;



可移动光标 

 cursor:move;



 禁止样式 

 cursor: not-allowed;



 问号光标样式 

 cursor: help; 



 光标指示可向左移动 

 cursor: e-resize;

# 三、CSS核心属性

### 一、字体属性 font

​	1、**字体大小font-size**
​		*默认的字体大小为16px，最小为12px
​		*9pt=12px，12pt=16px
​	2、**字体加粗font-weight**
​		*属性值：normal默认情况下不加粗  bold加粗
​		*（100-500表示normal，600-900表示bold）
​	3、**字体倾斜font-style**
​		*属性值：normal默认情况下不倾斜  italic倾斜 oblique更加倾斜
​	4、**字体家族font-family**
​		*属性值为汉字或者多个单词，属性值要加双引号
​		*同一个CSS分属性的多个属性值用逗号隔开
​	5、**字体颜色 color**
​		*属性值：英文单词、十六进制（光学模式）[#000000]
​		*十六进制表示法：#000000
​			*每位数字的取值可以是0-9或者a-f
​			*前面两位数字表示红色#ff0000
​			*中间两位数字表示绿色#00ff00
​			*后面两位数字表示蓝色#0000ff
​			*若表示同一个颜色的两位数都相等，可以省略为三位数#000黑色#fff白色

### 二、文本属性

​	1、**text-transform检索文本的大小写**
​		*属性值：
​			none 默认不改变
​			uppercase全部转成大写
​			lowercase全部转成小写
​			capitalize首字母大写
​	2、text-decoration文本修饰
​		*属性值：
​			none 默认没有文本修饰
​			underline 下划线
​			overline 上划线
​			line-through删除线
​			blink闪烁:IE、Chrome 或 Safari 不支持 "blink" 属性值。
​	3、text-indent 首行缩进
​		*单位： em以一个字体的大小为基准 
​		*可以为负数

```
text-indent:2em;
```

​	4、letter-spacing字间距
​		*以字或字母作为分界点
​	5、word-spacing词间距
​		*以空格作为分界点
​	6、text-align 文本在当前容器的水平方向的对齐方式
​		*属性值：left默认向左对齐 	 center居中对齐  right向右对齐   justify两端对
​		*文本：文字、图片
​		*容器：块级元素
​	7、vertical-align行内元素在垂直方向上的对齐方式
​		*属性值：baseline 默认以基线对齐  top顶线对齐  bottom底线对齐  middle中线对齐
​		*常用于将文字与图片垂直方向居中对齐 

```
vertical-align:center;
```

​	8、line-height行高
​		*行高：一行的高度
​		*行高=文本的上间距+文本的下间距+字体大小
​		*1.在行高中，文字一定是居中显示的
​		*2.在同一段文本中，行高中的文本上间距=文本下间距
​		*3.常用操作：（1）若想一行文本在容器中垂直居中，可以将line-height设成容器的高度（2）若单行文本在居中偏上，则line-height<容器高度

### 三、列表属性

​	1、list-style-type 列表样式类型
​		*属性值：
​			disc 默认实心圆
​			circle空心圆
​			square方块
​			none没有样式(用得最多)
​	2、list-style-image: url(路径)  列表样式图片
​	3、list-style-position 列表样式位置
​		*属性值： outside在li的内容外边 inside在li的内容里面（不稳定，不常用，有其他方式）
​	*总属性

```
list-style: 1/2 3;
```

​	*用得最多的是

```
list-style:none;
```

​	块级元素除了div、li以外，基本都有默认的margin或padding样式

### 四、背景属性 background

​	1、background-color 背景颜色

```
background-color:red;
background-color:#f00;
```

​	2、background-image:url(路径) 背景图片
​		*当容器的尺寸小于背景图片的尺寸，背景图片会有一部分丢失
​		*当容器尺寸大于背景图片的尺寸，背景图片会平铺满整个容器
​		*当容器尺寸等于背景图片的尺寸，背景图片能刚好在容器中完整的呈现
​	3、background-repeat 背景图片是否平铺
​		*属性值： repeat默认平铺  no-repeat不平铺  repeat-x水平方向平铺  repeat-y垂直方向平铺
​	4、background-pisition背景图片在容器中的定位
​		（1）数值：
​			*背景图片往左移，为负值
​			*背景图片往右移，为负值
​		（2）方位：
​			*水平：left左 center中 right右
​			*垂直： top上 center中 bottom下
​	5、background-attachment背景图片的固定
​		*必须配合有滚动条的元素才能使用
​		*属性值：
​			scroll随着滚动条滚动而滚动
​			fixed滚动条滚动，背景图片固定位置
总属性 background: 1 2 3 4 5;(可以缺省)

### 六、边框 border

​	1、border-width 边框宽度
​	2、border-style 边框样式
​		*属性值：solid实线  dashed虚线  dotted点线 double双线
​	3、border-color 边框颜色
​	*总属性 border:1 2 3;
​	*border-方位：1 2 3
​		*方位：left right top bottom
​	*border-方位-分属性
​		*border-方位-(width/style/color)
​	*分属性要覆盖总属性的某个值，则必须写在总属性的后面

# 四、盒模型

### 一、盒模型的组成

1、盒模型 = content+padding +border+margin
2、标准盒模型：width、height=content
3、怪异盒模型：width、height=content+padding+border
4、box-sizing 规定盒模型的解析方式
	*content-box 标准盒模型，以content以内为width、height大小
	*border-box 怪异盒模型，以border以内为width、height大小

### 二、padding内填充

padding的取值，遵循原则：上右下左，若某个方向的值缺省，找它的反义词的值。
​padding-方位：设置某个方位上的padding
​注意事项：
​	padding不能为负值
​	背景是从padding的左上角开始摆放的，background-position:0 0;在padding的左上角

### 三、margin 外间距

margin的取值，遵循原则：上右下左，若某个方向的值缺省，找它的反义词的值。
​margin-方位：设置某个方位上的margin
​注意事项：
​	margin可以为负值
​	margin区域没有背景
父元素的第一个子元素建议不要设置margin-top，因为可能存在margin塌陷问题

# *五、元素类型*

### 一、元素类型的分类

#### （一）块级元素

​	1.特点：块级元素的宽高默认占其父级元素的一整行，若设置了宽度，多余的区域margin填充。
​		*利用块级元素水平方向多余的margin实现块级元素在父容器中水平居中，给当前元素加margin:0 auto;
​		*块级元素可以设置宽高
​		*块级元素可以理解成容器，可以容纳所有的行内元素及部分的块级元素
​			例如：ul里面只能嵌套li，dl>dt+dd
​			例如：有语义的标签不能在里面嵌套div、p不能嵌套p、标题标签不能嵌套标题
​	2.代表元素
​		div、标题h1-h6、p、列表ul、ol>li;dl>dt+dd、form、option

#### （二）行内元素（内联元素）

​	1.特点：一行显示多个；宽高由内容决定，即不能设置宽高；行内元素遵循盒模型规律，但是设置上下的border、padding、margin并没有真正的生效。
​	2.代表元素
​		span，buis，strong，em，ins，del，a，img，input，select，textarea，label
​	3.实现行内元素在父容器中水平居中
​		*给父容器加text-align:center;

### 二、元素显示类型的转换

​	属性值：*block  转换成块级元素，拥有块级元素的所有特点
​			*inline 转换 成行内元素，拥有行内元素的所有特点
​			*list-item 块级元素的一种特殊显示类型，为列表项
​			*inline-block 转换成行内块级元素
​			*none 隐藏元素，不占位置
（二）dispaly:inline-block;行内元素的一种特殊显示类型
​	特点：一行显示多个；可以设置宽高；
​	代表元素：img/input/textarea
​	存在问题：
​		1.设置成行内块，元素之间的换行会被解析成一个空格
​			解决办法：不换行；给父元素设置font-size为0
​		2.行内元素之间存在基线对齐的问题
​			解决办法：vertical-align;
三、扩展知识

​	行内可置换元素(行内块级元素)：浏览器根据元素的标签和属性，来决定元素的具体显示内容-img[src]/input[type]/textarea[cols]+[rows]
​	行内不可置换元素（行内元素）

##### 隐藏元素的三种方式

1.display:none; 隐藏元素，不占位置

2.visibility:hidden;隐藏元素，占位置；

3.opacity:0;隐藏元素，占位置；

注：overflow:hidden;隐藏溢出容器的内容，不会隐藏容器

# *六、定位*

### 一、定位position

（一）.静态定位static：
​	*元素的默认定位，不设置就是该定位；
​	*标准流中的定位；

（二）.相对定位relative：
​	*相对定位的元素都是相对于自己本身所在的位置进行定位移动；
​	*配合top,right,bottom,left属性使用，若是正值，则从自己的某条边往元素中间移动为正值。
​	*相对定位的元素不脱离标准流。（灵魂出窍）

（三）.绝对定位absolute
​	*绝对定位的元素是相对于有定位的最近的父级元素或者html进行定位的
​	*配合left,right,top,bottom使用，从包含块的某条边往包含块的中间移动为正值。
​	*脱离了标准流
​	扩展：包含块-定位参考框；一般子元素设置了绝对定位，父元素的定位一般都是相	对定位除非有特殊要求。
4.实现任意元素类型的元素在父容器中居中显示
​	

扩展：1.子元素绝对定位absolute,父元素相对定位relative（子绝父相）；
​		

2.给子元素{left:50%;top:50%;margin-left:-自己宽度的一半;margin-top:-自己高度的一半;}
​	

扩展：盒模型相关的属性设置百分比都是根据父元素的宽高作为基准
width,height,padding,border,margin,left,top,right,bottom

（四）.固定定位fixed
​	*固定定位的元素相对于浏览器的可视区域进行定位
​	*配合left,right,top,bottom使用，从浏览器可视区域的某条边往中间移动为正值
​	*固定定位的元素脱离了标准流
拓展：子代选择器（ie8+）>
例如ul>li表示获取ul的子元素li

### 二、层级z-index

​	*层级越高的元素在越上面
​	*默认情况下，定位的层级>标准流中的层级。浮动的层级>标准流中的层级（-1,0,1）
​	*只有设置了定位的元素才可以设置z-index
​	*层级为整数，可以为负数

### 三、overflow

​	内容溢出容器时的处理方式
属性值：
​	visible默认可见
​	hidden隐藏溢出内容
​	scroll出现滚动条
​	auto自动判断溢出出现滚动条
overflow-x: 设置水平方向
overflow-y：设置垂直方向
**<u>当设置了某个方向的overflow不为visible,另外一个方向自动为auto</u>**

# 七、图片整合技术：

设置块、宽高背景、定位absolute（记得给它爸爸加relative）
宽高不一致就单独设置
背景的定位background-position也不一样，单独设置
小图标相对于父元素的位置也不一样，单独设置left、right、top、bottom



**原理：**将一组背景图片有规律的合并成一张背景图（精灵图，雪碧图），再利用background-position实现背景图片的定位。

**好处：**减少页面的请求次数，从而提高页面的加载速度；合并后的图片体积减小，从而提高加载速度；

**背景图：**

​	1.h1：背景图
​	2.小图标：背景图
​	3.轮播图：都可以（建议用背景图）
​	4.每天都要更新的图片：都可以（建议用图片）



# 八、自适应宽高

概念：元素的大小能够根据窗口或子元素自动调整，这就是自适应。
优点：可以适应在不同设备、不同窗口和不同分辨率下显示。

### 一、宽度自适应

概念：块级元素宽度设置成100%，或者不设置宽度，宽度都为父元素的一整行

### 二、高度自适应

概念：父元素高度不设置，或者设置成{height:auto;}可以由子元素撑开

###### （一）高度塌陷：

​	当子元素都浮动了，父元素的高度将没有办法被撑开
1	给父元素加overflow:hidden;缺点：造成该容器一部分布局内容丢失
2	给父元素添加最后一个子元素{height:0;clear:both;overflow:hidden;}缺点：产生大量的标签，影响页面性能
3	伪元素清除法（万能清除法）：给父元素添加类名clearfix

```
.clearfix:after{
     content:"";
     display:block;
     height:0;
     clear:both;
     overflow:hidden;
     visibility:hidden;
     *zoom:1;
 }
```

###### （二）内容为空

​	若子元素内容可能为空的情况下，父元素会出现高度为0的情况。
解决办法：设置最小高度min-height
​	兼容ie6办法1：在高版本浏览器，height为固定高度，但是在ie6，height代表最小高度。
​		*所以只能让ie6才识别到height属性，因此得使用过滤器
​		*_height 下划线属性过滤器，只有ie6才能识别
​	兼容ie6办法2：在高版本浏览器，!important代表最高权重，而在ie6没有这个概念，会解析成普通属性
​		*设置height:auto !important给高版本浏览器识别，再设置height:具体值，事项ie6时覆盖auto属性值

###### （三）自适应窗口高度

​	元素高度自适应窗口高度（移动端和PC端后台用的较多，平时较少使用）
*当元素设置{height:100%;}，即元素高度为父元素的高度
*设置窗口高度为100%，html,body{height:100%;}

给父元素添加类名clearfix
（2）若子元素内容可能为空的情况下，父元素会出现高度为0的情况。
​	解决办法：设置最小高度min-height
​		*当容器内容高度大于最小高度，按内容高度显示；当容器内容高度小于最小高度，按最小高度显示；
​	兼容ie6办法1：在高版本浏览器，height为固定高度，但是在ie6，height代表最小高度。
​		*所以只能让ie6才识别到height属性，因此得使用过滤器
​		*_height 下划线属性过滤器，只有ie6才能识别
​	兼容ie6办法2：在高版本浏览器，!important代表最高权重，而在ie6没有这个概念，会解析成普通属性
​		*设置height:auto !important给高版本浏览器识别，再设置height:具体值，事项ie6时覆盖auto属性值
2.元素高度自适应窗口高度（移动端和PC端后台用的较多，平时较少使用）
​	*当元素设置{height:100%;}，即元素高度为父元素的高度
​	*设置窗口高度为100%，html,body{height:100%;}

### 三、BFC

###### （一）概念：

块级格式化上下文，是一个独立的渲染区域，规定了内部的块如何布局，且不影响外部元素。

###### （二）布局规则

内部的块级元素会在垂直方向上一个接一个摆放
​属于同一个BFC的两个相邻块会发生margin重叠
bfc的区域不会与浮动块重叠(

###### （三）应用场景：

1	解决高度塌陷：通过给父元素添加最后一个子元素{height:0;clear:both;overflow:hidden;}
2	自适应两栏布局：左边固定宽度浮动+右overflow:hidden;左边固定宽度浮动+右margin-left
3	计算BFC的高度时，里面的浮动元素也参与计算（应用场景：给父元素加{overflow:hidden;}解决高度塌陷的问题）
4	BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素（应用场景：解决margin重叠问题：给其中一个元素设置overflow:hidden;将自己的样式全部给其子元素）

###### （四）触发元素成为bfc

1	html根元素
2	overflow不为visible，常用overflow:hidden;
3	浮动
4	脱离标准流的定位
5	display:inline-block; flex;

# 十一、CSS3

html:5声明文档类型为html5
html:4t声明文档类型为html4.01过渡版本
html:4s声明文档类型为html4.01严格版本
html:xt声明文档类型为xhtml1.0过渡版本
html:xs声明文档类型为xhtml1.0严格版本

### 一、选择器

（一）基本选择器
​	通配符，标签，类，id，群组 选择器

（二）层次（关系）选择器

1.后代选择器
E F:匹配到F元素，且F元素是E元素的后代

2.子代选择器
E>F:匹配到F元素，且F元素是E元素的子代

3.相邻兄弟选择器
E+F:匹配到F元素，且F元素是E元素后的第一个元素

4.兄弟选择器
E~F:匹配到F元素，且F元素是E元素后面的元素

（三）动态伪类选择器
:link 锚链接被点击前的样式
:visited锚链接被点击后的样式
:hover鼠标悬停在任意元素上，添加的样式
:actived 鼠标点击（激活）任意元素时，添加的样式
:focus 光标聚焦在表单元素上，添加的样式

（四）目标伪类选择器
E:target  获取到作为目标的E元素

（五）语言伪类选择器
q[lang="no"]会给标签内部的文本加上双引号
:lang(no){
​	quotes: "左符号""右符号";
}

（六）UI元素状态伪类选择器
:enabled给可用的表单元素添加样式
:disabled给不可用的表单元素添加样式
:checked给被选中的元素添加样式

（七）结构伪类选择器

1	E:first-child 父元素的第一个子元素，同时满足为E元素

2	E:last-child 父元素的最后一个子元素，同时满足为E元素

3	E:nth-child(n) n从1开始计数，满足为父元素的第n个子元素，同时为E元素

4	E:nth-last-child(n) n从1开始计数，满足为父元素的倒数第n个子元素，同时为E元素；
​	*2n 第偶数个孩子(even)
​	*2n-1 第奇数个孩子(odd)
​	\*-n+a 满足为父元素的第1个到第a个孩子*

5	E:first-of-type满足为父元素的第一个E类型的子元素

6	E:last-of-type满足为父元素的最后一个E类型的子元素

7	E:nth-of-type(n)满足为父元素的第n个E类型的子元素

8	E:nth-last-of-type(n)满足为父元素的倒数第n个E类型的子元素
9	E:empty 获取到内容为空（连空格都没有）的E元素
10	E:only-child 满足为父元素的唯一的一个孩子，且为E元素
11	E:only-of-type满足为父元素唯一的一个E元素类型的孩子
（八）否定伪类选择器
​	E :not(F) 在E元素的子元素中，除了F元素以外的所有
（九）属性选择器
1	E[attr] 拥有attr属性的E元素会被获取到
2	E[attr="val"] attr属性值为val的E元素会被获取到
3	E[attr*="val"] attr属性值包含val的E元素会被获取到
4	E[attr^="val"] attr属性值以val值开头的E元素会被获取到
5	E[attr$="val"] attr属性值以val结尾的E元素会被获取到
（十）伪类选择器
1	E::before 给E元素添加第一个子元素(行内)
2	E::after 给E元素添加最后一个子元素(行内)
3	E::first-letter 给E元素（块级）第一个文本添加样式
4	E::first-line 给E元素（块级）第一行文本添加样式
5	E::selection给选中的文本添加样式
​	*火狐不支持，加私有前缀 -moz-

### 二、文本属性 text-shadow

1	文本阴影text-shadow: x-offset y-offset blur color[,...可以省略];多个用逗号隔开
​	x-offset 水平偏移
​	y-offset 垂直偏移
​	blur 模糊区域
​	color阴影颜色

2	文本溢出的处理方式 text-overflow
​	

*clip文本溢出直接被裁掉（默认）
​	

*ellipsis 文本溢出用省略号代替

实现单行文本省略：配合overflow:hidden; width;white-space:nowrap;使用

实现多行文本省略：配合overflow:hidden;text-overflow:ellipsis;display:-webkit-box;-webket-line-clamp:2;-webkit-box-orient:vertical;使用

3	单词换行word-wrap
​	normal默认正常显示
​	break-word允许在单词内进行换行

4	单词换行的规则 word-break
​	normal按照默认的换行规则
​	break-all允许在单词内部换行
​	keep-all 只能在空格或连接符处换行
5	使用服务器端字体@font-face{}
​	*font-family给字体起名字
​	*src:url()引入字体路径
​	*font-family使用该字体
​	（2）字体图标
​		www.iconfont.com
​		好处：图片放大会失真，而文字不会；占内存小，从而提高加载速度

### 三、新增颜色模式

1	rgba(red0-255,green0-255,blue0-255,alpha不透明度0-1)
2	hsla(色调0-360，饱和度0-100%，lighter0-100%，alpha不透明度0-1)
3	transparent完全透明
​	利用transparet实现三角形(border)

### 四、边框属性

1	边框阴影box-shadow
​	box-shadow: x-offset y-offset blur spread color 外阴影/内阴影[,...可省略];
​	*x-offset 水平偏移，往右为正
​	*y-offset 垂直偏移，往下为正
​	*blur 模糊区域
​	*spread 扩展区域
​	*color
​	*默认为外阴影outset，内阴影inset
2	边框图片border-image
​	*boorder-image-source: url();引入边框图片，默认放在边框的四个角上
​	*border-image-slice边框图片切割。遵循上右下左原则，若缺省找反义词
​	*border-image-width 边框图片宽度，若没写，默认就是border宽度
​	*border-image-outset 边框图片向外延伸，不能为负数
​	*border-image-repeat 边框图片是否重复，stretch默认拉伸；repeat只重复；round重复完整图形
3	边框圆角border-radius
*将正方形做圆,border-radius:50%;
*border-垂直方位-水平方位-radius:水平半径 垂直半径;
水平方位：left right
垂直方位：top bottom
*border-radius: 水平半径（左上角开始顺时针）/垂直半径（左上角开始顺时针）
若某个方向的半径值缺省，找对角

### 五、伪类及伪元素

伪类模仿类的存在
伪元素模仿元素的存在
二、伪元素::
1、E::before 给E元素添加第一个子元素
​	*{content: "文字或图片路径"} 即使内容为空也不能省略content属性 content: "";
​	*默认情况下为行内元素
2、E::after 给E元素添加最后一个子元素
​	*{content: "文字或图片路径"} 即使内容为空也不能省略content属性 content: "";
​	*默认情况下为行内元素
3、E::first-letter给E元素的第一个字符添加样式
4、E::first-line给E元素的第一行字符添加样式

# 十二、背景属性、弹性盒

### 一、背景属性

1	background-size 规定背景图片的尺寸
*属性值：
​	数值（或百分比）：水平 	垂直；若只写了一个值，代表水平方向的值，垂直方向会等比拉伸。大多数情况下，背景图片会发生变形。若background-size:100% 100%;则图片不会变形（错的）
​	cover 背景图片完全覆盖容器，可能会出现背景图片丢失；背景图片等比缩放，不会出现变形的情况。
​	contain 容器完全包含背景图片，可能会出现留白
*应用：
​	利用{background-size:cover;background-position: center;}实现大图片在容器中的显示
2	background-origin背景图片的定位起始区域
​	padding-box 默认的定位区域为padding以内。
​	content-box 定位区域为content以内
​	border-box 定位区域为border以内
3	background-clip 背景图片的最终显示区域
​	padding-box 默认的显示区域为padding以内。
​	content-box 显示定位区域为content以内
​	border-box 显示区域为border以内
4	多张背景图片的使用

### 二、弹性盒布局

（一）概念原理：容器有能力让其子项目能够改变其宽度、高度（甚至顺序），以最佳的方式填充可用空间
​	*主轴：默认为水平方向
​	*侧轴：主轴的交叉轴，默认为垂直方向
（二）容器的属性
1	display:flex;将容器设置成弹性盒，里面的子项目会在主轴方向顺序排列（不会换行），侧轴方向的大小若缺省，会被默认拉伸
2	flex-direction 设置主轴方向
​	row 从左到右
​	row-reverse 从右到左
​	column 从上往下
​	column-reverse 从下往上
3	flex-wrap 设置主轴方向的换行
​	*nowrap 默认不换行，若主轴方向放不下，子项目进行缩放
​	*wrap 换行
4	justify-content   子项目在主轴方向的对齐方式
​	*flex-start 默认在主轴方向的开始位置顺序摆放
​	*flex-end在主轴方向的结束位置顺序摆放
​	*center 在主轴方向的中间位置顺序摆放
​	*space-between 将主轴方向的剩余空间平分在子项目之间
​	*space-around 将主轴方向的剩余空间环绕在子项目之间
5	align-items 子项目在侧轴方向（单行上）的对齐方式
​	*stretch若子项目在侧轴方向没有设置大小，则在当前行上默认拉伸
​	*flex-start若子项目在侧轴方向设置了大小，默认在侧轴方向（单行上）的开始位置顺序摆放
​	*flex-end在侧轴方向（单行上）的结束位置顺序摆放
​	*center在侧轴方向（单行上）的中间位置顺序摆放
​	*baseline 在侧轴方向（单行上）以基线对齐
6	align-content 多行子项目在侧轴方向的对齐方式
​	*flex-start 默认在侧轴方向的开始位置顺序摆放
​	*flex-end在侧轴方向的结束位置顺序摆放
​	*center 在侧轴方向的中间位置顺序摆放
​	*space-between 将侧轴方向的剩余空间平分在子项目之间
​	*space-around 将侧轴方向的剩余空间环绕在子项目之间
（三）子项目的属性
1	flex 设置子项目的比份，无单位。
2	align-self 单个子项目在侧轴方向（单行上）的对齐方式
​	属性值同align-items
3	order 规定子项目的排列顺序
​	*不定义order的子项目会排到前面
​	*order越小，排在越前面
弹性盒：移动端使用。
​	*老版本语法：需要时查阅
​	*flex 设置子项目在主轴方向的比份
​		*flex-grow定义子项目的扩展比率
​		*flex-shrink定义子项目的收缩比率
​		*flex-basis定义子项目的默认基准值
​		flex属性有两个快捷值：auto(1 1 auto)和none(0 0 auto)。
​			auto:当容器有多余空间，子项目平分剩余空间，放大。当容器空间不足，子项目平分缩小。
​			none:不管容器位置多还是少，子项目都不改变自己本身大小。

### 三、多列布局

1	概念：自动将内容按指定的列数排列，这种特性实现的效果和报纸、杂志类排版非常相似。
2	核心属性：
​	（1）column-count 列数，定义分列列数；最多列数，auto自适应（由列宽、容器宽和列间距决定），也可固定
​	（2）column-width 列宽，定义每列列宽； 类似于最小宽度min-width； auto 自适应；
​	（3）column-gap：定义列间距； 不能为负数；
​	（4）column-rule：定义列边框；与定义边框一样：2px dashed #ccc;
​	（5）column-span：定义多列布局中子元素的跨列效果；通常用于标题；
​		*none：不跨列；
​		*all：跨所有列
​	（6）break-inside: avoid;避免图片与文字断行
columns: column-width column-count;



# 十三、移动端

### （一）设置理想视口

1	布局视口viewpoint：比实际屏幕尺寸大很多，保证页面完整显示，但是是全局缩小后的页面。
2	理想视口viewpoint:meta标签实现meta:vp 移动端一定要记得加上这句代码
​	许多智能手机都使用了一个比实际屏幕尺寸大很多的虚拟可视区域viewpoint(布局视口)，主要目的就是让页面在智能手机端阅读时不会因为实际可视区域变形。所以你看到的页面还是普通样式，即一个全局缩小后的页面。为了让智能手机能根据媒体查询匹配对应样式，让页面在智能手机中正常显示，特意添加了一个meta标签。这个标签的主要作用就是让智能手机浏览页面时能进行优化，并且可以自定义界面可视区域的尺寸和缩放级别。
​	如何识别手机尺寸通过设置meta语句：

```html
<meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no" />
```



width:可视区域的宽度;

height:可视区域的高度;

device-width:设备屏幕分辨率的宽度值

initial-scale:初始的缩放比例（0-10.0），取值为1时页面按实际尺寸显示，无任何缩放
​	

minimum-scale 		允许用户缩放到的最小比例 
​	

maximum-scale 		允许用户缩放到的最大比例
​	

user-scalable 		设定用户是否可以缩放（yes/no）
​	可以写成：

```html
<meta name="viewport" content="width=device-width,initial-scale=1.0" />
```

### （二）媒体查询

1	分界点：
超小屏幕xs  （移动设备）768px以下
小屏设备sm	768px-992px
中等屏幕md	992px-1200px
宽屏设备lg	1200px 以上
2	语法 @media screen and (条件){css语法}
​	*min-width 若当前页面宽度大于min-width，则样式生效。按照从小到大的书写顺序。
​	*max-width
​	*min-device-width
​	*响应式布局：利用媒体查询，在不同的设备、不同的分辨率或者不同的屏幕宽度，对同一套页面的细节进行调整
​		*局限性：一般都只能做简单的页面
​		*bootstrap框架 以响应式出名
一、弹性盒布局
二、等比缩放布局
移动端百分比布局：
1	meta:vp	设置理想视口
2	稿纸640px时，在iphone5开发；稿纸750px，在iphone6开发
3	拷贝三句代码，记得给meta：vp加id名;
4	弹性盒布局：
​	整个页面高度100%，设置成弹性盒，主轴向下
​	除了中间有滚动条的部分设置成flex:1，其他部分直接设置高度;
​	中间有滚动条部分设置overflow-y:auto;overflow-x:hidden;(只设置overflow-x:hidden也可以，另外方向自动auto;)

### （三）rem

​	em 以当前一个字符的大小为基准
​	rem 以html根部字体大小为基准 
移动端rem布局
1	meta:vp	设置理想视口
2	稿纸640px时，在iphone5开发；稿纸750px，在iphone6开发
3	拷贝三句代码，记得给meta：vp加id名;
4	弹性盒布局：
​	整个页面高度100%，设置成弹性盒，主轴向下
​	除了中间有滚动条的部分设置成flex:1，其他部分直接设置高度;
​	中间有滚动条部分设置overflow-y:auto;overflow-x:hidden;(只设置overflow-x:hidden也可以，另外方向自动auto;)
5	拷贝另外2句js代码，目的是让html字体大小根据设备改变。在哪个设备开发，就要用当前根部字体大小将px换算成rem
​	建议字体大小别转rem，用媒体查询做。

### （四）vw与vh

相对于视口的高度和宽度
1vw相当于视口宽度的1%，1vh相当于视口高度的1%

# 十四、渐变+运动

### 一、渐变（背景）

1.线性渐变 linear-gradient
(1)gradient(linear,起点，终点，颜色，[,颜色])
​	兼容问题
​	起点终点可以是数值，0 0代表左上角，100% 100%或者width 大小 height大小代表右下角
(2)gradient(linear,起点,终点,color-stop(颜色开始位置，颜色)[,颜色])
(3)linear-gradient( 方向||角度, `<stop'>`, `<stop'>` [, `<stop'>`]* )
​	* 第一个参数表示线性渐变的方向。
​	·to left：设置渐变为从右到左，相当于: 270deg;
​	·to right：设置渐变从左到右，相当于: 90deg;
​	·to top：设置渐变从下到上，相当于: 0deg;
​	·to bottom：设置渐变从上到下，相当于: 180deg。（默认值）
​	·也可以直接指定度数，如45deg
角度：
​	有前缀的兼容写法，角度跟象限角度一样
​	新版本（不加前缀）角度+老版本 = 90deg
2、radial-gradient 径向渐变
​	从一个中心点开始沿着四周产生渐变效果
radial-gradient([ [ at `<position>` ]? [ `<shape>` || `<size>` ]  , ?`<color-stop>`[ , `<color-stop>` ]+)
​	*shape：渐变的形状，ellipse椭圆形(默认)，circle表示圆形。
​	*size：渐变的大小，即渐变到哪里停止，它有四个值。 
​		closest-side：最近边； farthest-side：最远边；
​		closest-corner：最近角； farthest-corner：最远角（默认值）
​	*`<position>` 确定圆心的位置。
​		如果提供2个参数，第一个表示横坐标，第二个表示纵坐标；
​		如果只提供1个，第二值默认为50%，即center
​	*`<color>`：指定颜色。

### 二、过渡 transition

1.经过一定的时间， 从某个状态到另一个状态
2.分属性
​	(1)transition-property 过渡的属性（不可缺）
​	(2)transition-duration 过渡的时间（不可缺）
​	(3)transition-timing-function 过渡的形式
​		linear线性过渡
​		ease慢速进入慢速离开
​		ease-in慢速进入
​		ease-out慢速离开
​	(4)transition-delay 过渡的延时
3.总属性：
​	transition: 1 2 3 4;

### 三、变换transform

#### （一）2d变换

1.移动变换 translate
​	transform: translate(水平，垂直)
​	可以用百分比，代表自己宽高的百分比
​	transform: translateX()
​	transflorm:translateY()
2.缩放变换 scale(x,y)
​	x,y代表水平，垂直方向的缩放比例
​	缩放的基准点为元素中心
​	transform: scaleX(x)
​	transform: scaleY(y)
3.旋转rotate(角度)
​	transform: rotate(角度),角度以顺时针旋转,旋转的基准点在元素中心
4.扭曲变换 skew()
​	tranform:skew(x轴旋转的角度,y轴旋转的角度),扭曲变换的基准点在元素中间
​	transform: skewX(x轴旋转的角度)
​	transform: skewY(y轴旋转的角度)
注意：若存在多个变换，在书写属性值时，应用空格将多个变换隔开
5.改变元素变换的基准点 transform-origin

#### （二）3d变换

1.移动变换
​	transform:translate3d(x,y,z);
​	transform:translateZ(z);
2.旋转变换
​	transform:rotate3d(x,y,z,deg)
​		xyz取值为0或1,0代表不旋转，1代表旋转
​	transform:rotateX()
​	transform:rotateY()
​	transform:rotateZ()
左手定律：大拇指指向轴的正方向，手指弯曲的方向为旋转的正方向
3.skew()扭曲变换
4.transform-style规定变换的样式（需要设置在父元素上）
​	flat默认为平面
​	preserve-3d使被转换的子元素保留其3D转换
5.perspective设置观察的距离，景深（需要设置在父元素上）
6.perspective-origin设置观察的基准点（设置在父元素上）

### 四、动画animation

1.定义动画@keyframes
​	@keyframes 动画名{
​	节点{样式}
}
​	通过百分比将动画分成多个节点
​	最后通过animation将动画应用到指定元素上
2.animation用于设置动画属性
​	animation-name 动画名字
​	animation-duration 动画每次的播放时间
​	animation-timing-function 动画播放形式；linear ease ease-in ease-out；steps(n)相邻两个节点之间分成多少步，默认情况下每步都是用初始状态填充分配下来的时间段，steps(n)==>steps(n,end)默认; steps(n,start)用每一步的结束状态填充分配下来的时间
​	animation-delay动画延迟
​	animation-iteration-count 动画的播放次数；infinite无限次
​	animation-direction动画是否轮流反向播放； reverse方向播放 alternate交替播放 alternate-reverse 交替方向播放
​	animation-fill-mode动画完成模式；forwards保持最后一个状态



# 格式化上下文

（`formatting contexts`）

`Formatting context`是`W3C CSS2.1`规范中的一个概念。它是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系、相互作用。
格式化上下文指的是初始化元素定义的环境。包含两个要点，元素定义的环境和初始化。

在 `CSS` 中，元素定义的环境有两种，一种是块格式化上下文( `Block formatting context` )，另一种是行内格式化上下文( `Inline formatting context` )。 这两种上下文定义了在 `CSS` 中元素所处的环境，格式化则表明了在这个环境中，元素处于此环境中应当被初始化，即元素在此环境中应当如何布局等。

## 格式化上下文包含以下几种情况

a：块级格式化上下文( `Block formatting contexts` )( BFC )
b：行内格式化上下文( `Inline formatting contexts` ) ( IFC )
c：自适应格式化上下文( `Flex Formatting Contexts` )( FFC )（CSS3新增）
d：网格布局格式化上下文( `GridLayout Formatting Contexts` )( GFC )（CSS3新增）

## `BOX`:CSS布局的基本单位

`Box` 是 `CSS` 布局的对象和基本单位， 直观点来说，就是一个页面是由很多个 `Box` 组成的。元素的类型和 `display` 属性，决定了这个 `Box` 的类型。 不同类型的 `Box`， 会参与不同的 `Formatting Context`（一个决定如何渲染文档的容器），因此`Box`内的元素会以不同的方式渲染。让我们看看有哪些盒子：

a：block-level box:`display` 属性为 `block, list-item, table` 的元素，会生成 `block-level box`。并且参与 `block fomatting context`；

c：inline-level box:display 属性为 `inline, inline-block, inline-table` 的元素，会生成 `inline-level box`。并且参与 `inline formatting context`；



# 布局拓展

#### 设置元素在父元素上居中

​	1、利用行内块级元素
​	垂直居中：
​		*将元素设置成行内块级元素，同时尺子也要设置成行内块，高度与父元素一致，宽度为0；
​		*将与元素及尺子都要设置vertical-align:middle;
​	水平居中：
​		*给父容器设置text-align:center;
若子元素与父元素都浮动了，父元素可以不设置宽度，父元素能被子元素撑开；



#### 自适应两栏布局

某一栏确定宽度+浮动，另外一栏设置margin的值为上一栏的宽度，自己的宽度不设置

#### 自适应三栏布局

先写左右的元素设置宽度+浮动，最后才写中间的元素，设置margin

 

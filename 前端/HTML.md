## 一、HTML文件格式

1、文件名

xx.html

2.文档格式

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  
</body>
</html>
```

`<!DOCTYPE html>`告知文档类型为html5

`charset="UTF-8"`字符编码为UTF-8

`<head>,<meta>,<title>`标签默认样式display:none

## 二、html的基本语法

1	html标签： 由尖括号包裹起来的一个整体

2	html元素： 由一对双标签或者一个单标签组成

3	html属性：[html属性名="属性值"]，紧跟标签名字后面，空格隔开。一个html属性的多个属性值，也用空格隔开

4	html注释:书写一些信息方便自己或他人理解，不会显示在页面中

`<!--注释-->`ctrl/command+？

```html
<!-- 注释 -->

<!-- 单标签-->
<br />
```

## 三、HTML基础标签

#### 1.标题标签h1-h6

标题的语义从重到轻，默认样式从大到小，建议每个页面只用一次h1

| 标签   | display | font-weight | font-size | margin-block-start | margin-block-end |      |      |
| ---- | ------- | ----------- | --------- | ------------------ | ---------------- | ---- | ---- |
| h1   | block   | bold        | 2em       | 0.67em             | 0.67em           |      |      |
| h2   | block   | bold        | 1.5em     | 0.83em             | 0.83em           |      |      |
| h3   | block   | bold        | 1.17em    | 1em                | 1em              |      |      |
| h4   | block   | bold        | 1em       | 1.33em             | 1.33em           |      |      |
| h5   | block   | bold        | 0.83em    | 1.67em             | 1.67em           |      |      |
| h6   | block   | bold        | 0.67em    | 2.33em             | 2.33em           |      |      |

Chrome、FireFox

#### 2.段落标签`<p></p>`

语义：表示文本段落，一个p元素表示一个段落

#### 3.列表标签`ul、ol`

* 无序列表ul（unordered list）>  li列表项（list item）
* 有序列表ol（ordered list）>  li列表项（list item）
* 自定义列表dl（definition list）>  dt自定义列表项（definition term） +  dd对自定义列表项的描述（definition description）

ul、ol的子元素只能是li，不可以是其他元素

#### 4.表格标签`table`

table > tr行 > td单元格

table的html属性：

​	[border给表格加上边框]

​	[cellpadding 给表格单元格加上内填充（内容与边框之间的距离）]

​	[cellspacing 单元格之间的距离]

​	[width 表格宽度]

​	[height 表格高度] 

td的html属性：
​	[colspan合并列]
​	[rowspan合并行]

#### 5.表单标签`form`

（1）表单的作用：收集数据，提交到服务器

​	form

​		[aciton提交到的后台地址]

​		[method提交方式get/post]

（2）input表单控件：[type规定表单控件的类型]

​	[type]

​		text文本框

​		password密码框

​		radio单选框

​		checkbox多选框

​		button按钮框

​		submit提交按钮（提交功能）

​		reset重置按钮

​	[value]

​		给表单控件添加内容

​	[name]

​		给同一组的单选框或多选框起相同的名字；

​		拥有name属性的表单控件内容才可以提交到服务器

​	[checked]

​		给单选框或多选框设置默认选中

（3）select > option 下拉列表

​	option

​		[value] 该option选项的值

​		[selected] 下拉列表的选项被默认选中

（4）textarea文本域

​	[cols]列表

​	[rows]行数

#### 6.换行标签`<br />`

#### 7.分割线标签`<hr />`

#### 8.视觉标签

​	b 加粗     i倾斜     u下划线    s删除线

#### 9.语义标签

​	strong加粗     em倾斜    ins下划线    del删除线

#### 10.图片标签

​	[src]引入图片的路径

​		相对路径、绝对路径

​	[alt]图片未加载出来，呈现的文字

​	[title]光标移入图片时，呈现的文字

#### 11.没有语义的元素

div、span

#### 12.锚点标签`<a>`

（1）跳转到其他页面

（2）跳转到当前页面某个部分（命名锚点）

​	给元素设置id名，给锚链接设置[href="#id名"]，当点击锚链接，就能跳转到id名所在的元素

（3）跳转到其他页面某个部分（命名锚点）

​	给元素设置id名，给锚链接设置[href="路径#id名"]，当点击锚链接，就能跳转到id名所在的元素

（4）不跳转

​	如果使用了a标签，但是不想要a标签跳转

* 最常见的，不会跳转，但是会刷新页面a[href="#" ]


* onclick事件返回false; a[href="#" onclick="return false"]


* ### 用href="javascript:void(0)"伪协议，不会跳转也不会刷新 a[href="javascript:void(0)"]

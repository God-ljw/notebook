# 一、html的基本语法

## 一 、格式

### 1.骨架

| 标签名          | 定义     | 说明                                                 |
| --------------- | -------- | ---------------------------------------------------- |
| <html></html>   | HTML标签 | 根标签                                               |
| <head></head>   | 文档头部 | 注意head标签中必须设置的标签是title                  |
| <title></title> | 文档标题 | 让页面拥有属于自己的网页标题                         |
| <body></body>   | 文档主题 | 元素包含文档的所有内容，页面内容基本都是放在body里面 |

### 2.文档格式

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

`<!--注释-->`ctrl/command+？

```html
<!-- 注释 -->

<!-- 单标签-->
<br />
```

## 二、基本语法

#### 1.标题标签h1-h6

<h1>标题一共六级选,</h1>
       特点：
        1.加了标题的文字会变的加粗，字号也会依次变大。
        2. 一个标题独占一行。
        
       	<h2>文字加粗一行显。</h2>
        <h3>由大到小依次减，</h3>
        <h4>从重到轻随之变。</h4>
        <h5>语法规范书写后，</h5>
        <h6>具体效果刷新见。</h6>

#### 2.段落标签`<p></p>`

语义：表示文本段落，一个p元素表示一个段落

特点：
      1. 文本在一个段落中会根据浏览器窗口的大小自动换行。
      2. 段落和段落之间保有空隙。

#### 3.div和span标签

```
<div> 和 <span> 是没有语义的，它们就是一个盒子，用来装内容的。
具体实现：
         <div> 这是头部 </div>    
         <span> 今日价格 </span>
    特点：
        1. <div> 标签用来布局，但是现在一行只能放一个<div>。 大盒子
        2. <span> 标签用来布局，一行上可以多个 <span>。小盒子
```

#### 4.`<img>`标签

```
实现：
<img src="图像URL" />
 src 是<img>标签的必须属性，它用于指定图像文件的路径和文件名。
```

| 属性   | 属性值   | 说明                               |
| ------ | -------- | ---------------------------------- |
| src    | 图片路径 | 必须属性                           |
| alt    | 文本     | 替换文字。图像不能显示的文字       |
| title  | 文本     | 提示文本。鼠标放在图像上，显示文字 |
| width  | 像素     | 设置图像宽度                       |
| height | 像素     | 设置图像高度                       |
| border | 像素     | 设置图像边框粗细                   |

图像标签注意点：
        1.图像标签可以拥有多个属性，必须写在标签名的后面。
        2.属性之间不分先后顺序，标签名与属性、属性与属性之间均以空格分开。
        3.**属性采取键值对的格式，即 key=“value" 的格式，属性 =“属性值”。**

#### 5.路径

**相对路径和绝对路径**

- **相对路径**：以引用文件所在位置为参考基础，而建立出的目录路径。

  - | 相对路径   | 符号 | 说明                                                      |
    | ---------- | ---- | --------------------------------------------------------- |
    | 同一级路径 |      | 图像文件位于html文件同一级 如<img src="baidu.gif">        |
    | 下一级路径 | /    | 图像文件位于html文件下一级 如<img src="images/baidu.gif"> |
    | 上一级路径 | ../  | 图像文件位于html文件上一级 如<img src="../baidu.gif">     |

- **绝对路径**：是指目录下的绝对位置，直接到达目标位置，通常是从盘符开始的路径。（**全路径名**）

#### 6.链接标签`<a>`

```
<a href="跳转目标" target="目标窗口的弹出方式"> 文本或图像 </a>
```

- 属性：
  - href：用于指定链接目标的url地址
  - target:用于指定链接页面的打开方式**_self**默认值 **_blank**新窗口打开
  - href中写**"#"**为空链接

```html
1.外部链接: 例如 < a href="http:// www.baidu.com "> 百度</a >。
    2.内部链接:网站内部页面之间的相互链接. 直接链接内部页面名称即可，例如 < a href="index.html"> 首页 </a >。
    3.空链接: 如果当时没有确定链接目标时，< a href="#"> 首页 </a > 。
    4.下载链接: 如果 href 里面地址是一个文件或者压缩包，会下载这个文件。
    5.网页元素链接: 在网页中的各种网页元素，如文本、图像、表格、音频、视频等都可以添加超链接.
    6.锚点链接:  点我们点击链接,可以快速定位到页面中的某个位置. 
```

#### 7.表格标签`table`

- 一个可选的<caption>元素 -- 表格标题
- 零个或多个的<colgroup>元素 --表格列组
- 一个可选的<thead>元素 --题头（开始标签是必须的） 下列任意一个：
  - 零个或多个<tbody> -- 正文
  - 零个或多个<tr> --表行
    - 一个或多个<th>--表头
    - 一个或多个<td>--表格单元
- 一个可选的<tfoot>元素

```HTML
<table>
        <caption>内容</caption>
        <colgroup>

        </colgroup>
        <thead> <!--可以不用这个标签 如果使用了 就必须使用以下的标签 -->
            <tr>
                <th></th>
                <th></th>
            </tr>
        </thead>
        <tbody> <!--可以不用这个标签 如果使用了 就必须使用以下的标签 -->
            <tr>
                <td></td>
                <td></td>
            </tr>
            <td>
                <td></td>
                <td></td>
            </td>
        </tbody>
        <tfoot> <!--可以不用这个标签-->
            <tr>
                <td></td>
                <td></td>
            </tr>
        </tfoot>
    </table>
```

```
1.<table> </table> 是用于定义表格的标签。
2.<tr> </tr> 标签用于定义表格中的行，必须嵌套在 <table> </table>标签中。
3.<td> </td> 用于定义表格中的单元格，必须嵌套在<tr></tr>标签中。
4.字母 td 指表格数据（table data），即数据单元格的内容。
```

- <th>表头（用<thead>括起来）

- ```html
  	<table>
              <tr>
                  <th>姓名</th> <!--表头-->
                  ...
              </tr>
              ...
        </table>
  ```

  1.一般表头单元格位于表格的第一行或第一列，表头单元格里面的文本内容加粗居中显示.
              <th> 标签表示 HTML 表格的表头部分(table head 的缩写)
  2.一般表头单元格位于表格的第一行或第一列，表头单元格里面的文本内容加粗居中显示.
              <th> 标签表示 HTML 表格的表头部分(table head 的缩写)
  5.表头单元格也是单元格，常用于表格第一行突出重要性，表头单元格里面的文字会加粗居中

- **表格样式（不常用） 用css样式代替了**

| 属性名      | 属性值              | 描述                                            |
| ----------- | ------------------- | ----------------------------------------------- |
| align       | left、center、right | 元素对齐方式                                    |
| border      | 1或""               | 规定表格单元是否拥有边框，默认为"",表示没有边框 |
| cellpadding | 像素值              | 单元边沿于其内容之间的空白，默认像素为1         |
| cellspacing | 像素值              | 规定单元格之间的空白，默认像素为2               |
| width       | 像素值或百分比      | 规定表格的宽度                                  |

- **合并单元格**
  - **跨行合并**：rowspan="合并单元格的个数"  
  -  **跨列合并**：colspan="合并单元格的个数"

#### 8.列表标签`ul、ol`

* **无**序列表**ul**（unordered list）>  li列表项（list item）

* **有**序列表**ol**（ordered list）>  li列表项（list item）

* 自定义列表dl（definition list）>  dt自定义列表项（definition term） +  dd对自定义列表项的描述（definition description）

  * **ul、ol的子元素只能是li，不可以是其他元素**

* 自定义列表

  * **使用场景**:自定义列表常用于对**术语**或**名词**进行解释和描述，**定义列表的列表项前没有任何项目符号**

  * ```
    <dl> 标签用于定义描述列表（或定义列表），该标签会与 <dt>（定义项目/名字）和 <dd>（描述每一个项目/名字）一起使用
    ```

    ```html
     <dl>   <dt>名词1</dt>   <dd>名词1解释1</dd>   <dd>名词1解释2</dd> </dl>
    ```

* 总结

  * | 标签名    | 定义       | 说明                |
    | --------- | ---------- | ------------------- |
    | <ul></ul> | 无序标签   | 只能包含li          |
    | <ol></ol> | 有序标签   | 只能包含li 较少使用 |
    | <dl></dl> | 自定义标签 | 只能包含 dt dd      |


#### 9.表单标签`form`

```html
<form action=“url地址” method=“提交方式” name=“表单域名称">各种表单元素控件
      <input type="text" name="username" id="username">                                         
</form>
```

- <input type="属性值"  />

- 根据不同的 type 属性值，输入字段拥有很多种形式（可以是文本字段、复选框、掩码后的文本控件、单选按钮、按钮等）

- | 属性值   | 描述                                                         |
  | -------- | ------------------------------------------------------------ |
  | button   | 定义可点击按钮                                               |
  | checkbox | 多选框                                                       |
  | file     | 定义输入字段和"浏览"按钮，供文件上传                         |
  | hidden   | 定义隐藏的输入字段                                           |
  | image    | 定义图像形式的按钮提交方式                                   |
  | password | 定义密码字段。该字段的字符被掩盖                             |
  | radio    | 定义单选按钮                                                 |
  | reset    | 定义重置按钮                                                 |
  | submit   | 定义提交按钮，发送数据到服务端                               |
  | text     | 定义单行的输入字段，用户可在其中输入文本。默认宽度为20个字符 |

- 除 type 属性外，<input>标签还有其他很多属性，其常用属性如下：

- | 属性        | 属性值                   | 描述                                    |
  | ----------- | ------------------------ | --------------------------------------- |
  | name        | 有用户自定义             | 定义input元素的名称                     |
  | value       | 有用户自定义             | 规定input元素的值（默认显示在文本框中） |
  | checked     | 默认被选择               | 默认被选择                              |
  | maxlength   | 正整数                   | 规定输入的字段的字符最大长度。          |
  | selected    | 下拉列表的选项被默认选中 | 下拉列表的选项被默认选中                |
  | placeholder | 输入框提示               | 属性提供可描述输入字段预期值的提示信息  |
  
- `<select>`表单元素 

  - 下拉框

  - ```html
    城市：<select>
            <option>北京</option>
            <option>广州</option>
            <option>上海</option>
        </select>
    ```

- `<textarea> `表单元素

  - **使用场景**: 当用户输入**内容较多**的情况下，我们就不能使用文本框表单了，此时我们可以使用 <textarea> 标签。

  - [cols]列表 [rows]行数

  - ```html
    <form>
    
    		<input type=“text " name=“username”>
    
    		<select name="jiguan">  
    
    		 <option>北京</option>
    
    		</select> 
    
    		<textarea name= "message">
    
    		</textarea>
    
    </form>
    ```

    

#### 10.`<label>` 标签

- <label> 标签为 input 元素定义标注（标签）。
- <label> 标签用于绑定一个表单元素, 当点击<label> 标签内的文本时，浏览器就会自动将焦点(光标)转到或者选择对应的表单元素上,用来增加用户体验.

```html
<label for="sex">男</label>
    <input type="radio" name="sex"  id="sex" />
<!--核心： <label> 标签的 for 属性应当与相关元素的 id 属性相同。-->

<label for="username">用户名：</label>
    <input type="text" name="username" id="username">
    <br>
    <label for="password">密码：</label>
    <input type="text" name="passwork" id="password">

    <br>
    性别：
    <input type="radio" name="gender" id="male" value="男"> <label for="male">男</label>
    <input type="radio" name="gender" id="female" value="nv"> <label for="female">女</label>
    <br>
```

#### 11.字体标签格式（容易忘记）

- strong加粗     em倾斜    ins下划线    del删除线
- b 加粗     i倾斜     u下划线    s删除线

#### 12.换行标签`<br />`

#### 13分割线标签`<hr />`

# 二、HTML5新增特性

## 1. HTML5新特性

发展到了HTML5后，新增了一些语义化标签，这样的话更加有利于浏览器的搜索引擎搜索，也方便了网站的seo（Search Engine Optimization，搜索引擎优化），下面就是新增的一些语义化标签

### 1.1 HTML5新增语义化标签

- `<header>`：头部标签
- `<nav>`：导航标签
- `<article>`：内容标签
- `<section>`：定义文档某个区域
- `<asider>`：侧边栏标签
- `<footer>`：尾部标签
- ![image-20221211154533606](https://picgo-img-pic.oss-cn-guangzhou.aliyuncs.com/img/202402272340913.png)

### 1.2 HTML5新增多媒体标签

#### 1. 视频 `<video>`

所有浏览器支持 mp4 格式。

**使用语法：**

```html
 <video src="media/mi.mp4"></video>
```

- #### 兼容写法

  由于各个浏览器的支持情况不同，所以我们会有一种兼容性的写法，这种写法了解一下即可

  ```html
  <video  controls="controls"  width="300">
      <source src="move.ogg" type="video/ogg" >
      <source src="move.mp4" type="video/mp4" >
      您的浏览器暂不支持 <video> 标签播放视频
  </ video >
  ```

  **上面这种写法，浏览器会匹配video标签中的source，如果支持就播放，如果不支持往下匹配，直到没有匹配的格式，就提示文本**

  **video 常用属性**

- `autoplay="autoplay"`自动播放（谷歌游览器需要添加muted）

- `controls="controls"` 显示控件

- `width` 设置宽度

- `height` 设置高度

- `loop=loop` 设置循环播放

- `preload="auto/none"` 是否预加载

- `src=url` 视频地址

- `poster=url` 封面图片

- `muted=muted` 静音播放

#### 2. 音频 `<audio>`

**使用语法：**

```html
<audio src="media/music.mp3"></audio>
```

所有浏览器支持 mp3 格式。

#### 兼容写法

由于各个浏览器的支持情况不同，所以我们会有一种兼容性的写法，这种写法了解一下即可

```html
< audio controls="controls"  >
    <source src="happy.mp3" type="audio/mpeg" >
    <source src="happy.ogg" type="audio/ogg" >
    您的浏览器暂不支持 <audio> 标签。
</ audio>
```

**上面这种写法，浏览器会匹配audio标签中的source，如果支持就播放，如果不支持往下匹配，直到没有匹配的格式，就提示文本**

**audio 常用属性**

- `controls`：显示控件
- `autoplay`：（谷歌禁用）
- `loop=loop` 设置循环播放

### 1.3 HTML5 新增 input 类型

- `type="email"`
- `type="url"`
- `type="date"`
- `type="time"`
- `type="month"`
- `type="week"`
- `type="number"`
- `type="tel"`
- `type="search"`
- `type="color"`

### 1.4 HTML5新增的表单属性

| 属性         | 值        | 说明                                                         |
| ------------ | --------- | ------------------------------------------------------------ |
| required     | required  | 表单拥有该属性表示其内容不能为空，必填                       |
| placeholder  | 提示文本  | 表单的提示信息                                               |
| autofocus    | autofocus | 自动聚焦属性，页面加载完成自动聚焦到指定表单                 |
| autocomplete | off/on    | 当用户在字段开始键入时，浏览器基于之前键入过的值，应该显示出在字段中填写的选项默认已经打开,如autocomplete="on",关闭autocomplete ="off" 需要放在表单内，同时加上name属性，同时成功提交 |
| multiple     | multiple  | 可以多选文件                                                 |

可以通过以下设置方式修改placeholder里面的字体颜色：

```css
input::placeholder {
    color: pink;
}Copy to clipboardErrorCopied
```

## 
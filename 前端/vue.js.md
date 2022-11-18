## 3. Vue 核心技术

#### 3.1 v-model的运用

```
<body>
    <div id="app">
        <!-- {{}}用于标签体内显示数据 -->
        Hello, {{ content }} <br />
        Hello2, {{ content }} <br />
        <h3 v-text="content"></h3>
        <!-- v-model进行数据的双向绑定 -->
        <input type="text" v-model="content">
    </div>
    <div id="app2"></div>
    <script src="./node_modules/vue/dist/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app', //指定被Vue管理的入口, 值为选择器, 不可以指定body或者是html
            data: { // 用于初始化数据，在Vue实例管理的Dom节点下，可通过模板语法来引用
                content: 'Vue.js'
            }
        })
    </script>
</body>
```

#### 3.2 分析 MVVM 模型https://segmentfault.com/a/1190000017199142

常见面试题：什么是 MVVM 模型？ MVVM 是 Model-View-ViewModel 的缩写，它是一种软件架构风格
Model：模型， 数据对象（data选项当中 的）View：视图，模板页面（用于渲染数据） ViewModel：视图模型，其实本质上就是 Vue 实例核心思想
通过数据驱动视图 把需要改变视图的数据初始化到 Vue中，然后再通过修改 Vue 中的数据，从而实现对视图的更新声明式编程
按照 Vue 的特定语法进行声明开发，就可以实现对应功能，不需要我们直接操作Dom元素
命令式编程Jquery它就是，需要手动去操作Dom才能实现对应功能

![img](C:/Users/10850/Desktop/index_files/435a8558-8eff-48f1-a444-e3868ad0c126.jpg)

#### 3.3 Vue Devtools 插件安装

Vue Devtools 插件让我们在一个更友好的界面中审查和调试 Vue 项目。

- 谷歌浏览器访问： chrome://extensions ，然后右上角打开 开发者模式 ， 打开的效果如下；
- 教程：https://www.cnblogs.com/chenhuichao/p/11039427.html

 

```
教程：1.克隆仓库：https://github.com/vuejs/vue-devtools.git
2.进入目录；
3.安装依赖：npm install
4.找到 manifest.json。修改配置。把"persistent":false改成true
5.打包：npm run build
6.去到浏览器扩展程序：点击加载已解压程序按钮->vue-devtools\packages\shell-chrome
```

![img](C:/Users/10850/Desktop/index_files/8db20cbe-0716-462f-972b-d92dfcbe0c92.png)

- 将软件直接拖到上面页面空白处，会自动安装
- 当你访问Vue开发的页面时，按 F12 可 Vue 标签页

![img](C:/Users/10850/Desktop/index_files/d3460165-f36c-46d6-8d8c-44f8cd32cb29.png)

#### 3.4 模板数据绑定渲染3.4.1 双大括号语法 {{}}格式： {{表达式}} 
作用： 
使用在标签体中，用于获取数据 可以使用 JavaScript 表达式 3.4.2 一次性插值 v-once通过使用 v-once 指令，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新。
3.4.3 输出HTML指令 v-html格式： v-html='xxxx'
作用： 如果是HTML格式数据，双大括号会将数据解释为普通文本，为了输出真正的 HTML，你需要使用 vhtml 指令
3.4.4 输出文本指令 v-text格式： v-text='xxxx'
作用：渲染数据到页面，但是不会渲染标签，当做文本处理
3.4.5 解决{{}}闪烁问题 v-cloak

- 格式： v-cloak
- 作用：为了解决{{}}的闪烁问题

 

```
<style>
        [v-cloak]{
            display: none
        }
    </style>
<body>
    <!-- 
        {{JS表达式}}
     -->
    <div id="app">
        <h3>1、{{}}双大括号输出文本内容</h3>
        <!-- 文本内容 -->
        <p>{{message}}</p>
        <!-- JS表达式 -->
        <p>{{score + 1}}</p>
        <h3>2、 一次性插值 v-once </h3>
        <span v-once>{{message}}</span>
        <h3>3、指令输出真正的 HTML 内容 v-html</h3>
        <p>双大括号：{{contentHtml}}</p>
        <!-- 
             v-html: 
             1. 如果输出的内容是HTML数据，双大括号将数据以普通文本方式进行输出，
             为了输出真正ＨＴＭＬ的效果，就需要使用v-html 指定
            
         -->
        <p>v-html: <span v-html="contentHtml"></span></p>
        <h3>4、指令输出内容 v-text</h3>
        <span v-text="contentHtml"></span>
        
        <h3>5、指令解决问题v-clock</h3>
        <p v-clock>{{message}}</p>
    </div>
    <script src="./node_modules/vue/dist/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                message: '钢铁侠',
                score: 100,
                contentHtml: `<span style="color:red">此内容为红色字体
                    alert('hello world')
                    </span>`
            }
        })
    </script>
</body>
```

#### 3.5 元素属性绑定指令 v-bind完整格式： v-bind:元素的属性名='xxxx'
缩写格式： :元素的属性名='xxxx' 
作用：将数据动态绑定到指定的元素上

#### 3.6 事件绑定指令 v-on

- 完整格式： v-on:事件名称="事件处理函数名"
- 缩写格式： @事件名称="事件处理函数名" 注意： @ 后面没有冒号
- 作用：用于监听 DOM 事件

 

```
<body>
   <div id="app">
        <h3>1、元素绑定指令 v-bind</h3>
        <img v-bind:src="imgUrl">
        <img :src="imgUrl">
         <a :href="mxgUrl">跳转</a>
         <h3>2、事件绑定指令 v-on</h3>
         <input type="text" value="1" v-model="num">
         <button @click="add">点击+1</button>
     </div>
     <script src="./node_modules/vue/dist/vue.js"></script>
     <script>
         var vm = new Vue({
            el: '#app',
            data: {
                imgUrl: 'https://cn.vuejs.org/images/logo.png',
                mxgUrl: 'https://www.baidu.com',
                num: 10
            },
            methods: { // 指定事件处理函数 v-on:事件名="函数名" 来进行调用
                add: function() { //定义了add函数
                    console.log('add被调用')
                    this.num ++
                }
            }
         })
     </script>
</body>
```

### 3.7 计算属性和监听器

#### 3.7.1 计算属性 computedcomputed 选项定义计算属性

计算属性 类似于 methods 选项中定义的函数
计算属性 会进行缓存，只在相关响应式依赖发生改变时它们才会重新求值。函数 每次都会执行函数体进行计算computed 选项内的计算属性默认是 getter 函数,不过在需要时你也可以提供一个 setter3.7.2 监听器 watch

- 当属性数据发生变化时,对应属性的回调函数会自动调用, 在函数内部进行计算
- 通过 watch 选项 或者 vm 实例的 $watch() 来监听指定的属性

 

```
<body>
    <div id="app">
        数学：<input type="text" v-model="mathScore">
        英语：<input type="text" v-model="englishScore"><br>
        <!-- v-model调用函数时，不要少了小括号 -->
        总得分（函数-单向绑定）：<input type="text" v-model="sumScore()"><br>
        <!-- 绑定计算属性后面不加上小括号 -->
        总得分（计算属性-单向绑定）：<input type="text" v-model="sumScore1"><br>
        总得分（计算属性-双向绑定）：<input type="text" v-model="sumScore2">
        <!-- 通过 watch 选项 监听数学分数， 当数学更新后回调函数中重新计算总分sumScore3 -->
        总得分（监听器）: <input type="text" v-model="sumScore3">
    </div>
    <script src="./node_modules/vue/dist/vue.js"></script>
    <script>
        /* 
        1. 函数没有缓存，每次都会被调用
        2. 计算属性有缓存 ，只有当计算属性体内的属性值被更改之后才会被调用，不然不会被调用
        3. 函数只支持单向
        4. 计算属性默认情况下：只有getter函数，而没有setter函数，所以只支持单向
            如果你要进行双向，则需要自定义setter函数
        */
        var vm = new Vue({
            el: '#app',
            data: {
                mathScore: 80,
                englishScore: 90,
                sumScore3: 0 // 通过监听器，计算出来的总得分
            },
            methods: {
                sumScore: function () { //100
                    console.log('sumScore函数被调用')
                    // this 指向的就是 vm 实例 , 减0是为了字符串转为数字运算
                    return (this.mathScore - 0) + (this.englishScore - 0)
                }
            },
            computed: { //定义计算属性选项
                //这个是单向绑定，默认只有getter方法
                sumScore1: function () { //计算属性有缓存，如果计算属性体内的属性值没有发生改变，则不会重新计算，只有发生改变 的时候才会重新计算
                    console.log('sumScore1计算属性被调用')
                    return (this.mathScore - 0) + (this.englishScore - 0)
                },
                sumScore2: { //有了setter和getter之后就可以进行双向绑定
                    //获取数据
                    get: function () {
                        console.log('sumScore2.get被调用')
                        return (this.mathScore - 0) + (this.englishScore - 0)
                    },
                    //设置数据， 当 sumScore2 计算属性更新之后 ，则会调用set方法
                    set: function (newValue) { // newVulue 是 sumScore2 更新之后的新值
                        console.log('sumScore2.set被调用')
                        var avgScore = newValue / 2
                        this.mathScore = avgScore
                        this.englishScore = avgScore
                    }
                }
            },
            //监听器，
            watch: {
                //需求：通过 watch 选项 监听数学分数， 当数学更新后回调函数中重新计算总分sumScore3
                mathScore: function (newValue, oldValue) {
                    console.log('watch监听器,监听到了数学分数已经更新')
                    //  newValue 是更新后的值，oldValue更新之前的值
                    this.sumScore3 = (newValue - 0) + (this.englishScore - 0)
                }
            },
        })
        //监听器方式2: 通过 vm 实例进行调用
        //第1个参数是被监听 的属性名， 第2个是回调函数 
        vm.$watch('englishScore', function (newValue) {
            //newValue就是更新之后的英语分数
            this.sumScore3 = (newValue - 0) + (this.mathScore - 0)
        })
        vm.$watch('sumScore3', function (newValue) {
            //newValue就是更新之后部分
            var avgScore = newValue / 2
            this.mathScore = avgScore
            this.englishScore = avgScore
        })
    </script>
</body>
```

#### 3.8 Class 与 Style 绑定 v-bind

- 通过 class 列表和 style 指定样式是数据绑定的一个常见需求。它们都是元素的属性，都用 v-bind 处理，其中表达式结果的类型可以是：字符串、对象或数组

- 语法格式

- - v-bind:class='表达式' 或 :class='表达式

  - class 的表达式可以为：

  - - 字符串 :class="activeClass"
    - 对象 :class="{active: isActive, error: hasError}"
    - 数组 :class="['active', 'error']" 注意要加上单引号,不然是获取data中的值
    - v-bind:style='表达式' 或 :style='表达式'`

  - style 的表达式一般为对象

  - - :style="{color: activeColor, fontSize: fontSize + 'px'}"
    - 注意：对象中的value值 activeColor 和 fontSize 是data中的属性

 

```
<style>
        .active {
            color: green; 
        }
        .delete {
            background: red;
        }
        .error {
            font-size: 35px;
        }
    </style>
</head>
<body>
    <div id="app">
        <h3>Class绑定，v-bind:class 或 :class </h3>
        <!-- <p class="active">字符串表达式</p> -->
        <p v-bind:class="activeClass">字符串表达式</p>
        <!-- key值是样式 名，value值是data中绑定的属性
        当isDelete为true的时候，delete就会进行渲染
        -->
        <p :class="{delete: isDelete, error: hasError}">对象表达式</p>
        <p :class="['active', 'error']">数组表达式</p>
        <h3>Style绑定，v-bind:style 或 :style</h3>
        <p :style="{color: activeColor, fontSize: fontSize + 'px'}">Style绑定</p>
        
    </div>
    <script src="./node_modules/vue/dist/vue.js"></script>
    <script>
        new Vue({
            el: '#app',
            data: {
                activeClass: 'active',
                isDelete: false,
                hasError: true,
                activeColor: 'red',
                fontSize: 100
            }
        })
    </script>
</body>
```

#### 3.9 条件渲染 v-if

#### 3.9.1 条件指令

- v-if 是否渲染当前元素
- v-else
- v-else-if
- v-show 与 v-if 类似，只是元素始终会被渲染并保留在 DOM 中,只是简单切换元素的 CSS 属性 display 来显
- 示或隐藏

 

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>v-if的使用</title>
    <style>
        .con {
            width: 200px;
            height: 200px;
            background: red;
            margin-bottom: 10px;
        }
        .green {
            width: 200px;
            height: 200px;
            background: green;
            margin-bottom: 10px;
        }
    </style>
</head>
<body>
    <div id="app">
        <input type="button" value="下拉菜单1" @click="change()">
        <!-- v-if是创建和删除元素  v-if和v-else中间不要隔开否则报错 -->
        <div class="con" v-if="isok1"></div>
        <div class="green" v-else></div><br>
        <input type="button" value="下拉菜单2" @click="change2()">
        <!-- v-show显示和隐藏 -->
        <div class="con" v-show="isok2"></div>
    </div>
</body>
<script src="../node_modules/vue/dist/vue.js"></script>
<script>
    /*
        v-if:创建和删除元素
        v-show:显示和隐藏元素
        结论：如果需要频频的显示隐藏元素，建议v-show性能更好
    */
    //实例化app对象
    let app = new Vue({
        el: '#app',//el 放挂载对象,里面写的是选择器,不能挂载在html和body节点上
        data: {//放数据的地方
            isok1: false,
            isok2: false
        },
        methods: {//methods存放方法的地方
            change() {
                console.log('点击按钮1');
                console.log(this.isok1);//this指的是app实例
                this.isok1 = !this.isok1;
            },
            change2() {
                this.isok2 = !this.isok2;
            }
        }
    });
</script>
</html>
```

#### 3.9.2 v-if 与 v-show 比较

- 什么时候元素被渲染

- - v-if 如果在初始条件为假，则什么也不做，每当条件为真时，都会重新渲染条件元素
  - v-show 不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换

- 使用场景选择

- - v-if 有更高的切换开销，
  - v-show 有更高的初始渲染开销。
  - 因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行后条件很少改变，则使用 v-if 较好

### 3.10 列表渲染 v-for

#### 3.10.1 v-for 迭代数组

- 语法： v-for="(alias, index) in array"
- 说明： alias : 数组元素迭代的别名； index : 数组索引值从0开始(可选)

#### 3.10.2 v-for 迭代对象的属性

- 语法： v-for="(value, key, index) in Object"
- 说明： value : 每个对象的属性值; key : 属性名(可选); index : 索引值(可选) 。
- 可用 of 替代 in 作为分隔符

 

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>v-for</title>
</head>
<body>
    <div id="app">
        <h1>v-for渲染数据</h1>
        <!-- <ul v-for="(item, index) in list"> -->
        <!-- item指的是：遍历数组的每一项  index：数组的下标 -->
        <ul v-for="(item, index) of list">
            <li>{{ index + 1 }}.{{ item.name }} 工资：{{ item.salary }}</li>
            <!-- <li>刘强东 工资：3000</li>
            <li>小马哥 工资:4000</li> -->
        </ul>
        <!-- v-for遍历对象 item：键值  key：键名 index:第几对键值对 -->
        <ul v-for="(value, key, index) in goods">
            <p>{{index + 1}}.{{ key }}:{{ value }}</p>
            <!-- <p>2.tel:锤子手机</p>
            <p>3.price:1999</p> -->
        </ul>
    </div>
</body>
<script src="../node_modules/vue/dist/vue.js"></script>
<script>
    //实例化app对象
    let app = new Vue({
        el: '#app',//el 放挂载对象,里面写的是选择器,不能挂载在html和body节点上
        data: {//放数据的地方
            list: [
                {
                    name: '马云',
                    salary: 2000
                }, {
                    name: '刘强东',
                    salary: 3000
                }, {
                    name: '小马哥',
                    salary: 4000
                }
            ],
            goods: {
                name: '罗老师',
                tel: '锤子手机',
                price: '1999'
            }
        },
        methods: {
        }
    });
</script>
</html>
```

#### 3.10.3 key的作用

DOM操作次数

 

```
<body>
    <p id="num">0</p>
</body>
<script>
    console.time('start');
    //DOM操纵多少次? 3w -> 3次
    for (var i = 0; i < 10000; i++) {
        let ele = document.querySelector('#num');
        let num = ele.innerHTML;
        num++;
        ele.innerHTML = num;
    }
    console.timeEnd('start');
</script>
```

key的作用

 

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>key的使用</title>
</head>
<body>
    <div id="app">
        <input type="text" v-model="newdata">
        <input type="button" value="添加数据" @click="add">
        <ul>
            <li v-for="(item, index) in arr" :key="item"> <input type="checkbox"> {{ index }} - {{ item }}</li>
        </ul>
    </div>
</body>
<script src="../node_modules/vue/dist/vue.js"></script>
<script>
    /*
        v-for的时候如果遍历的数据是同类型：li
            * key 里面的值是唯一且稳定
    */
    //实例化app对象
    let vm = new Vue({
        el: '#app', //el 放挂载对象,里面写的是选择器,不能挂载在html和body节点上
        //普通属性
        data: { //放数据的地方
            newdata: '',
            // arr : [
            //     {
            //         id : 0,
            //         con : '濑尿虾'
            //     }
            // ]
            arr: ['濑尿虾', '大闸蟹', '生蚝', '小龙虾']
        },
        //方法
        methods: {
            add() {
                // console.log(this.newdata);
                this.arr.unshift(this.newdata);
            }
        },
        //计算属性
        computed: {
        },
        //监听器：方法
        watch: {
        }
    });
</script>
</html>
```

### 3.11 事件处理 v-on相关

3.11.1 事件处理方法

- 完整格式： v-on:事件名="函数名" 或 v-on:事件名="函数名(参数……)"
- 缩写格式： @事件名="函数名" 或 @事件名="函数名(参数……)" 注意： @ 后面没有冒号
- event ：函数中的默认形参，代表原生 DOM 事件
- 当调用的函数，有多个参数传入时，需要使用原生DOM事件，则通过 $event 作为实参传入
- 作用：用于监听 DOM 事件

 

```
<body>
<div id="app">
<h2>1. 事件处理方法</h2>
<button @click="say">Say {{msg}}</button>
<button @click="warn('hello', $event)">Warn</button>
</div>
<script src="./node_modules/vue/dist/vue.js"></script>
<script type="text/javascript">
    
```

#### 3.11.2 事件修饰符

- .stop 阻止单击事件继续传播 event.stopPropagation()
- .prevent 阻止事件默认行为 event.preventDefault()
- .once 点击事件将只会触发一次

#### 3.11.3 按键修饰符

- 格式： v-on:keyup.按键名 或 @keyup.按键名

- 常用按键名：

- - .enter
  - .tab
  - .delete (捕获“删除”和“退格”键)
  - .esc
  - .space
  - .up
  - .down
  - .left
  - .right

 

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>事件修饰符</title>
    <style>
        div {
            padding: 50px;
        }
        .father {
            background: hotpink;
        }
        .son {
            background: khaki;
        }
    </style>
</head>
<body>
    <div id="app">
        <!-- 注意：方法名后面的圆括号可以省略不写,但是如果你需要传参就必须写 -->
        <div class="father" @click="outer">
            <!-- 阻止事件冒泡 -->
            <div class="son" @click.stop="inner"></div>
        </div>
        <!-- 默认行为：a跳转 submit提交 选择文字 回车换行 右键菜单 -->
        <a href="http://www.baidu.com" @click.prevent="show">百度</a>
        <!-- 事件只能触发一次 -->
        <p @click.once="change">变了: {{ num }}</p>
        <!-- <input type="text" @keyup.13="submit"> -->
        <input type="text" @keyup.enter="submit">
    </div>
</body>
<script src="../node_modules/vue/dist/vue.js"></script>
<script>
    /*
        事件的修饰符:https://cn.vuejs.org/v2/guide/events.html#%E4%BA%8B%E4%BB%B6%E4%BF%AE%E9%A5%B0%E7%AC%A6
            * @click.stop 阻止冒泡
            * @click.prevent 阻止默认行为
            * @click.once 事件只能触发一次
            * @keyup.键值  键盘修饰符 可以写按键码，也可以写单词
    */
    //实例化app对象
    let vm = new Vue({
        el: '#app',//el 放挂载对象,里面写的是选择器,不能挂载在html和body节点上
        data: {//放数据的地方
            num: 0
        },
        methods: {
            outer() {
                alert('父节点')
            },
            inner() {
                alert('子节点');
            },
            show() {
                console.log('阻止了默认行为');
            },
            change() {
                this.num++;
            },
            submit() {
                console.log('回车提交了');
            }
        }
    });
</script>
</html>
```

### 3.12 表单数据双向绑定v-model

- 单向绑定：数据变，视图变；视图变（浏览器控制台上更新html），数据不变；
- 双向绑定：数据变，视图变；视图变（在输入框更新），数据变

#### 3.12.1 基础用法

- v-model 指令用于表单数据双向绑定，针对以下类型：

- - text 文本
  - textarea 多行文本
  - radio 单选按钮
  - checkbox 复选框
  - select 下拉框

 

```
<body>
  <div id="demo">
    <form action="#">
     姓名(文本)：<input type="text" >
      <br><br>
     性别(单选按钮)：
        <input name="sex" type="radio" value="1"/>男
        <input name="sex" type="radio" value="0"/>女
      <br><br>
     技能(多选框)：
        <input type="checkbox" name="skills" value="java">Java开发
        <input type="checkbox" name="skills" value="vue">Vue.js开发
        <input type="checkbox" name="skills" value="python">Python开发
      <br><br>
     城市(下拉框)：
      <select name="citys">
        <option value="bj">北京</option>
      </select>
      <br><br>
     说明(多行文本)：<textarea cols="30" rows="5"></textarea>
      <br><br>
      <button type="submit" >提交</button>
    </form>
  </div>
</body>
```

进行双向数据绑定后：

 

```
<body>
        <div id="demo">
                <!-- @submit.prevent 阻止事件的默认行为，当前阻止的是action行为 -->
            <form action="#" @submit.prevent="submitForm">
                姓名(文本)：<input type="text" v-model="name">
                <br><br>
    
                性别(单选按钮)：
                    <input name="sex" type="radio" value="1" v-model="sex"/>男
                    <input name="sex" type="radio" value="0" v-model="sex"/>女
                <br><br>
    
                技能(多选框)：
                    <input type="checkbox" name="skills" value="java" v-model="skill">Java开发
                    <input type="checkbox" name="skills" value="vue" v-model="skill">Vue.js开发
                    <input type="checkbox" name="skills" value="python" v-model="skill">Python开发
                <br><br>
    
                城市(下拉框)：
                <select name="citys" v-model="city">
                    <option v-for="c in citys" :value="c.code">
                        {{c.name}}
                    </option>
                </select>
                <br><br>
    
                说明(多行文本)：<textarea cols="30" rows="5" v-model="desc"></textarea>
                <br><br>
                <button type="submit" >提交</button>
            </form>
        </div>
        <script src="./node_modules/vue/dist/vue.js"></script>
        <script>
            new Vue({
                el: '#demo',
                data: {
                    name: '',
                    sex: '1',   //默认选中的是 男
                    skill: ['vue', 'python'], //默认选中 Vue.js开发 、Python开发
                    citys: [
                        {code: 'bj', name: '北京'},
                        {code: 'sh', name: '上海'},
                        {code: 'gz', name: '广州'}
                    ],
                    city: 'sh', // 默认选中的城市：上海
                    desc: ''
                },
                methods: {
                    submitForm: function () { //处理提交表单
                        //发送ajax异步处理
                        alert(this.name + ', ' + this.sex + ', ' + this.skills +  ', ' + this.city + ', ' + this.desc)
                    }
                }
            })
        </script>
    </body>
```

#### 3.12.2 v-model的修饰符

 

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>修饰符</title>
</head>
<body>
    <div id="app">
        <input type="text" v-model.trim="msg">
        <input type="text" v-model.number="num">
        <!-- v-model默认的效果:oninput事件效果,需求：失去焦点才触发 -->
        <input type="text" v-model.lazy="tex">
        <!-- 点击的时候传参 -->
        <input type="button" value="提交" @click="submit('666')">
        <!-- <input type="button" value="提交" @click="submit2"> -->
        <!-- <input type="button" value="提交" @click="submit3('666',$event)"> -->
    </div>
</body>
<script src="../node_modules/vue/dist/vue.js"></script>
<script>
    /*
        v-model修饰符：
            * v-model.trim 去除前后空格
            * v-model.number 把字符串转成数字类型
            * v-model.lazy 失去焦点才触发
    */
    //实例化app对象
    let vm = new Vue({
        el: '#app',//el 放挂载对象,里面写的是选择器,不能挂载在html和body节点上
        data: {//放数据的地方
            msg: '',
            num: 0,
            tex: ''
        },
        methods: {
            submit(data) {//形参
                // console.log(data);
                // let str = this.msg.trim();
                // console.log(str);
                console.log(this.msg);
            },
            submit2(ev) {
                console.log(ev.target.value);//value
                console.log(ev.target.tagName);//INPUT
            },
            submit3(data, ev) {//形参
                console.log(data);
                console.log(ev);
            }
        }
    });
</script>
</html>
```

### 3.13 指令v-pre

- v-pre 可以用来显示原始插入值标签 {{}} 。并跳过这个元素和它的子元素的编译过程。加快编译

 

```
<!-- v-pre作用：保留双花括号，当成文本显示不去编译 -->
<h1 v-pre>{{ 梦里花落知多少 }}</h1>
```

### 3.14 自定义指令的作用

除了内置指令外，Vue 也允许注册自定义指令。有的情况下，你仍然需要对普通 DOM 元素进行底层操作，这时候使用自定义指令更为方便

自定义指令文档： https://cn.vuejs.org/v2/guide/custom-directive.html 

#### 3.14.1 注册与使用自定义指令方式

- 注册全局指令:

 

```
// 指令名不要带 v-
Vue.directive('指令名', {
 // el 代表使用了此指令的那个 DOM 元素
 // binding 可获取使用了此指令的绑定值 等
 inserted: function (el, binding) {
   // 逻辑代码
}
})
```

- 注册局部指令：

 

```
directives : {
  '指令名' : { // 指令名不要带 v-
    inserted (el, binding) {
      // 逻辑代码
   }
 }
}
```

注意：注册时，指令名不要带 v-

- 使用指令：

- - 引用指令时，指令名前面加上 v-
  - 直接在元素上在使用即可 ： v-指令名='表达式'

- 需求：

- - 实现输出文本内容全部自动转为大写，字体为红色 ( 功能类型于 v-text , 但显示内容为大写)
  - 当页面加载时，该元素将获得焦点 (注意： autofocus 在移动版 Safari 上不工作)

 

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div id="app">
        <p v-upper-text="message">xxxxx</p>
        自动获取焦点：<input type="text" v-focus>
    </div>
    <div id="app2">
        <p v-upper-text="msg">xxxxx</p>
        
    </div>
    <script src="./node_modules/vue/dist/vue.js"></script>
    <script>
        // 注册全局自定义指令，可以在多个Vue管理的入口下使用该指令
        // 第一个参数为指令名，但是不要有v-开头
        Vue.directive('upper-text',{
            //一般对样式 的操作在bind中，bind函数只调用一次
            bind: function (el) {
                el.style.color = 'red'
            },
            //一般对js操作在inserted中，inserted也是只调用一次
            // el是当前指令作用的那个Dom元素，
            // binding用于获取使用了当前指令的绑定值(value)、表达式(expression)、指令名(name)等
            inserted: function (el, binding) {
                // 将所有字母文本内容转换为大写
                el.innerHTML = binding.value.toUpperCase()
            }
        })
        new Vue({
            el: '#app',
            data: {
                message: 'html5, 拼搏到无能为力'
            },
            //注册局部自定义指令：只能在当前Vue实例管理的入口 下引用这个指令
            directives: {
                'focus': { // 指令名，
                    bind: function () {
                    },
                    // 刷新页面自动获取焦点
                    inserted: function (el, binding) {
                        //被 v-focus 作用的那个元素在刷新页面后会自动 获取焦点
                        el.focus()
                    }
                }
            }
        })
        new Vue({
            el: '#app2',
            data: {
                msg: 'hello'
            }
        })
    </script>
</body>
</html>
```

## 4.经典实战项目-TodoMVC

### 4.1 项目介绍与演示

- TodoMVC 是一个非常经典的案例，功能非常丰富，并且针对多种不同技术分别都开发了此项目，比如React、AngularJS、JQuery等等。

- TodoMVC 案例官网：http://todomvc.com/

- 在官网首页右下角， 有 案例的模板下载 和 开发规范（需求文档），如下图：

- ![image-20220705212800751](C:/Users/10850/AppData/Roaming/Typora/typora-user-images/image-20220705212800751.png)

  

### 4.2 需求说明

#### 4.2.1 数据列表渲染

当任务列表（items ）没有数据时， #main 和 #footer 标识的标签应该被隐藏

任务涉及字段 : id 、任务名称（name ）、是否完成（ completed true 为已完成）

![img]()

#### 4.2.2 添加任务



\1. 在最上面的文本框中添加新的任务。

\2. 不允许添加非空数据。

\3. 按 Enter 键添加任务列表中，并清空文本框。

\4. 当加载页面后文本框自动获得焦点，在 input 上使用 autofocus 属性可获得

![img]()

#### 4.2.3 显示所有未完成任务数

\1. 左下角要显示未完成的任务数量。确保数字是由 <strong> 标签包装的。

\2. 还要将 item 单词多元化( 1 没有 s , 其他数字均有 s )： 0 items , 1 item , 2 items

示例： 2 items left

#### 4.2.4 切换所有任务状态

\1. 点击复选框 V 后，将所有任务状态标记为复选框相同的状态。

- 分析：

- - 复选框状态发生变化后，就迭代出每一个任务项, 再将复选框状态值赋给每个任务项即可 。
  - 为复选框绑定一个 计算属性 ，通过这个计算属性的 set 方法监听复选框更新后就更新任务项状态。

\2. 当 选中/取消 某个任务后，复选框 V 也应同步更新状态。

- 分析：

- - 当所有未完成任务数( remaining )为 0 时，表示所有任务都完成了，就可以选中复选框。
  - 在复选框绑定的 计算属性 的 get 方法中判断所有 remaining 是否为 0 ，从而绑定了 remaining ,
  - 当 remaining 发生变化后, 会自动更新复选框状态（为 0 复选框会自动选中，反之不选中）

1.点击复选框 V 后，将所有任务状态标记为复选框相同的状态（全选功能）

![img]()

2.当 选中/取消 了单个任务时，复选框 V 也应同步更新（反控制全选）

![img]()

#### 4.2.5 移除任务项

悬停在某个任务项上显示 X 移除按钮，可点击移除当前任务项

![img]()

#### 4.2.6 清除所有已完成任务

\1. 单击右下角 Clear completed 按钮时，移除所有已完成任务。

\2. 单击 Clear completed 按钮后，确保复选框清除了选中状态。

\3. 当列表中没有已完成的任务时，应该隐藏 Clear completed 按钮

#### 4.2.7 编辑任务项

\1. 双击 <label> （某个任务项）进入编辑状态（在 <li> 上通过 .editing 进行切换状态）。

\2. 进入编辑状态后输入框显示原内容，并获取编辑焦点。

\3. 输入状态按 Esc 取消编辑， editing 样式应该被移除。

\4. 按 Enter 键 或 失去焦点时 保存改变数据，移除 editing 样式

#### 4.2.8 路由状态切换（过滤不同状态数据）

根据点击的不同状态（ All / Active / Completed ），进行过滤出对应的任务，并进行样式的切换

#### 4.2.9 数据持久化

将所有任务项数据持久化到 localStorage 中,它主要是用于本地存储数据

### 4.3 实战

- 下载模板：git clone 仓库地址
- 安装依赖：npm i
- 引入vuejs
- 开始开发:编写app.js

index.html

 

```
<!doctype html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <title>Template • TodoMVC</title>
        <link rel="stylesheet" href="node_modules/todomvc-common/base.css">
        <link rel="stylesheet" href="node_modules/todomvc-app-css/index.css">
        <!-- CSS overrides - remove if you don't need it -->
        <link rel="stylesheet" href="css/app.css">
    </head>
    <body>
        <section class="todoapp" id="todoapp">
            <header class="header">
                <h1>todos</h1>
                <!-- 添加任务， -->
                <input 
                @keyup.enter="addItem" 
                class="new-todo" placeholder="What needs to be done?" v-app-focus>
            </header>
            <!-- This section should be hidden by default and shown when there are todos -->
            <!-- items.length 当值为0时，表示false，则不显示 -->
            <!-- template元素，页面渲染之后这个template元素就不会有
            ，需要使用 v-if 与 template搭配，如果使用v-show是不行的 -->
            <template v-if="items.length">
                <section class="main" >
                    <input v-model="toggleAll" id="toggle-all" class="toggle-all" type="checkbox">
                    <label for="toggle-all">Mark all as complete</label>
                    <ul class="todo-list">
                        <!-- These are here just to show the structure of the list items -->
                        <!-- List items should get the class `editing` when editing and `completed` 
                            when marked as completed -->
                        <li v-for="(item, index) in filterItems" :class="{completed: item.completed, editing: item === currentItem}">
                            <div class="view">
                                <input class="toggle" type="checkbox" v-model="item.completed">
                                <label @dblclick="toEdit(item)">{{item.content}}</label>
                                <button class="destroy" :value="item.id" @click="removeItem(index)"></button>
                            </div>
                            <input  v-todo-focus="item === currentItem" @keyup.enter="finishEdit(item, index, $event)" @blur="finishEdit(item, index, $event)"
                             @keyup.esc="cancelEdit" class="edit" :value="item.content">
                        </li>
                    </ul>
                </section>
                <!-- This footer should hidden by default and shown when there are todos -->
                <!-- items.length 当值为0时，表示false，则不显示 -->
                <footer class="footer"  >
                    <!-- This should be `0 items left` by default -->
                    <span class="todo-count"><strong>{{remaining}}</strong> item{{ remaining === 1 ? '' : 's' }} left</span>
                    <!-- Remove this if you don't implement routing -->
                    <ul class="filters">
                        <li>
                            <a :class="{selected: filterStatus === 'all'}" href="#/">All</a>
                        </li>
                        <li>
                            <a :class="{selected: filterStatus === 'active'}" href="#/active">Active</a>
                        </li>
                        <li>
                            <a :class="{selected: filterStatus === 'completed'}" href="#/completed">Completed</a>
                        </li>
                    </ul>
                    <!-- Hidden if no completed items are left ↓ -->
                    <!-- 当总任务数 大于 未完成任务数量 ，则显示下面按钮 -->
                    <button v-show="items.length > remaining"
                    @click="removeCompleted" class="clear-completed">Clear completed</button>
                </footer>
            </template>
        </section>
        <footer class="info">
            <p>Double-click to edit a todo</p>
            <!-- Remove the below line ↓ -->
            <p>Template by <a href="http://sindresorhus.com">Sindre Sorhus</a></p>
            <!-- Change this out with your name and url ↓ -->
            <p>Created by <a href="http://todomvc.com">you</a></p>
            <p>Part of <a href="http://todomvc.com">TodoMVC</a></p>
        </footer>
        <!-- Scripts here. Don't remove ↓ -->
        <script src="node_modules/vue/dist/vue.js"></script>
        <script src="node_modules/todomvc-common/base.js"></script>
        <script src="js/app.js"></script>
    </body>
</html>
```

app.js

 

```
(function (Vue) { //表示依赖了全局的 Vue, 其实不加也可以，只是更加明确点
   var STORAGE_KEY = 'items-vuejs';
    
   // 本地存储数据对象
   const itemStorage = {
      fetch: function () { // 获取本地数据
         return JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]');
      },
      save: function (items) { // 保存数据到本地
         localStorage.setItem(STORAGE_KEY, JSON.stringify(items));
      }
   }
   //初始化任务
   const items = []
    //注册全局指令
    //指令名不要加上v-, 在引用这个指令时才需要加上 v-
    Vue.directive('app-focus', {
        inserted (el, binding) {
            //聚集元素
            el.focus()
        }
    })
    
   var app = new Vue({
      el: '#todoapp',
       
      data: {
         //items,   // 对象属性简写，等价于items: items
         items: itemStorage.fetch(), //获取本地数据进行初始化
         currentItem: null, //上面不要少了逗号，接收当前点击的任务项
         filterStatus: 'all' // 上面不要少了逗号，接收变化的状态值
      },
       
      // 监听器
      watch: {
         // 如果 items 发生改变，这个函数就会运行
         items: {
            deep: true, // 发现对象内部值的变化, 要在选项参数中指定 deep: true。
            handler: function(newItems, oldItems) {
               //本地进行存储
               itemStorage.save(newItems)
            }
         }
      },
      //自定义局部指令
       directives : {
           'todo-focus': { //注意指令名称
               update (el, binding) {
                   //只有双击的那个元素才会获取焦点
                   if(binding.value) {
                       el.focus()
                   }
               }
           }
       },
      // 定义计算属性选项
      computed: {
         // 过滤出不同状态数据
         filterItems () {
            //this.filterStatus 作为条件，变化后过滤不同数据
            switch (this.filterStatus) {
               case "active": // 过滤出未完成的数据
                  return this.items.filter( item => !item.completed)
                  break
               case "completed": // 过滤出已完成的数据
                  return this.items.filter( item => item.completed)
                  break
               default: // 其他，返回所有数据
                  return this.items
            }
         },
         // 复选框计算属性
         toggleAll : {
            get () { //等价于 get : functon () {...}
               console.log(this.remaining)
               //2. 当 this.remaining 发生变化后，会触发该方法运行
               // 当所有未完成任务数为 0 , 表示全部完成, 则返回 true 让复选框选中
               //反之就 false 不选中
               return this.remaining === 0
            },
            set (newStatus) {
               // console.log(newStatus)
               //1. 当点击 checkbox 复选框后状态变化后，就会触发该方法运行,
               // 迭代出数组每个元素,把当前状态值赋给每个元素的 completed
               this.items.forEach((item) => {
                  item.completed = newStatus
               })
            }
         },
          // 过滤出所有未完成的任务项
          remaining () {
              /*
                return this.items.filter(function (item) {
                    return !item.completed
                }).length
                */
              //ES6 箭头函数
              return this.items.filter(item => !item.completed).length
          }
      }, // **注意** 后面不要少了逗号 ，
      methods: {
         //编辑完成
         finishEdit (item, index, event) {
            const content = event.target.value.trim();
            // 1. 如果为空, 则进行删除任务项
            if (!event.target.value.trim()){
               //重用 removeItem 函数进行删除
               this.removeItem(index)
               return
            }
            // 2. 添加数据到任务项中
            item.content = content
            // 3. 移除 .editing 样式
            this.currentItem = null
         },
         //取消编辑
         cancelEdit () {
            // 移除样式
            this.currentItem = null
         },
         // 进入编辑状态,当前点击的任务项item赋值currentItem，用于页面判断显示 .editing
         toEdit (item) {
            this.currentItem = item
         },
         //移除所有已完成任务项
         removeCompleted () {
            // 过滤出所有未完成的任务，重新赋值数组即可
            this.items = this.items.filter(item => !item.completed)
         },
         // 移除任务项
         removeItem (index) {
            // 移除索引为index的一条记录
            this.items.splice(index, 1)
         },
         //增加任务项
         addItem (event) { //对象属性函数简写，等价于addItem: function () {
            console.log('addItem', event.target.value)
            //1. 获取文本框输入的数据
            const content = event.target.value.trim()
            //2. 判断数据如果为空，则什么都不做
            if (!content.length) {
               return
            }
            //3.如果不为空，则添加到数组中
            //  生成id值
            const id = this.items.length + 1
            //  添加到数组中
            this.items.push({
               id, //等价于 id:id
               content,
               completed: false
            })
            //4. 清空文本框内容
            event.target.value = ''
         }
      }
   })
   //当路由 hash 值改变后会自动调用此函数
   window.onhashchange = function () {
      console.log('hash改变了' + window.location.hash)
      // 1.获取点击的路由 hash , 当截取的 hash 不为空返回截取的，为空时返回 'all'
      var hash = window.location.hash.substr(2) || 'all'
      // 2. 状态一旦改变，将 hash 赋值给 filterStatus
      //    当计算属性 filterItems  感知到 filterStatus 变化后，就会重新过滤
      //    当 filterItems 重新过滤出目标数据后，则自动同步更新到视图中
      app.filterStatus = hash
   }
   // 第一次访问页面时,调用一次让状态生效
   window.onhashchange()
})(Vue);
```

## 5.过滤器

### 5.1 什么是过滤器

- 过滤器对将要显示的文本，先进行特定格式化处理，然后再进行显示
- 注意：过滤器并没有改变原本的数据, 只是产生新的对应的数据

### 5.2 使用方式

#### 5.2.1 定义过滤器

全局过滤器：

 

```
Vue.filter(过滤器名称, function (value1[,value2,...] ) {
 // 数据处理逻辑
})
```

局部过滤器：在Vue实例中使用 filter 选项 ， 当前实例范围内可用

 

```
new Vue({
  filters: {
   过滤器名称: function (value1[,value2,...] ) {
    // 数据处理逻辑
  }
 }
})
```

#### 5.2.2 过滤器的使用

过滤器可以用在两个地方：双花括号 {{}} 和 v-bind 表达式

 

```
<!-- 在双花括号中 -->
<div>{{数据属性名称 | 过滤器名称}}</div>
<div>{{数据属性名称 | 过滤器名称(参数值)}}</div>
<!-- 在 `v-bind` 中 -->
<div v-bind:id="数据属性名称 | 过滤器名称"></div>
<div v-bind:id="数据属性名称 | 过滤器名称(参数值)"></div>
```

### 5.3 案例演示

需求：

\1. 实现过滤敏感字符，如当文本中有 tmd、sb 都将进行过滤掉

\2. 过滤器传入多个参数 ，实现求和运算

 

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>过滤器</title>
</head>
<body>
    <div id="app">
        <h3>测试过滤器单个参数</h3>
        <!-- 原始属性名|过滤器 -->
        <p>{{content | contentFilter}}</p>
        <input type="text" :value="content | contentFilter">
        <h3>测试过滤器多个参数</h3>
        <p>{{javaScore | add(vueScore, pythonScore)}}</p>
        <input type="text" :value="javaScore | add(vueScore, pythonScore)">
    </div>
    <script src="node_modules/vue/dist/vue.js"></script>
    <script>
        /*全局过滤器*/
        /* Vue.filter('contentFilter', function (value) {
            if(!value) {
                return ''
            }
            return value.toString().toUpperCase().replace('TMD', '***').replace('SB', '***')
        }) */
        new Vue({
            el: '#app',
            data: {
                content: '小伙子，tmd就是个SB',
                javaScore: 90,
                vueScore: 99,
                pythonScore: 89
            },
            filters: { //定义局部 过滤器
                contentFilter (value) { // contentFilter 过滤名, value
                    if(!value) {
                        return ''
                    }
                    return value.toString().toUpperCase().replace('TMD', '***').replace('SB', '***')
                },
                add (num1, num2, num3) {// add 过滤名， num1 其实就是引用时 | 左边的那个参数
                    return num1 + num2 + num3
                }
            }
        })
    </script>
</body>
</html>
```

## 6.自定义插件

### 6.1 插件的作用

\1. 插件通常会为 Vue 添加全局功能，一般是添加全局方法/全局指令/过滤器等

\2. Vue 插件有一个公开方法 install ，通过 install 方法给 Vue 添加全局功能

\3. 通过全局方法 Vue.use() 使用插件，它需要在你调用 new Vue() 启动应用之前完成

 

```
//plugins.js
(function(){
    //声明 MyPlugin 插件对象
    const MyPlugin = {}
    MyPlugin.install = function (Vue, options) {
        // 1. 添加全局方法或属性
        Vue.myGlobalMethod = function () {
          alert('MyPlugin.myGlobalMethod全局方法调用了')
        }
      
        // 2. 添加全局指令
        Vue.directive('my-directive', {
          inserted (el, binding) {
              el.innerHTML = "MyPlugin.my-directive指令被调用了" + binding.value
          }
        })
        // 3. 添加实例方法 new Vue()
        Vue.prototype.$myMethod = function (value) {
            alert('Vue 实例方法myMethod被调用了:' + value)
        }
      }
      //将插件添加 到 window对象
      window.MyPlugin = MyPlugin
})()
```

插件的应用：

 

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>自定义插件</title>
</head>
<body>
    <div id="app">
        <span v-my-directive="content">  </span>
    </div>
    <script src="node_modules/vue/dist/vue.js"></script>
    <!-- 要引入在vue.js下面 -->
    <script src="js/plugins.js"></script>
    <script>
        //1. 引入插件MyPlugin， 其实就是安装 MyPlugin 插件
        Vue.use(MyPlugin)
        var vm = new Vue({
            el: '#app',
            data: {
                content: 'hello'
            }
        })
        //调用插件中的实例 方法，注意是vm调用，不是Vue
        // 注意，不要少了 $
        vm.$myMethod('迪丽热巴被求婚')
        //调用全局方法，注意是 Vue进行调用，不是vm
        Vue.myGlobalMethod()
    </script>
</body>
</html>
```

## 7.Vue 组件化开发

### 7.1 组件的概念

- 组件（component） 是 Vue.js 最强大的功能之一。
- Vue 中的组件化开发就是把网页的重复代码抽取出来 ，封装成一个个可复用的视图组件，然后将这些视图组件拼接到一块就构成了一个完整的系统。这种方式非常灵活，可以极大的提高我们开发和维护的效率。
- 通常一套系统会以一棵嵌套的组件树的形式来组织：

![img]()

例如：项目可能会有头部、底部、页侧边栏、内容区等组件，每个组件又包含了其它的像导航链接、博文之类的组件

- 组件就是对局部视图的封装，每个组件包含了：

- - HTML 结构
  - CSS 样式
  - JavaScript 行为
  - data 数据
  - methods 行为

- 提高开发效率，增强可维护性，更好的去解决软件上的高耦合、低内聚、无重用的3大代码问题

- Vue 中的组件思想借鉴于 React

- 目前主流的前端框架：Angular、React 、Vue 都是组件化开发思想

### 7.2 组件的基本使用

- 为了能在模板中使用，这些组件必须先注册以便 Vue 能够识别。
- 有两种组件的注册类型：全局注册和局部注册

#### 7.2.1 全局注册

- 一般把网页中特殊的公共部分注册为全局组件：轮播图、分页、通用导航栏
- 全局注册之后，可以在任何新创建的 Vue 实例 (new Vue) 的模板中使用
- 简单格式：

 

```
Vue.component('组件名',{
  template: '定义组件模板'， 
  data: function(){ //data 选项在组件中必须是一个函数
    return {}
 }
  //其他选项：methods
})
```

说明：

- 组件名：

- - 可使用驼峰(camelCase)或者横线分隔(kebab-case)命名方式
  - 但 DOM 中只能使用横线分隔方式进行引用组件
  - 官方强烈推荐组件名字母全小写且必须包含一个连字符

- template：定义组件的视图模板

- data ：在组件中必须是一个函数（为了保证组件的独立性 和 可 复用性，data  是一个函数，组件实例化的时候这个函数将会被调用，返回一个对象，计算机会给这个对象分配一个内存地址，你实例化几次，就分配几个内存地址，他们的地址都不一样，所以每个组件中的数据不会相互干扰，改变其中一个组件的状态，其它组件不变。）https://www.cnblogs.com/yuerdong/p/11395410.html

#### 7.2.2 局部注册（子组件）

- 一般把一些非通用部分注册为局部组件，一般只适用于当前项目的
- 简单格式：

 

```
1. JS 对象来定义组件:
var ComponentA = { data: function(){}， template: '组件模板A'}
var ComponentA = { data: function(){}， template: '组件模板A'}
2. 在Vue实例中的 components 选项中引用组件：
new Vue({
  el: '#app',
  data: {},
  components: { // 组件选项
    'component-a': ComponentA // key：组件名，value: 引用的组件对象
    'component-b': ComponentB
}
})
```

代码实例：

 

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>全局注册</title>
</head>
<body>
    <div id="app">
        <!-- 引用组件时，组件名必须采用横线分隔符 -->
        <component-a></component-a>
        <component-a></component-a>
        <component-b></component-b>
    </div>
    <div id="app2">
        <component-a></component-a>
    </div>
    <script src="node_modules/vue/dist/vue.js"></script>
    <script>
        //全局注册组件
        /*
        组件名：
            1. 驼峰、横线分隔符命名方式
            2. 使用组件时，必须采用横线分隔符的方式进行引用
        组件可以理解为就是特殊的Vue实例 ， 不需要手动的实例化而已，它用于管理自已的模板
        */
        Vue.component('component-a', {
            // template选项指定组件的模板代码
            template: '<div><h1>头部组件---{{ name }}</h1></div>',
            data: function () { // 在组件中，data选项必须 是一个函数  
                return {
                    name: '全局组件'
                }
            }
        })
        // 定义局部 组件对象 
        const ComponentB = {
            template: '<div>这是 {{ name }}</div>',
            data: function () {
                return {
                    name: '局部组件'
                }
            }
        }
        new Vue({
            el: '#app',
            // 组件选项
            components: { 
                // key: value   key 组件名，value 就是组件对象
                'component-b': ComponentB
            }
        })
        new Vue({
            el: '#app2'
        }) 
    </script>
</body>
</html>
```

### 7.3 总结

- 组件是可复用的 Vue 实例，不需要手动实例化
- 与 new Vue 接收相同的选项，例如 data 、 computed 、 watch 、 methods 等
- 组件的 data 选项必须是一个函数

### 7.4 多个组件示例

![img]()

 

```
//Header.js 文件内容
Vue.component('app-header',{
  // template 选项指定此组件的模板代码
  template: '<div class="header"><h1>头部组件</h1></div>'
})
//Main.js 文件内容
Vue.component('app-main',{
  // template 选项指定此组件的模板代码
  template: '<div class="main"><ul><li>客户管理</li><li>帐单管理</li><li>供应商管理</li></ul><h3></h3>
</div>'
})
//Footer.js 文件内容
Vue.component('app-footer',{
  // template 选项指定此组件的模板代码
  template: '<div class="footer"><h1>底部组件</h1></div>'
})
```

- 可将组件注册抽取在一个一个的js文件中方便管理
- 页面引入组件 js 文件后, 进行使用

 

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div id="app">
        <app-header></app-header> 
        <app-main></app-main>
        <app-footer></app-footer>
    </div>
    <script src="node_modules/vue/dist/vue.js"></script>
    <script src="component/Header.js"></script>
    <script src="component/Main.js"></script>
    <script src="component/Footer.js"></script>
    <script>
        
        new Vue({
            el: '#app'
        }) 
    </script>
</body>
</html>
```

### 7.5 Bootstrap 首页组件化

![img]()

共拆分为6个组件，做成子组件

 

```
<body>
  <!-- 留一个组件的出口，此处要被子组件替换 -->
  <div id="app">
  </div>
  <script src="../node_modules/vue/dist/vue.js"></script>
  <!-- 注意要在 vue.js 下面引入 -->
  <script src="components/Navbar.js"></script>
  <script src="components/Leaf.js"></script>
  <!-- 注意：Dashboard.js 和 HomeList.js 要在 Home.js 前面引入 -->
  <script src="components/Home/Dashboard.js"></script>
  <script src="components/Home/HomeList.js"></script>
  <script src="components/Home/Home.js"></script>
  <script src="App.js"></script>
  <script>
    new Vue({
      el: '#app',
//Vue根实例中有template选项引用了组件后，然后会把template中的渲染结果替换掉 #app 标识的元素。
      template: '<app></app>',
      components: {
        App //等价于 App: App
     }
   })
  </script>
</body>
```

#### 7.5.1 组件化注意事项

- 组件可以理解为特殊的 Vue 实例，不需要手动实例化，管理自己的 template 模板
- 组件的 template 必须有且只有一个根节点
- 组件的 data 选项必须是函数，且函数体返回一个对象
- 组件与组件之间是相互独立的，可以配置自己的一些选项资源 data、methods、computed 等等
- 思想：组件自己管理自己，不影响别人

## 8.组件通信

### 8.1 Vue 父子组件间通信

#### 8.1.1 组件间通信方式

\1. props 父组件向子组件传递数据：父传子、子传父

\2. $emit 自定义事件：子传父

\3. slot 插槽分发内容：父传子

#### 8.1.2 组件间通信规则

\1. 不要在子组件中直接修改父组件传递的数据

\2. 数据初始化时，应当看初始化的数据是否用于多个组件中，如果需要被用于多个组件中，则初始化在父组件

中；如果只在一个组件中使用，那就初始化在这个要使用的组件中。

\3. 数据初始化在哪个组件, 更新数据的方法(函数)就应该定义在哪个组件(自己的数据自己管理)

#### 8.1.3 props 向子组件传递数据

- 声明组件对象中定义 props
- 在声明组件对象中使用 props 选项指定

 

```
const MyComponent = {
  template: '<div></div>',
  props: 此处值有以下3种方式,
  components: {
}
}
```

方式1：指定传递属性名，注意是 数组形式

 

```
props: ['id','name'，'salary', 'isPublished', 'commentIds', 'author', 'getEmp']
```

方式2：指定传递属性名和数据类型，注意是 对象形式

 

```
props: {
 id: Number,
 name: String,
 salary: Number,
 isPublished: Boolean,
 commentIds: Array,
 author: Object,
 getEmp: Function
}
```

方式3：指定属性名、数据类型、必要性、默认值

 

```
props: {
 name: {
  type: String,
  required: true,
  default: 'msg'
}
}
```

- 引用组件时动态赋值
- 在引用组件时，通过 v-bind 动态赋值

 

```
<my-component v-bind:id="2" :name="msg" :salary="9999" :is-published="true" :comment-ids="[1, 2]"
:author="{name: '李贤'}" :get-emp="getEmp" >
</my-component>
```

#### 8.1.4 案例

在父组件准备好数据，准备向子组件传输，实现组件通信

 

```
//父组件
let appMain = {
  template: `<div class="col-sm-9 col-sm-offset-3 col-md-10 col-md-offset-2 main">
    <!--右边上半区域-->
    //2、给子组件的节点，绑定自定义属性  :src :href :title :class :style
    <app-news :likes="hobbies"></app-news>
    <!--右边下半区域-->
    <app-list :empList="empList"></app-list>
  </div>`,
  data() {
    return {
        //1、在父组件准备数据
      hobbies: ['看书', '台球', '睡觉', '撸代码'],
      empList: [{
          id: 1,
          name: '马云',
          salary: 80001
        },
        {
          id: 2,
          name: '马化腾',
          salary: 80002
        },
        {
          id: 3,
          name: '罗永浩',
          salary: 80003
        },
        {
          id: 4,
          name: '李彦宏',
          salary: 80004
        }
      ]
    }
  },
  components: {
    appNews,
    appList
  }
}
```

子组件用props接收数据并使用

 

```
(function () {
  let tem = `<div><h1 class="page-header">兴趣爱好</h1>
  <div class="row placeholders">
    <div class="col-xs-6 col-sm-3 placeholder" v-for="(item, index) in likes">
      <img src="data:image/gif;base64,R0lGODlhAQABAIAAAHd3dwAAACH5BAAAAAAALAAAAAABAAEAAAICRAEAOw==" width="200"
        height="200" class="img-responsive" alt="Generic placeholder thumbnail">
      <h4>{{ item }}</h4>
      <span class="text-muted" @click="removeItem(index)"><a>删除</a></span>
    </div>
    
  </div></div>`;
  window.appTop = {
    //3.去到子组件，用props接收父组件的数据
    props: ['likes', 'delHobby'],
    template: tem,
    methods: {
      removeItem(index) {
        this.delHobby(index);
      }
    }
  }
})();
```

#### 8.1.5 传递数据注意

\1. props 只用于父组件向子组件传递数据

\2. 所有标签属性都会成为组件对象的属性, 模板页面可以直接引用

\3. 问题:

> a. 如果需要向非子后代传递数据，必须多层逐层传递
>
> b. 兄弟组件间也不能直接 props 通信, 必须借助父组件才可以

#### 8.1.6 自定义事件

作用：通过 自定义事件 来代替 props 传入函数形式

- 绑定自定义事件

- - 在父组件中定义事件监听函数，并引用子组件标签上 v-on 绑定事件监听

 

```
// 通过 v-on 绑定
// @自定义事件名=事件监听函数
// 在子组件 dashboard 中触发 delete_hobby 事件来调用 deleteHobby 函数
<dashboard @delete_hobby="deleteHobby"></dashboard>
```

- 触发监听事件函数执行

- - 在子组件中触发父组件的监听事件函数调用

 

```
// 子组件触发事件函数调用
// this.$emit(自定义事件名, data)
this.$emit('delete_emp', index)
```

- 自定义事件注意

- - 自定义事件只用于子组件向父组件发送消息(数据)
  - 隔代组件或兄弟组件间通信此种方式不合适

#### 8.1.7 slot 插槽

- 作用: 主要用于父组件向子组件传递 标签+数据 , （而上面prop和自定事件只是传递数据）
- 场景：一般是某个位置需要经常动态切换显示效果（如饿了么）

##### 子组件定义插槽

在子组件中定义插槽, 当父组件向指定插槽传递标签数据后, 插槽处就被渲染，否则插槽处不会被渲染

 

```
<div>
<!-- name属性值指定唯一插槽名，父组件通过此名指定标签数据-->
<slot name="aaa">不确定的标签结构 1</slot>
<div>组件确定的标签结构</div>
<slot name="bbb">不确定的标签结构 2</slot>
</div>
```

##### 父组件传递标签数据

 

```
<child>
<!--slot属性值对应子组件中的插槽的name属性值-->
<div slot="aaa">向 name=aaa 的插槽处插入此标签数据</div>
<div slot="bbb">向 name=bbb 的插槽处插入此标签数据</div>
</child>
```

#### 8.1.8 插槽注意事项

\1. 只能用于父组件向子组件传递 标签+数据

\2. 传递的插槽标签中的数据处理都需要定义所在父组件中

### 8.2 单文件组件

- 当前存在的问题

- - 全局定义 (Global definitions) 强制要求每个 component 中的命名不得重复
  - 字符串模板 (String templates) 缺乏语法高亮
  - 不支持 CSS (No CSS support) 意味着当 HTML 和 JavaScript 组件化时，CSS 明显被遗漏
  - 文件扩展名为 .vue 的 single-file components(单文件组件) 为以上所有问题提供了解决方法，并且还可以使用webpack 或 Browserify 等构建工具

- 单文件组件模板格式

 

```
<template>
// 组件的模块
</template>
<script>
// 组件的JS
</script>
<style>
// 组件的样式
</style>
```

有所了解，后期我们脚手架会用到 .vue文件

## 9. 生命周期和 Ajax 服务端通信

### 9.1 Vue 实例生命周期

#### 9.1.1 生命周期钩子函数

每个 Vue 实例在被创建时都要经过一系列的初始化过程

- 生命周期分为三大阶段：初始化显示、更新显示、销毁Vue实例

- - 初始化阶段的钩子函数：

  - - beforeCreate() 实例创建前：数据和模板均未获取到
    - created() 实例创建后: 最早可访问到 data 数据，但模板未获取到
    - beforeMount() 数据挂载前：模板已获取到，但是数据未挂载到模板上。
    - mounted() 数据挂载后： 数据已挂载到模板中

  - 更新阶段的钩子函数：

  - - beforeUpdate() 模板更新前：data 改变后，更新数据模板前调用
    - updated() 模板更新后：将 data 渲染到数据模板中

  - 销毁阶段的钩子函数：

  - - beforeDestroy() 实例销毁前
    - destroyed() 实例销毁后

#### 9.1.2 生命周期图示

![img]()

#### 9.1.3 代码演示

 

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>生命周期</title>
</head>
<body>
    <div id="app">
        <h1> {{ message }}</h1>
    </div>
    <script src="./node_modules/vue/dist/vue.js"></script>
    <script>
        const vm = new Vue({
            // el: '#app',
            data: {
                message: '生命周期钩子函数'
            },
            beforeCreate() {
                // Vue 实例创建前被调用，数据和模板均未获取到
                console.log('beforeCreate()', this.$el, this.$data)
            },
            created() {
                // Vue实例创建后，最早可以获取到 data数据的钩子，但是模板未获取到
                // 建议在这里面发送 ajax 异步请求
                console.log('created()', this.$el, this.$data)
            },
            beforeMount() {
                // 数据挂载之前，获取到了模板，但是数据还未挂载到模板上
                console.log('beforeMount()', this.$el, this.$data)
            },
            mounted() {
                // 数据已经挂载到模板中了
                console.log('mounted()', this.$el, this.$data)
            },
            beforeUpdate() {
                // 当data 数据更新之后，去更新模板中的数据前调用
                console.log('beforeUpdate()', this.$el.innerHTML, this.$data)
            },
            updated() {
                console.log('updated()', this.$el.innerHTML, this.$data)
            },
            beforeDestroy() {
                //销毁 Vue 实例之前调用
                // 收尾工作
                console.log('beforeDestroy()')
            },
            destroyed() {
                //销毁 Vue 实例之后调用
                console.log('destroyed()')
            },
        }).$mount('#app') // 如果实例中没有 el 选项，可使用 $mount()手动挂载 Dom
        // vm.$destroy() // 销毁Vue实例
    </script>
</body>
</html>
```

### 9.2 liveServer 服务端插件

在 VS Code 中安装 liveServer 服务端插件，用于 Ajax 接口调用（目前没有讲node。暂时用这个服务器允许vue代码，发起ajax请求）

![img]()

启动服务器：

- 方式1：任意打开一个 html 页面，在最下面可看到点击它可启动，默认端口5500 

- - ![img]()

- 方式2：在 html 页面单击右键 ，点击如下，访问页面

- - 注意：如果之前启动过服务器，则使用 方式2 启动html页面，不能使用 方式1 会端口占用
  - ![img]()

- 更改 liveServer 默认端口号（不冲突建议不要改，用默认端口5500即可）

- - 按 Ctrl + , 打开 Settings ，如下操作，打开 settings.json,
  - 再json文件中添加 "liveServer.settings.port": 8080,
  - ![img]()

### 9.3 Vue 中常用的 ajax 库

- 目前 Vue 官方没有内置任何 ajax 请求方法
- 在 vue 2+ 版本中，官方推荐使用非常棒的第三方 ajax 请求库：axios
- 使用：结合生命钩子获取数据，渲染数据
- vue-resource 不推荐，也是发送ajax
- fetch

#### 9.3.1 axios 的使用

- 参考文档:https://github.com/axios/axios/blob/master/README.md 或到npm官网看api：npmjs.com
- 安装

 

```
npm install axios -S
```

- 查看文档，准备一个json文件，发送请求获取数据

 

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>vue-resource</title>
</head>
<body>
    <div id="app">
        <h1>{{ repData }}</h1>
    </div>
    <script src="./node_modules/vue/dist/vue.js"></script>
    <script src="./node_modules/axios/dist/axios.js"></script>
    <script>
        new Vue({
            el: '#app',
            data: {
                repData: []
            },
            methods: {
                getdata() {
                    let p1 = axios.get('./db.json')
                        .then(response => { //function (response) {
                            // 成功的回调
                            console.log(response.data, this);
                            this.repData = response.data
                        })
                        .catch(error => { //function (error) {
                            // 失败的回调
                            console.log(error.message);
                        })
                        .finally(() => { //function () {
                            // 不论成功与失败都执行这里
                            console.log('finally');
                        });
                    console.log(p1);
                }
            },
            //发送异步请求
            created() {
                this.getdata();
            },
        })
    </script>
</body>
</html>
```

- async await的写法

 

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>vue-resource</title>
</head>
<body>
    <div id="app">
        <h1>{{ repData }}</h1>
    </div>
    <script src="./node_modules/vue/dist/vue.js"></script>
    <script src="./node_modules/axios/dist/axios.js"></script>
    <script>
        try {
            let sum = a + 6;
            console.log(sum);
        } catch {
            let c = 666;
            console.log(c);
        }
        new Vue({
            el: '#app',
            data: {
                repData: []
            },
            methods: {
                async getdata() {
                    try {
                        let p1 = await axios.get('./db.json')
                        console.log(p1.data);
                    } catch (error) {
                        console.log(error)
                    }
                }
            },
            //发送异步请求
            created() {
                this.getdata();
            },
        })
    </script>
</body>
</html>
```

#### 9.3.2 案例：

- 把boostrap部分数据，用ajax的方式请求渲染

![img]()

- 准备json数据

 

```
[
    {"id": 1, "name": "张三1", "salary": 9899},
    {"id": 2, "name": "张三2", "salary": 9999},
    {"id": 3, "name": "张三3", "salary": 9099},
    {"id": 4, "name": "张三4", "salary": 9199}
]
```

- 在需要渲染数据的页面，发送ajax请求

 

```
created() {
            axios.get('./emp.json')
                .then(response => { //function (response) {
                    console.log(response.data, this);
                    this.empList = response.data
                })
                .catch(error => { //function (error) {
                    alert(error.message)
                })
        }
```

## 10. Vue-Router 路由

### 10.1 什么是路由

Vue Router 是 Vue.js 官方的路由管理器。它和 Vue.js 的核心深度集成，让构建单页面应用变得非常简单。通过根据不同的请求路径，切换显示不同组件进行渲染页面。

### 10.2 基本路由使用

#### 10.2.1 安装路由

注意：要先安装 vue 模块

 

```
npm install vue-router
```

#### 10.2.2 引入 vue-router.js

 

```
<script src="./node_modules/vue/dist/vue.js"></script>
<script src="./node_modules/vue-router/dist/vue-router.js"></script>
```

#### 10.2.3 简单路由切换

 

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>路由管理</title>
</head>
<body>
    <div id="app">
        <div class="header">
            <h1>头部</h1>
        </div>
        <div class="left">
            <p>
                <ul>
                    <li><a href="#/foo">Go Foo</a></li>
                    <li><a href="#/bar">Go Bar</a></li>
                    <!-- 
                        官方推荐：
                        通过 router-link 标签会默认渲染出 a 标签来
                        其中 通过 to属性指定跳转链接，不用带上 #  号
                    -->
                    <li><router-link to="/foo">Go to Foo</router-link> </li>
                    <li><router-link to="/bar">Go to Bar</router-link></li>
                </ul>
            </p>
        </div>
        <div class="main">
            <!-- 路由出口 -->
            <!-- 路由匹配到的组件将渲染在这里 -->
            <router-view></router-view>
        </div>
    </div>
    <script src="./node_modules/vue/dist/vue.js"></script>
    <script src="./node_modules/vue-router//dist/vue-router.js"></script>
    <script>
        // 1. 定义组件
        const Foo = {
            template: '<div> 我是 Foo 组件</div>'
        }
        const Bar = {
            template: '<div> 我是 Bar 组件</div>'
        }
        // 2. 配置路由表： 当点击指定url时，显示对应的那个组件
        const router = new VueRouter({
            routes: [ // 注意单词，是routes 不是 router
                {path: '/foo', component: Foo},
                {path: '/bar', component: Bar}
            ]
        })
        
        // 3. 将路由器注入到实例中
        new Vue({
            el: '#app',
            router // router: router
        })
    </script>
</body>
</html>
```

### 10.3 路由案例实战

![img]()

#### 10.3.1 拷贝项目

- 把之前的bootstrap项目拷贝过来并重命名为bootstrap-ajax-router；
- 进入 bootstrap-ajax-router 命令行，通过 NPM 安装 vue-router

 

```
npm install vue-router
```

#### 10.3.2 准备两个子组件

- News 组件：News 模板位于： bootstarp\news.html
- About 组件

#### 10.3.3 配置路由表

新建router.js，配置好路由规则（路由表设置）

 

```
;(function () {
  window.router = new VueRouter({
    routes: [
     {path: '/', component: AppHome},
     {path: '/news', component: News},
     {path: '/about', component: About}
   ]
 })
})()
```

#### 10.3.4 注入路由到实例中

 

```
 new Vue({
  el: '#app',
  // Vue实例中的template选项中引用了组件后，会将这个组件的渲染结果替换掉 #app 标签的元素
  // template: '<app> </app>',
  template: '<app/>',
  router, // 引用路由配置
  components: {
   App // 等价于 App: App
 }
})
```

#### 10.3.5 配置路由渲染组件出口

 

```
<router-view>
   <h1 slot="dashboard" class="page-header">{{title}}</h1>
</router-view>
```

#### 10.3.6 修改跳转链接

 

```
; (function () {
window.AppLeaf = {
 template: `<div class="col-sm-3 col-md-2 sidebar">
   <ul class="nav nav-sidebar">
    <li class="active">
     <router-link to="/">首页</router-link>
    </li>
    <li>
     <router-link to="/news">新闻管理</router-link>
    </li>
    <li>
    <router-link to="/about">关于我们</router-link>
    </li>
   </ul>
  </div>`
}
})()
```

#### 10.3.7 引入 js 文件

在 index.html 中引入 vue-router.js 、News.js 、About.js、router.js ，注意引入的位置顺序

 

```
<body>
  <div id="app">
  </div>
  <script src="./node_modules/vue/dist/vue.js"></script>
  <!-- vue-router.js要引入在 vue.js 下方--->
  <script src="node_modules/vue-router/dist/vue-router.js"></script>
  <script src="./node_modules/axios/dist/axios.js"></script>
  <script src="components/AppNavbar.js"></script>
  <script src="components/AppLeaf.js"></script>
  <script src="components/home/Dashboard.js"></script>
  <!-- 注意引入的位置 -->
  <script src="components/home/Item.js"></script>
  <script src="components/home/HomeList.js"></script>
  <script src="components/home/AppHome.js"></script>
  <!-- 引入 News.js About.js router.js-->
  <script src="components/home/News.js"></script>
  <script src="components/home/About.js"></script>
  <script src="router.js"></script>
  <script src="App.js"></script>
  <script src="main.js"></script>
 </body>
```

#### 10.3.8 启动测试

用服务器打开查看效果

### 10.4 样式匹配-高亮显示导航

问题：你会发现不管点击左侧导航哪个链接，当前都是在第一个导航处显示高亮背景。按 F12 查看源代码，发现高亮样式在 <li class="active"> 标签上，应该点击哪个作用在哪个上面

参考路由 API 文档：https://router.vuejs.org/zh/api/

#### 10.4.1 tag

- <router-link> 默认渲染后生成 <a> 标签
- 可在 <router-link> 上使用 tag 属性，指定渲染后生成其他标签

 

```
<router-link to="/foo" tag="li">foo</router-link>
<!-- 渲染结果 -->
<li>foo</li>
```

#### 10.4.2 active-class

- <router-link> 渲染后生成标签上默认有 CSS 类名： router-link-active

- - 可在 <router-link> 上使用 active-class 属性，指定渲染后生成其他类名。
  - 可以通过路由的构造选项 linkActiveClass 来全局配置，不用在每个 <router-link> 使用 active-class 指定生成的类名 

#### 10.4.3 exact

- 默认情况 下，路由地址 / 、 /foo 、 /bar 都以 / 开头，它们都会去匹配 / 地址的 CSS 类名
- 可在 <router-link> 上使用 exact 属性开启 CSS 类名精确匹配

 

```
<!-- 这个链接只会在地址为 / 的时候被激活, -->
<router-link to="/" exact>
```

#### 10.4.4 实现高亮显示导航链接

重新修改代码：appmenu.js

 

```
; (function () {
 window.AppLeaf = {
  template: `<div class="col-sm-3 col-md-2 sidebar">
   <ul class="nav nav-sidebar">
   <!--
     1. 因为 .active 样式生成在 li 标签上而不 a 标签上,
      可以通过在 router-link 标签上使用 tag 属性, 让它渲染为 li 标签
     2. 可在每个 router-link 标签上使用 active-class 属性, 指定渲染后标签上面的 CSS 类名,
       或者在 VueRouter 实例(router.js)中使用选项 linkActiveClass 全局配置生成的 CSS 类名
     3. 通过上面,发现不管点击哪一个链接, / 路由(首页)一直有高亮背景,
      因为 /foo 都会去模糊匹配 / 和 /foo, 所以 / 一直有高亮
      可在router-link 标签上使用 exact 属性, 使用开启精确匹配
    -->
     <router-link to="/" tag="li" exact>
      <a>首页</a>
     </router-link>
     <router-link to="/news" tag="li" >
      <a>新闻管理</a>
     </router-link>
    <router-link to="/about" tag="li">
      <a>关于我们</a>
    </router-link>
 </ul>
  </div>`
}
})()
```

router.js

 

```
;(function () {
  window.router = new VueRouter({
    // 全局配置 router-link 标签生成的 CSS 类名
    linkActiveClass: 'active',
    routes: [
     {path: '/', component: AppHome},
     {path: '/news', component: News},
     {path: '/about', component: About}
   ]
 })
})()
```

![img]()

### 10.5  嵌套路由

#### 10.5.1 演示效果

![img]()

10.5.1 子路由组件

- Sport.js 体育栏目组件
- Tech.js 科技栏目组件

10.5.2 配置嵌套路由

 

```
{
path: '/news',
component: News,
children: [
    // 当匹配到 /news/sport 请求时，
    // 组件 Sport 会被渲染在 News 组件中的 <router-view> 中
    {
        path: '/news/sport',
        component: Sport
    },
    // 简写方式，等价于 /news/tech 路径，注意前面没有 / ,有 / 就是根目录了
    {
        path: 'tech',
        component: Tech
    },
    //点击新闻管理默认选中 新闻，
    // 就是/news后面没有子路径时, redirect 重定向到 体育
    {
        path: '',
        redirect: '/news/sport'}
    ]
]
```

#### 10.5. 3 路由跳转链接

 

```
<ul class="nav nav-pills">
<router-link to="/news/sport" tag="li">
<a >体育</a>
</router-link>
<router-link to="/news/tech" tag="li">
<a >科技</a>
</router-link>
</ul>
<!--定义路由出口-->
<router-view></router-view>
```

### 10.6 缓存路由组件与案例

#### 10.6.1 场景与作用

\1. 默认情况下，当路由组件被切换后组件实例会销毁，当切换回来时实例会重新创建。

\2. 如果可以缓存路由组件实例，切换后不用重新加载数据，可以提高用户体验。

#### 10.6.2 实现缓存路由组件

 可缓存渲染的路由组件实例

 

```
<keep-alive>
 <router-view></router-view>
</keep-alive>
```

### 10.7 路由组件传递数据

#### 10.7.1 传递数据步骤

\1. 路由配置

 

```
{
path: '/news/sport',
component: Sport,
children: [
{
path: '/news/sport/detail/:id', // :id 路径变量占位符
component: SportDetail
}
]
},
```

2、路由跳转路径

 

```
<!--
要动态拼接值, 则 to 属性值是 JS 表达式,
要写 JS 表达式, 则要使用 v-bind 方式绑定属性
注意 + 前面有单引号 ''
-->
<router-link :to="'/news/sport/detail/' + sport.id">
{{sport.title}}
</router-link>
```

3、在路由组件中读取请求参数

 

```
this.$route.params.id
```

### 10.8 编程式路由导航

#### 10.8.1 声明式与编程式路由

| 声明式导航                     | 编程式导航                                          |
| ------------------------------ | --------------------------------------------------- |
| 直接通过  标签href指定链接跳转 | 采用 js 代码链接跳转,如 location.href/window.open() |
| <router-link to="">            | router.push(...)                                    |

#### 10.8.2 编程式路由导航 API

利用Router实例（`this.$router`）的方法实现路由跳转

$router.push(location)相当于点击路由链接(后退1步,会返回当前路由界面)

 

```
this.$router.push('/home');//等同于：<router-link to="/home"></router>
    // 对象
    this.$router.push({ path: '/home' })
    this.$router.push({name:'Home'});//等同于：<router-link :to="{name:'Home'}"></router>
```

- $router.replace(location)

- - 类似于router.push()，唯一不同的是它不会向 history 添加新记录
  - 用新路由替换当前路由(后退1步,不可返回到当前路由界面)

- $router.go(n)/$router.back()/$router.forward()

- - 在history 记录中向前或者后退多少步，类似 window.history.go(n)

 

```
this.$router.push(path) 相当于点击路由链接(后退1步,会返回当前路由界面)
this.$router.replace(path) 用新路由替换当前路由(后退1步,不可返回到当前路由界面)
this.$router.back() 后退回上一个记录路由
this.$router.go(n) 参数 n 指定步数
this.$router.go(-1) 后退回上一个记录路由
this.$router.go(1) 向前进下一个记录路由
$router.forward() 前进一个路由
```

#### 10.8.3 路由传参

在组件中通过`this.$route`获取当前路由信息，包含传入的参数（params/query等）

##### 10.8.3.1 **跳转时传参**

- params

- - 通过`this.$route.params`获取，可通过动态路由、导航时传入等方式传入，刷新后参数不存在

 

```
// params传参，只支持name方式跳转
this.$router.push({name:'Goods',params:{id:123}})
// 动态路由传参
this.$router.push('/goods/123')
```

- query传参

- - 通过`this.$route.query`获取，可通过路由导航时传入，刷新后参数依然存在

#### 

 

```
this.$router.push('/goods?id=123')
this.$router.push({path:'/goods',query:{id:123}})
```

- 动态路由传入

 

```
//路由组件
    const User = {
        template:`<div>{{$route.params.id}}</div>`
    }
    //路由配置
    routes:[{
        path:'/news/detail/:id',
        component:User
    }]
```

注意：当路由从 /news/detail/1导航到 /news/detail/2，组件实例会被复用（因为两个路由都渲染同个组件sport，比起销毁再创建，复用则显得更加高效）因此不会执行组件的生命周期钩子函数。解决方案：

利用watch 监测 $route 对象变化beforeRouteUpdate路由守卫（2.2+）

 

```
const User = {
        watch: {
            '$route' (to, from) {
            // 对路由变化作出响应...
            }
        },
        beforeRouteUpdate (to, from, next) {
            //to:目标路由
            //from:当前路由
            //一定要调用next()方法才可进入目标路由
        }
    }
```

#### 

#### 10.8.4 科技栏目案例

- 配置路由

 

```
{
  path: 'tech',
  component: Tech,
  children: [
   {  // 点击 栏目标题列表，查看详情
      path: '/news/tech/detail/:id',
      component: TechDetail
   }
 ]
},
```

- Tech 绑定点击事件和渲染出口
- 测试JS函数中push/replace处理路由跳转 和 back/ge后退

 

```
;(function () {
  const template = `
    <div>
<ul >
        <li v-for="(tech, index) in techArr" :key="tech.id">
<span> {{tech.title}} </span>
<button class="btn btn-default btn-xs" @click="pushTech(tech.id)">查看(Push)</button>&nbsp;
<button class="btn btn-default btn-xs" @click="replaceTech(tech.id)">查看(replace)</button>
</li>
      </ul>
      <button @click="$router.back()">后退</button>
<!--详情-->
<router-view></router-view>
</div>
  `
  window.Tech = {
      data () {
      return {
        techArr: []
     }
   },
    //钩子异步加载数据
    created() {
      this.getTechArr()
   },
    methods: {
      pushTech (id) {
        // push 可后退回来
        // 是 $router , 有字母 r 路由器。用的反单引号 ` ` 拼接
        this.$router.push(`/news/tech/detail/${id}`)
     },
      replaceTech (id) {
        // replace 不可后退回来
        this.$router.replace(`/news/tech/detail/${id}`)
     },
      getTechArr() {
        axios.get('./db/tech.json')
       .then(response => {//function (response) {
          console.log(response.data, this);
          this.techArr = response.data
       })
       .catch(error => {//function (error) {
          alert(error.message)
       })
     }
   },
    template
 }
})()
```

- TechDetail 详情组件

 

```
;(function () {
  const template = `
    <div class="jumbotron">
      <h2>{{techDetail.title}}</h2>
      <p>{{techDetail.content}}</p>
    </div>
  `
  window.TechDetail = {
    template,
    data () {
      return {
        techDetail: {}
     }
        },
    created () {
      // 不要少 this
     this.getTechById()
   },
    methods: {
      // 通过 id 获取详情
      getTechById() {
        // 1. 获取路径变量 id 值
        const id = this.$route.params.id-0
        // 2. 获取所有数据
        axios.get('./db/tech.json')
       .then(response => {
          const techArr = response.data
          // 3. 查找指定id的数据
          this.techDetail = techArr.find(tech => {
            return tech.id === id
         })
       })
       .catch(error => {
          alert(error.message)
       })
     }
   },
   
    watch: { // 是对象，不是函数噢
      // 使用 watch 监听 $route 路由的变化,获取 ID 值
      '$route': function () {
        this.getTechById()
     }
   }
 }
})()
```

## 11. Webpack和Vue-Loader打包资源

另外有课件

## 12. Vue-CLI 3.x 脚手架构建项目

### 12.1 什么是 Vue-CLI

\1. Vue-CLI 是 Vue 官方提供的, 用来搭建项目脚手架的工具，它是 Vue.js 开发的标准工具，它已经集成了webpack , 内置好了很多常用配置, 使得我们在使用 Vue 开发项目时更加的标准化。

\2. 作用： 通过 Vue-CLI 下载模板项目。

\3. 官方文档：https://cli.vuejs.org/zh/

\4. github站点：https://github.com/vuejs/vue-cli

### 12.2 Vue CLI 安装

- Vue CLI 环境要求：
- 需要 Node.js 8.9+ (推荐 8.11.0+)

\1. 全局安装 Vue-CLI （只需要安装一次，以后就可以不断使用来创建项目）

 

```
npm install -g @vue/cli
```

\2. 安装成功后，在命令行可以使用 vue 命令，比如查看当前安装的版本：

 

```
vue --version
# 或者 大写V
vue -V
```

- 如果执行上面后，命令行提示 'vue' 不是内部或外部命令

![img]()

- 解决方法：配置环境变量

> > > \1. 查看全局安装目录 npm root -g 并复制这个路径
>
> > > \2. 右键计算机，属性—》高级系统设置—》环境变量，将 vue.cmd 的路径加入环境变量，点击“确定”
> > >
> > > \3. 重启命令行窗口， vue -V 执行正常

![img]()

### 12.3 基于 Vue-CLI 创建项目

运行 vue create 命令来创建一个新项目

 

```
vue create 项目名称
```

#### 12.3.1 创建默认项目

1.进入vuecli目录下新建项目

 

```
vue create vue-cli-demo1
```

![img]()

- 会提示选择默认（ default ）还是手动（ Manually ）,默认的配置非常适合快速创建一个新项目的原型，而手动设置则提供了更多的选项。
- 这里选择 default ，等待一会（如有提示等待，一直回车执行下去就行）

![img]()

\2. 进入： cd vue-cli-demo1

\3. 运行： npm run serve

\4. 访问：http://localhost:8080/

![img]()

#### 12.3.2 创建自定义项目

\1. 在 vuecli 目录下打开命令行窗口，输入以下命令进行新建项目，项目名是 vue-cli-demo2

 

```
vue create vue-cli-demo2
```

![img]()

选择 Manually select features 手动选择自定义配置进行创建项目

\2. 如下图，根据项目需求, 选择哪些配置选项

- 注意：按 空格键 是选中或取消， a 键是全选

![img]()

 

```
Bable, （常用）解决兼容性问题,支持 ES6 的代码转译成浏览器能识别的代码
TypeScript, 是一种给 JavaScript 添加特性的语言扩展,增加了很多功能,微软开发的
Progressive Web App (PWA) Support, 渐进式的Web应用程序支持
Router, （常用）是 vue-router 路由。
Vuex, 是Vue.js应用程序的状态管理模式+库 （常用）。
CSS Pre-processors, （常用）支持 CSS 预处理器, Sass/Less预处理器。
Linter / Formatter, （常用）支持代码风格检查和格式化。
Unit Testing, 支持单元测试。
E2E Testing, 支持 E2E 测试
```

- 选择对应配置选项

![img]()

- 选择后按回车键, 会提示: 是否使用 history 模式的路由, 按回车即可

![img]()

- 选择CSS预处理器

![img]()

- 选择ESLint + Prettier

![img]()

- 选择语法检查方式，这里我选择: 保存就检测

![img]()

- 会提示: 把babel,postcss,eslint这些配置放哪，我选择: 放在独立文件中, 然后回来即可

![img]()

- 会提示: 是否将当前项目设置的配置保存为预配置, 方便后面创建项目时, 继续使用这套配置?
- 按回车保存即可, 下次创建项目时,就会多有一个选项(vue-cli-demo2)

![img]()

如果要删除 preset（预配置），在 C:\Users\你的用户名 目录下的 .vuerc 文件中删除

- 确定后，等待下载依赖模块

![img]()

- 运行

 

```
cd vue-cli-demo2
npm run serve
```

![img]()

### 12.4 CLI 服务命令

参考 ：https://cli.vuejs.org/zh/guide/cli-service.html

- CLI 服务 ( @vue/cli-service ) 是一个开发环境依赖。针对绝大部分应用优化过的内部的 webpack 配置；
- 在一个 Vue CLI 项目中， @vue/cli-service 模块安装了一个名为 vue-cli-service 的命令。
- 在 package.json 中的 scripts 指定 vue-cli-service 相关命令

 

```
"scripts": {
  "serve": "vue-cli-service serve --open",
  "build": "vue-cli-service build",
  "lint": "vue-cli-service lint"
}
```

- serve 启动一个开发环境服务器(基于 webpack-dev-server)

- - 修改组件代码后，会自动热模块替换

- build 会项目根目录下自动创建一个 dist/ 目录，打包后的文件都在其中

- - 打包后的 js 会自动生成后缀为 .js 和 .map 的文件

  - - js文件: 是经过压缩加密的，如果运行时报错，输出的错误信息无法准确定位到哪里的代码报错。
    - map文件：文件比较大， 代码未加密，可以准确的输出是哪一行哪一列有错

- lint 使用 Eslint 进行检查并修复代码的规范

- - 比如: 将 main.js 中的格式多加个空格 ，执行 npm run lint 后它会自动的去除多余空格

- 执行命令方式：

 

```
npm run serve
npm run build
npm run lint
```

### 12.5 脚手架项目结构

 

```
|-- node_modules: 存放下载依赖的文件夹
|-- public: 存放不会变动静态的文件，它与src/assets的区别在于，public目录中的文件不被webpack打包处理，会原
样拷贝到dist目录下
    |-- index.html: 主页面文件
    |-- favicon.ico: 在浏览器上显示的图标
|-- src: 源码文件夹
    |-- assets: 存放组件中的静态资源
    |-- components: 存放一些公共组件
    |-- views: 存放所有的路由组件
    |-- router: 存放路由配置信息
    |-- store: 存放公共状态 vuex
    |-- App.vue: 应用根主组件
    |-- main.js: 应用入口 js
|-- .browserslistrc: 指定了项目可兼容的目标浏览器范围, 对应是package.json 的 browserslist选项
|-- .eslintrc.js: eslint相关配置
|-- .gitignore: git 版本管制忽略的配置
|-- babel.config.js: babel 的配置,即ES6语法编译配置
|-- package-lock.json: 用于记录当前状态下实际安装的各个包的具体来源和版本号等, 保证其他人在 npm install 项
目时大家的依赖能保证一致.
|-- package.json: 项目基本信息,包依赖配置信息等
|-- postcss.config.js: postcss一种对css编译的工具，类似babel对js的处理
|-- README.md: 项目描述说明的 readme 文件
```

关于 browsers 的配置如下：

![img]()

### 12.6 自定义配置

在项目根目录下创建 vue.config.js 文件。

vue.config.js 基本常用配置（其他的具体看文档: https://cli.vuejs.org/zh/config/#vue-config-js） 

 

```
module.exports = {
 devServer: {
  port: 8001, // 端口号，如果端口被占用，会自动提升 1
  open: true, // 启动服务自动打开浏览器
  https: false, // 协议
  host: "localhost", // 主机名，也可以 127.0.0.1 或 做真机测试时候 0.0.0.0
},
 lintOnSave: false, // 默认 true, 警告仅仅会被输出到命令行，且不会使得编译失败。
 outputDir: "dist2", // 默认是 dist ,存放打包文件的目录,
 assetsDir: "assets", // 存放生成的静态资源 (js、css、img、fonts) 的 (相对于 outputDir) 目录
 indexPath: "out/index.html", // 默认 index.html, 指定生成的 index.html 的输出路径 (相对于 outputDir)。
 productionSourceMap: false, // 打包时, 不生成 .map 文件, 加快打包构建
}
```

### 12.7 Eslint 插件

#### 12.7.1 什么是 ESLint

ESLint 是一个语法规则和代码风格的检查工具，可以 用来保证写出语法正确、风格统一的代码。

如果我们开启了 Eslint , 也就意味着要接受它 非常苛刻的语法检查，包括空格不能少些或多些，必须单引不能双引，语句后不可以写 分号等等，这些规则其实是可以设置的。

我们作为前端的初学者，最好先关闭这种校验，否则会浪费很多精力在语法的规范性上。如果以后做真正的企业级开发，建议开启。Vue-Cli 中默认是开启的

#### 12.7.2 Eslint 配置介绍

在项目根目录下的 package.json 文件中有 eslintConfig 选项中配置, 或者 .eslintrc.js 配置

 

```
"eslintConfig": { // eslint配置
  "root": true, // 用来告诉eslint找当前配置文件
  "env": { // 指定你想启用的环境，下面的配置指定为node环境
   "node": true
 },
  "extends": [
   "plugin:vue/essential", // 格式化代码插件
   "eslint:recommended" // 启用推荐规则
 ],
  "rules": { //新增自定义规则
   "semi":[1,'never'], // 禁止用分号
 "indent": [2, 2] // 缩进采用2个空格
}, 
  "parserOptions": {
   "parser": "babel-eslint" //用来指定eslint解析器的
 }
},
```

#### 12.7.3 自定义语法规则

参考：

Vue Github文档：https://github.com/vuejs/vue-docs-zh-cn/blob/master/vue-cli-plugin-eslint/README.md

Eslint 官方规则：https://cn.eslint.org/docs/rules/

\1. 语法规则写在 package.json 或 .eslintrc.js 文件中 rules 选项中, 语法如下。

 

```
rules: {
  "规则名": [错误等级值, 规则配置]
}
```

规则值：

 

```
"off" 或者 0  // 关闭规则
"warn" 或者 1  // 打开规则，作为警告（信息打印黄色字体）
"error" 或者 2  // 打开规则，作为错误（信息打印红色字体）
```

\2. 常用 Eslint 规则配置参数

 

```
"no-alert": 0,//禁止使用alert confirm prompt
"no-array-constructor": 2,//禁止使用数组构造器
"no-bitwise": 0,//禁止使用按位运算符
"no-caller": 1,//禁止使用arguments.caller或arguments.callee
"no-catch-shadow": 2,//禁止catch子句参数与外部作用域变量同名
"no-class-assign": 2,//禁止给类赋值
"no-cond-assign": 2,//禁止在条件表达式中使用赋值语句
"no-console": 2,//禁止使用console
"no-const-assign": 2,//禁止修改const声明的变量
"no-constant-condition": 2,//禁止在条件中使用常量表达式 if(true) if(1)
"no-continue": 0,//禁止使用continue
"no-control-regex": 2,//禁止在正则表达式中使用控制字符
"no-debugger": 2,//禁止使用debugger
"no-delete-var": 2,//不能对var声明的变量使用delete操作符
"no-div-regex": 1,//不能使用看起来像除法的正则表达式/=foo/
"no-dupe-keys": 2,//在创建对象字面量时不允许键重复 {a:1,a:1}
"no-dupe-args": 2,//函数参数不能重复
"no-duplicate-case": 2,//switch中的case标签不能重复
"no-else-return": 2,//如果if语句里面有return,后面不能跟else语句
"no-empty": 2,//块语句中的内容不能为空
"no-empty-character-class": 2,//正则表达式中的[]内容不能为空
"no-empty-label": 2,//禁止使用空label
"no-eq-null": 2,//禁止对null使用==或!=运算符
"no-eval": 1,//禁止使用eval
"no-ex-assign": 2,//禁止给catch语句中的异常参数赋值
"no-extend-native": 2,//禁止扩展native对象
"no-extra-bind": 2,//禁止不必要的函数绑定
"no-extra-boolean-cast": 2,//禁止不必要的bool转换
"no-extra-parens": 2,//禁止非必要的括号
"no-extra-semi": 2,//禁止多余的冒号
"no-fallthrough": 1,//禁止switch穿透
"no-floating-decimal": 2,//禁止省略浮点数中的0 .5 3.
"no-func-assign": 2,//禁止重复的函数声明
"no-implicit-coercion": 1,//禁止隐式转换
"no-implied-eval": 2,//禁止使用隐式eval
"no-inline-comments": 0,//禁止行内备注
"no-inner-declarations": [2, "functions"],//禁止在块语句中使用声明（变量或函数）
"no-invalid-regexp": 2,//禁止无效的正则表达式
"no-invalid-this": 2,//禁止无效的this，只能用在构造器，类，对象字面量
"no-irregular-whitespace": 2,//不能有不规则的空格
"no-iterator": 2,//禁止使用__iterator__ 属性
"no-label-var": 2,//label名不能与var声明的变量名相同
"no-labels": 2,//禁止标签声明
"no-lone-blocks": 2,//禁止不必要的嵌套块
"no-lonely-if": 2,//禁止else语句内只有if语句
"no-loop-func": 1,//禁止在循环中使用函数（如果没有引用外部变量不形成闭包就可以）
"no-mixed-requires": [0, false],//声明时不能混用声明类型
"no-mixed-spaces-and-tabs": [2, false],//禁止混用tab和空格
"linebreak-style": [0, "windows"],//换行风格
"no-multi-spaces": 1,//不能用多余的空格
"no-multi-str": 2,//字符串不能用\换行
"no-multiple-empty-lines": [1, {"max": 2}],//空行最多不能超过2行
"no-native-reassign": 2,//不能重写native对象
"no-negated-in-lhs": 2,//in 操作符的左边不能有!
"no-nested-ternary": 0,//禁止使用嵌套的三目运算
"no-new": 1,//禁止在使用new构造一个实例后不赋值
"no-new-func": 1,//禁止使用new Function
"no-new-object": 2,//禁止使用new Object()
"no-new-require": 2,//禁止使用new require
"no-new-wrappers": 2,//禁止使用new创建包装实例，new String new Boolean new Number
"no-obj-calls": 2,//不能调用内置的全局对象，比如Math() JSON()
"no-octal": 2,//禁止使用八进制数字
"no-octal-escape": 2,//禁止使用八进制转义序列
"no-param-reassign": 2,//禁止给参数重新赋值
"no-path-concat": 0,//node中不能使用__dirname或__filename做路径拼接
"no-plusplus": 0,//禁止使用++，--
"no-process-env": 0,//禁止使用process.env
"no-process-exit": 0,//禁止使用process.exit()
"no-proto": 2,//禁止使用__proto__属性
"no-redeclare": 2,//禁止重复声明变量
"no-regex-spaces": 2,//禁止在正则表达式字面量中使用多个空格 /foo bar/
"no-restricted-modules": 0,//如果禁用了指定模块，使用就会报错
"no-return-assign": 1,//return 语句中不能有赋值表达式
"no-script-url": 0,//禁止使用javascript:void(0)
"no-self-compare": 2,//不能比较自身
"no-sequences": 0,//禁止使用逗号运算符
"no-shadow": 2,//外部作用域中的变量不能与它所包含的作用域中的变量或参数同名
"no-shadow-restricted-names": 2,//严格模式中规定的限制标识符不能作为声明时的变量名使用
"no-spaced-func": 2,//函数调用时 函数名与()之间不能有空格
"no-sparse-arrays": 2,//禁止稀疏数组， [1,,2]
"no-sync": 0,//nodejs 禁止同步方法
"no-ternary": 0,//禁止使用三目运算符
"no-trailing-spaces": 1,//一行结束后面不要有空格
"no-this-before-super": 0,//在调用super()之前不能使用this或super
"no-throw-literal": 2,//禁止抛出字面量错误 throw "error";
"no-undef": 1,//不能有未定义的变量
"no-undef-init": 2,//变量初始化时不能直接给它赋值为undefined
"no-undefined": 2,//不能使用undefined
"no-unexpected-multiline": 2,//避免多行表达式
"no-underscore-dangle": 1,//标识符不能以_开头或结尾
"no-unneeded-ternary": 2,//禁止不必要的嵌套 var isYes = answer === 1 ? true : false;
"no-unreachable": 2,//不能有无法执行的代码
"no-unused-expressions": 2,//禁止无用的表达式
"no-unused-vars": [2, {"vars": "all", "args": "after-used"}],//不能有声明后未被使用的变量或参数
"no-use-before-define": 2,//未定义前不能使用
"no-useless-call": 2,//禁止不必要的call和apply
"no-void": 2,//禁用void操作符
"no-var": 0,//禁用var，用let和const代替
"no-warning-comments": [1, { "terms": ["todo", "fixme", "xxx"], "location": "start" }],//不能有警告备注
"no-with": 2,//禁用with
"array-bracket-spacing": [2, "never"],//是否允许非空数组里面有多余的空格
"arrow-parens": 0,//箭头函数用小括号括起来
"arrow-spacing": 0,//=>的前/后括号
"accessor-pairs": 0,//在对象中使用getter/setter
"block-scoped-var": 0,//块语句中使用var
"brace-style": [1, "1tbs"],//大括号风格
"callback-return": 1,//避免多次调用回调什么的
"camelcase": 2,//强制驼峰法命名
"comma-dangle": [2, "never"],//对象字面量项尾不能有逗号
"comma-spacing": 0,//逗号前后的空格
"comma-style": [2, "last"],//逗号风格，换行时在行首还是行尾
"complexity": [0, 11],//循环复杂度
"computed-property-spacing": [0, "never"],//是否允许计算后的键名什么的
"consistent-return": 0,//return 后面是否允许省略
"consistent-this": [2, "that"],//this别名
"constructor-super": 0,//非派生类不能调用super，派生类必须调用super
"curly": [2, "all"],//必须使用 if(){} 中的{}
"default-case": 2,//switch语句最后必须有default
"dot-location": 0,//对象访问符的位置，换行的时候在行首还是行尾
"dot-notation": [0, { "allowKeywords": true }],//避免不必要的方括号
"eol-last": 0,//文件以单一的换行符结束
"eqeqeq": 2,//必须使用全等
"func-names": 0,//函数表达式必须有名字
"func-style": [0, "declaration"],//函数风格，规定只能使用函数声明/函数表达式
"generator-star-spacing": 0,//生成器函数*的前后空格
"guard-for-in": 0,//for in循环要用if语句过滤
"handle-callback-err": 0,//nodejs 处理错误
"id-length": 0,//变量名长度
"indent": [2, 4],//缩进风格
"init-declarations": 0,//声明时必须赋初值
"key-spacing": [0, { "beforeColon": false, "afterColon": true }],//对象字面量中冒号的前后空格
"lines-around-comment": 0,//行前/行后备注
"max-depth": [0, 4],//嵌套块深度
"max-len": [0, 80, 4],//字符串最大长度
"max-nested-callbacks": [0, 2],//回调嵌套深度
"max-params": [0, 3],//函数最多只能有3个参数
"max-statements": [0, 10],//函数内最多有几个声明
"new-cap": 2,//函数名首行大写必须使用new方式调用，首行小写必须用不带new方式调用
"new-parens": 2,//new时必须加小括号
"newline-after-var": 2,//变量声明后是否需要空一行
"object-curly-spacing": [0, "never"],//大括号内是否允许不必要的空格
"object-shorthand": 0,//强制对象字面量缩写语法
"one-var": 1,//连续声明
"operator-assignment": [0, "always"],//赋值运算符 += -=什么的
"operator-linebreak": [2, "after"],//换行时运算符在行尾还是行首
"padded-blocks": 0,//块语句内行首行尾是否要空行
"prefer-const": 0,//首选const
"prefer-spread": 0,//首选展开运算
"prefer-reflect": 0,//首选Reflect的方法
"quotes": [1, "single"],//引号类型 `` "" ''
"quote-props":[2, "always"],//对象字面量中的属性名是否强制双引号
"radix": 2,//parseInt必须指定第二个参数
"id-match": 0,//命名检测
"require-yield": 0,//生成器函数必须有yield
"semi": [2, "always"],//语句强制分号结尾
"semi-spacing": [0, {"before": false, "after": true}],//分号前后空格
"sort-vars": 0,//变量声明时排序
"space-after-keywords": [0, "always"],//关键字后面是否要空一格
"space-before-blocks": [0, "always"],//不以新行开始的块{前面要不要有空格
"space-before-function-paren": [0, "always"],//函数定义时括号前面要不要有空格
"space-in-parens": [0, "never"],//小括号里面要不要有空格
"space-infix-ops": 0,//中缀操作符周围要不要有空格
"space-return-throw-case": 2,//return throw case后面要不要加空格
"space-unary-ops": [0, { "words": true, "nonwords": false }],//一元运算符的前/后要不要加空格
"spaced-comment": 0,//注释风格要不要有空格什么的
"strict": 2,//使用严格模式
"use-isnan": 2,//禁止比较时使用NaN，只能用isNaN()
"valid-jsdoc": 0,//jsdoc规则
"valid-typeof": 2,//必须使用合法的typeof的值
"vars-on-top": 2,//var必须放在作用域顶部
"wrap-iife": [2, "inside"],//立即执行函数表达式的小括号风格
"wrap-regex": 0,//正则表达式字面量用小括号包起来
"yoda": [2, "never"]//禁止尤达条件
```

#### 12.7.4 按规则自动修复代码

执行下面命令会根据 Eslint 定义的语法规则进行检查，并按语法规则进行自动修复项目中不合规的代码

 

```
npm run lint
```

### 12.8 创建 .vue 模板代码

#### 12.8.1 安装插件识别vue文件

插件库中搜索 Vetur 安装

![img](C:/Users/10850/Desktop/index_files/3b473ffd-993f-4473-aa2a-94fcbf688caa.jpg)

#### 12.8.2 配置模板代码片段

\1. 依次选择 “ File（文件） -> preperences（首选项） -> User Snippets(用户代码片段)”，此时，会弹出一个搜索框，输入 vue ， 如下：

![img](C:/Users/10850/Desktop/index_files/d4e7f4c5-c766-4ad0-8729-bbca27791ee0.jpg)

\2. 选择 vue 后，VS Code 会自动打开一个名字为 vue.json 的文件，复制以下内容到这个文件中

 

```
{
    "Print to console": {
        "prefix": "vue",
        "body": [
            "<template>",
            "  <div>",
            "    $0",
            "  </div>",
            "</template>",
            "",
            "<script>",
            "export default {",
            "  data () {",
            "    return {",
            "    }",
            "  },",
            "",
            "  components: {},",
            "",
            "  methods: {}",
            "}",
            "</script>",
            "",
            "<style scoped>",
            "</style>"
        ],
        "description": "Log output to console"
    }
}
```

保存后关闭这个文件。

说明： $0 生成代码后光标的位置 ; prefix 表示生成对应预设代码的命令

#### 12.8.3 测试

新建一个名为 Demo.vue 的文件，输入 vue ，此时编辑区会有一系列提示，选择 Log output to console 这一项

![img](C:/Users/10850/Desktop/index_files/b2fbf7db-4cce-4c72-865c-893a766aaf93.jpg)

![img](C:/Users/10850/Desktop/index_files/ba3c546f-14a4-48c1-9a4e-c178f1096425.jpg)

### 12.9 部署项目

#### 12.9.1 打包项目

进入到打包的项目文件夹下,运行以下命令

 

```
npm run build
```

打包成功后，项目文件夹下会多出 dist 目录，那么你可以将 dist 目录里构建的内容部署到任何静态文件服务器中。这里其实是调用了 webpack 来实现打包的

#### 12.9.2 部署项目

方式1：使用 Node.js 静态文件服务器 (serve)

 

```
# 安装 server
npm install -g serve
# 本地部署 dist 目录下的项目
serve -s dist
```

方式2：使用动态 web 服务器 (tomcat)

\1. 在项目的根目录下新建 vue.config.js 文件（是根目录，不是src目录）

 

```
module.exports = {
    publicPath = '/vue-demo/'
}
```

\2. 配置打包信息。比如，项目名字为 vue-demo

\3. 进行重新打包，因为 dist 目录下的文件会使用项目名称 /vue-demo/

 

```
npm run build
```

\4. dist 目录拷贝到 tomcat\webapps 目录下，然后将 dist 目录名 修改为 项目名称 vue-demo

![img](C:/Users/10850/Desktop/index_files/731024f6-17d9-4bab-9445-bb37dc747b8f.jpg)

\5. 打开 tomcat/bin/startup.bat 启动服务器（注意之前启动的 8080 端口项目要先停止）

\6. 访问：http://localhost:8080/vue-demo/
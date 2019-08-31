---
title: JavaWeb_Vue框架学习
date: 2019-08-31 11:54:16
tags:
- JavaWeb
- 前端
categories: JavaWeb
---

## 指令学习

### v-cloak

* 使用 v-cloak 能够解决 插值表达式闪烁问题

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>v-cloak</title>
    <script src="./js/vue.js"></script>
    <style>
        [v-cloak]{
            display: none;
        }
    </style>
</head>
<body>
    <div id="app">
        <p v-cloak>{{msg}}</p>
    </div>


    <script>
        var vm = new Vue({
            el:'#app',
            data:{
                msg:'123'
            }
        })
    </script>
</body>
</html>
```



### v-text

* 以下代码与{{ msg }}效果相同，都是显示msg的值

* ```html
  <p v-text="msg"></p>
  ```

* v-text与插值表达式的区别

  1. 默认v-text没有闪烁问题
  2. 插值表达式只会替换自己的占位符，v-text会覆盖元素中原本的内容



### v-html

* v-text以及插值表达式会把内容以纯文本形式输出，v-html则可以将内容以html形式输出

* ```html
  <p v-html="msg"></p>
  ```



### v-bind

* 用于在属性中绑定Vue中定义的值
* 缩写：“ : ”

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>v-bind</title>
    <script src="./js/vue.js"></script>
</head>
<body>
    <input id="app" type="button" value="按钮" v-bind:title="mytitle">
    <input id="app" type="button" value="按钮" :title="mytitle">
    <!--title的值会直接显示为mytitle-->
    <input id="app" type="button" value="按钮" title="mytitle">
</body>
<script>
    var vm = new Vue({
        el: "#app",
        data:{
            mytitle:"自定义title"
        }
    })
</script>
</html>
```



### v-on

* 与v-bind用法类似，不过适用于绑定事件
* 缩写：@

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>v-on</title>
    <script src="./js/vue.js"></script>
</head>
<body>

    <input id="app" type="button" value="按钮" v-on:click="show">
    <input id="app" type="button" value="按钮" @click="show">

    <script>
        var vm = new Vue({
            el: '#app',
            //用于定义方法的属性
            methods: { //定义show方法
                show:function () {
                    alert("hello");
                }
            }
        })
    </script>
</body>
</html>
```



### v-for

1. 迭代数组

   ```html
   <p v-for="item in list">{{ item }}</p>
   <p v-for="(item, i) in list">索引值---{{ i }} 每一项---{{ item }}</p>
   <p v-for="user in emp_list">{{ user.id }}---{{ user.name }}</p>
   ```

2. 迭代对象中的属性

   ```html
   <p v-for="(val, key, i)">值:{{ val }}---键:{{ key }}---索引:{{ i }}</p>
   ```

3. 迭代数字

   ```html
   <!-- i的值从1开始 -->
   <p v-for="i in 10">这是第{{ i }}个p标签</p>
   ```

**注意**：在2.2.0以后版本后，在组件中使用v-for指令，key时必须的

* key属性只能使用number或者string
* 需要使用v-bind来绑定key的值

```html
<p v-for="item in list" :key="item">{{ item }}</p>
```



### v-if和v-show

```html
<!-- flag为data中的值 -->
<h3 v-if="flag">这是用v-if控制的元素</h3>
<h3 v-show="flag">这是用v-show控制的元素</h3>
```

* 区别：
  * **v-if**：每次都会重新删除或创建元素，有较高的切换性能消耗；**如果元素涉及到频繁的切换，最好不要使用v-if**
  * **v-show**：每次不会重新进行DOM的删除和创建操作，只是切换了元素的display:none样式，有较高的初始渲染消耗；**如果元素可能永远也不会被显示出来被用户看到，则最好选择v-if而不是v-show**



### 例子：跑马灯效果制作

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>跑马灯</title>
    <script src="./js/vue.js"></script>
</head>
<body>
    <div id="app">
        <h4>{{ msg }}</h4>
        <input type="button" value="开始" @click="start">
        <input type="button" value="停止" @click="stop">
    </div>

    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                msg: "跑马灯效果展示",
                intervalId:null
            },
            methods:{
                //开始跑马灯效果
                /*
                    1. 拿到msg字符串，调用字符串的substring来进行字符串的截取操作，
                    把第一个字符截取出来，放到最后一个位置
                    2. 自动截取需要一个计时器
                */
                start(){
                    //解决this的指向问题
                    var _this = this;
                    if(this.intervalId == null) {
                        this.intervalId = setInterval(function () {
                            //需要this来访问data上的数据
                            //获取到头的第一个字符
                            var start = _this.msg.substring(0, 1);
                            //获取到后面的所有字符
                            var end = _this.msg.substring(1);
                            //重新拼接
                            _this.msg = end + start;
                        }, 400);
                    }
                },
                stop(){
                    clearInterval(this.intervalId);
                    this.intervalId = null;
                }
            }
        })
    </script>
</body>
</html>
```

* 事件修饰符
  * .stop：阻止冒泡（组织子元素事件触发父元素事件）
  * .prevent：阻止默认事件(如阻止a标签的默认(跳转)事件)
  * .capture：添加事件侦听器使用事件捕获模式(从外到里执行执行事件)
  * .self：只当事件在该元素本身(比如不是子元素)触发时触发回调
    - .self只会阻止自己身上冒泡行为的发生，并不会阻止真正的冒泡行为
  * .once：事件只触发一次

```html
<input type="button" value="按钮" @click.stop="btnHandler">
```



### v-model和双向数据绑定

* 对比：v-bind只能实现数据单向绑定，从M自动绑定到V

* ```html
  <body>
      <div id="app">
          <h4>{{ msg }}</h4>
  
          <input type="text" v-model="msg">
      </div>
  
      <script>
          var vm = new Vue({
              el: "#app",
              data: {
                  msg: "展示"
              },
              methods: {}
          })
      </script>
  
  </body>
  ```

* 在网页内改变文本框的值，&lt;h4>标签内的值相应改变

* 即实现了表单元素 和 Model中数据的双向绑定

#### 简易计算器

```html
<body>

    <div id="app">
        <input type="text" v-model="n1">

        <select v-model="operator">
            <option value="+">+</option>
            <option value="-">-</option>
            <option value="*">*</option>
            <option value="/">/</option>
        </select>

        <input type="text" v-model="n2">

        <span>=</span>
        <input type="text" v-model="result">
        <input type="button" @click="cal" value="计算">
    </div>
    <script>
        var vm = new Vue({
            el: "#app",
            data: {
                n1:0,
                n2:0,
                result:0,
                operator:'+'
            },
            methods:{
                cal() {
                    /*switch (this.operator) {
                        case '+':
                            this.result = parseInt(this.n1) + parseInt(this.n2)
                            break;
                        case '-':
                            this.result = parseInt(this.n1) - parseInt(this.n2)
                            break;
                        case '*':
                            this.result = parseInt(this.n1) * parseInt(this.n2)
                            break;
                        case '/':
                            this.result = parseInt(this.n1) / parseInt(this.n2)
                            break;*/
                    
                    //投机取巧的方式
                    var calculate = "parseInt(this.n1)" + this.operator + "parseInt(this.n2)";
                    this.result = eval(calculate);
                    }
                }
            
        });
    </script>
</body>
```



***

## 样式

* 使用**class**样式

  1. 数组

     ```html
     <h1 :class="['thin', 'italic']">数组</h1>
     ```

  2. 三元表达式

     ```html
     <!-- isactive为data中定义的值 -->
     <h1 :class="['red', 'thin', isactive?'active':'']">三元表达式</h1>
     ```

  3. 数组中嵌套对象

     ```html
     <h1 :class="['red', 'thin', {'active':isactive}]">数组中嵌套对象</h1>
     ```

  4. 直接使用对象

     ```html
     <h1 :class="{red:true, italic:true, active:true, thin:true}">直接使用对象</h1>
     ```

  **注意**：以上定义的类

  ```css
  .red{
              color:red;
          }
          .thin{
              font-weight: 200;
          }
          .italic{
              font-style: italic;
          }
          .active{
              letter-spacing: 0.5em;
          }
  ```





* 使用**内联**样式：

  1. 直接在元素上通过:style的形式，书写样式对象

     ```html
     <h1 :style="{color:'red', 'font-size':200}">用于展示的h1</h1>
     ```

  2. 将样式对象，定义到data中，并直接引用到:style中

     * 在data上定义样式

       ```javascript
       data: {
                       h1StyleObj:{
                           color:'red',
                           'font-size':'40px',
                           'font-weight':200
                       },
                   },
       ```

     * 将样式对象应用到元素中

       ```html
       <h1 :style="h1StyleObj">用于展示的h1</h1>
       ```

  3. 在:style中通过数组，引用多个data上的样式对象

     * 在data上定义样式

       ```javascript
       data: {
                       h1StyleObj1:{
                           color:'red',
                           'font-size':'40px',
                           'font-weight':200
                       },
                       h1StyleObj2:{
                           fontStyle:'italic'
                       }
                   },
       ```

     * 将样式对象应用到元素中

       ```html
       <h1 :style="[h1StyleObj1, h1StyleObj2]">用于展示的h1</h1>
       ```

     

  


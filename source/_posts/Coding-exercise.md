---
title: Coding exercise
date: 2024-12-04 17:44:39
tags:
---

## FEd1 表单类型
描述: 在form标签里，依次写出以下类型的输入框。 1. 类型为密码，默认值为"nowcoder" 2. 类型为复选框，且状态为已勾选 代码：
```js
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <style>
       /* 填写样式 */
    </style>
</head>

<body>
   <form>
    <!-- 补全代码 -->
    <input type="password" value="nowcoder">
    <input type="checkbox" checked>
    
</form>
    <script type="text/javascript">
        // 填写JavaScript
        
    </script>
</body>

</html>
```

## FEd2 表格结构
描述: 请写出具有表格标题为"nowcoder"的2行3列表格结构。 代码：
```js
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <style>
       /* 填写样式 */
    </style>
</head>

<body>
   <table>
    <caption>nowcoder</caption>
    <tr>
        <td></td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td></td>
        <td></td>
    </tr>   
    
</table>
    <script type="text/javascript">
        // 填写JavaScript
        
    </script>
</body>

</html>
```

## FEd3 图像标签属性
描述: 请写出具有标题属性和代替文本属性的图片标签。 代码：
```js
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <style>
      //填写JavaScript
    </style>
</head>

<body>
     <img src="图片地址" title="标题" alt="代替文本"/>
    <script type="text/javascript">
        // 填写JavaScript
        
    </script>
</body>

</html>
```

## FEd4 新窗口打开文档
描述: 请写出可以在新窗口打开文档的a标签。 代码
```js
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <style>
       /* 填写样式 */
    </style>
</head>

<body>
   <a href="#" target="_blank"></a>
     <script type="text/javascript">
        /* 填写JavaScript */
        
    </script>
</body>

</html>
```

## FEd5 自定义列表
描述: 请写出列表项为"nowcoder"且列表项内容也为"nowcoder"的自定义列表。 代码：
```js
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <style>
       /* 填写样式 */
    </style>
</head>

<body>
  <dl>
    <dt>nowcoder</dt>
    <dd>nowcoder</dd>
  </dl> 
    <script type="text/javascript">
        /* 填写JavaScript */
        
    </script>
</body>

</html>
```

## FEd6 语义化标签
描述: 请使用语义化标签创建头部标签且包含导航标签。 注意：只需在html模块填写标签结构，有且仅有一个头部标签和一个导航标签。 代码：
```js
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <style>
       /* 填写样式 */
    </style>
</head>

<body>
   <header>
    <nav>
    </nav>
   </header>
    <script type="text/javascript">
        /* 填写JavaScript */
        
    </script>
</body>

</html>
```

## FEd9 视频媒体标签属性
描述： 请写出具有当视频的媒体数据加载期间发生错误时执行某个方法事件的视频媒体标签。 代码：
```js
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <style>
       /* 填写样式 */
    </style>
</head>

<body>
   <video src="" onerror="function()" controls>加载失败</video>
    <script type="text/javascript">
        /* 填写JavaScript */
        
    </script>
</body>

</html>
```

## FEd10 CSS选择器——标签、类、ID选择器
描述: 请将html模块中字体内容是"红色"的字体颜色设置为"rgb(255, 0, 0)"，"绿色"设置为"rgb(0, 128, 0)"，"黑色"设置为"rgb(0, 0, 0)"，且字体大小都为20px。 代码：
```js
<html>
    <head>
        <meta charset=utf-8>
        <style type="text/css">
            /*补全代码*/
        div{
            font-size:20px;
            color:rgb(255,0,0);
        }
        .green{
            color:rgb(0,128,0)
        }
        #black{
            color:rgb(0,0,0)
        }
        </style>
    </head>
    <body>
        <div>红色</div>
        <div class='green'>绿色</div>
        <div id='black'>黑色</div>
    </body>
</html>
```

## FEd11 CSS选择器——伪类选择器
描述: 请将html模块中ul列表的第2个li标签和第4个li标签的背景颜色设置成"rgb(255, 0, 0)"。 代码：
```js
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <style>
        ul li:nth-child(2),ul li:nth-child(4){
            background-color:rgb(255,0,0)
        }
       /* 填写样式 */
    </style>
</head>

<body>
  <ul>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
</ul> 
    <script type="text/javascript">
        /* 填写JavaScript */
        
    </script>
</body>

</html>
```

## FEd12 CSS选择器——伪元素
描述: 请给html模块的div元素加一个后伪元素，且后伪元素的宽度和高度都是20px，背景颜色为"rgb(255, 0, 0)"。 代码：
```js
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <style>
        div::after{
            content:"";
            width:20px;
            height:20px;
            background-color:rgb(255,0,0);
            display:block
        }
       /* 填写样式 */
    </style>
</head>

<body>
  <div></div> 
    <script type="text/javascript">
        /* 填写JavaScript */
        
    </script>
</body>

</html>
```
div::after表示在div元素后插入内容。 在元素上，content 的初始值为 ‘normal’。在:before和:after上，如果指定了content 的初始值为 ‘normal’，则计算为 ‘none’ 。content 的值设置为 ‘none’ 不会生成伪元素。所以:before和:after才需要指定一个看似无意义的 content: ""; 来初始化content的值。 题目规定了宽高，为了使宽高设置有效又必须显式定义该伪元素为块级元素，也就是语句 display:block

## FEd13 按要求写一个圆
描述： 请将html模块的div元素设置为一个半径是50px的圆，且边框为1px的黑色实线。 要求： 1. 圆角属性仅设置一个值 2. 圆角属性单位请使用px 注意：由于圆角属性设置广泛且都可以实现题目效果，所以请按照要求规范书写。 代码：
```js
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <style>
        div{
            height:100px;
            width:100px;
            border-radius: 50px;
            border:1px solid black;
        }
       /* 填写样式 */
    </style>
</head>

<body>
  <div></div> 
    <script type="text/javascript">
        /* 填写JavaScript */
        
    </script>
</body>

</html>
```

## FEd14 设置盒子宽高
描述: 请将html模块类为"box"的div元素宽度和高度都设置为100px，且内间距为20px、外间距为10px。 代码：
```js
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <style>
        .box{
            width:100px;
            height:100px;
            margin:10px;
            padding:20px;
        }
       /* 填写样式 */
    </style>
</head>

<body>
    <div class="box">
</div>
    <script type="text/javascript">
        // 填写JavaScript
        
    </script>
</body>

</html>
```

## FEd15 浮动和清除浮动
描述: 请将类为"left"的div元素和类为"right"的div元素在同一行上向左浮动，且清除类为"wrap"的父级div元素内部的浮动。 代码：
```js
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <style>
    .clearfix::after {
    /*补全代码*/
    content: "";
    display: block;
    height: 0;
    clear:both;
    visibility: hidden
}
 .clearfix{
        *zoom: 1;
    }

 .left {
    width: 100px;
    height: 100px;
    /*补全代码*/
    float:left; 
}
 .right {
    width: 100px;
    height: 100px;
    /*补全代码*/
    float:left;
}
    </style>
</head>

<body>
  <div class='wrap clearfix'>
	<div class='left'></div>
	<div class='right'></div>
</div>
    <script type="text/javascript">
        /* 填写JavaScript */
        
    </script>
</body>

</html>
```
```js
清除浮动方案  
1.额外标签法
额外标签法，也成为隔墙法，是W3C推荐的做法。
该方法会在浮动元素末尾添加一个空的标签，例如<div style="clear:both"></div>或其它标签。
注意：这个空标签必须是块级元素。

优点：通俗易懂，方便简单
缺点：添加许多无意义的标签，结构化差
 .clear{
  clear: both;
}
<body>
    <div class="box">
        <div class="wangcai">旺财</div>
        <div class="xiaoqiang">小强</div>
        <div class="clear"></div>
    </div>
    <div class="footer">
        footer
    </div>
</body>

2.父级添加overflow
可以给浮动元素父级添加overflow属性，将其属性值设置为hidden、auto或scroll。

优点：代码简介
缺点：无法显示溢出的部分或不是预期效果。
.box{
    background-color: chocolate;
    overflow: hidden;
}
<div class="box">
    <div class="wangcai">旺财</div>
    <div class="xiaoqiang">小强</div>
</div>
<div class="footer">
    footer
</div>

3.:after伪元素法
:after方式其实是额外标签发的升级版，也就是给父元素添加如下属性：
.box::after{
    content: "";
    height: 0;
    clear: both;
    display: block;
    visibility: hidden;
}
/* IE 6、7适配 */
.box {
    *zoom: 1;
}
<div class="box">
    <div class="wangcai">旺财</div>
    <div class="xiaoqiang">小强</div>
</div>
<div class="footer">
    footer
</div>
优点：没有增加标签，结构更简单
缺点：需适配低版本浏览器
代表网站：百度、网易等

4.双伪元素清除浮动
通过:before添加了前置伪元素，代码如下：
.box::before,.box::after{
    content: "";
    display: table;
}
.box::after{
    clear: both;
}
/* IE 6、7适配 */
.box{
    zoom: 1;
}

<div class="box">
    <div class="wangcai">1</div>
    <div class="xiaoming">2</div>
</div>
<div class="footer">
    footer
</div>
优点：代码更简洁
缺点：需适配低版本浏览器
代表网站：小米、腾讯等
```
清除浮动前提条件: 父级没有高度、 子盒子开启动、 影响后续布局排版


## FEd16 固定定位
描述: 请将html模块类为"box"的div元素固定在视口的左上角。 代码：
```js
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <style>
       .box {
    width: 100px;
    height: 100px;
    position: fixed;
    top:0;
    left:0;
    /*补全代码*/
    
}
    </style>
</head>

<body>
  <div class='box'></div> 
    <script type="text/javascript">
        /* 填写JavaScript */
        
    </script>
</body>

</html>
```

## FEd19 圣诞树
描述: 圣诞节来啦！请用CSS给你的朋友们制作一颗圣诞树吧~这颗圣诞树描述起来是这样的： 1. "topbranch"是圣诞树的上枝叶，该上枝叶仅通过边框属性、左浮动、左外边距即可实现。边框的属性依次是：宽度为100px、是直线、颜色为green（未显示的边框颜色都为透明） 2. "middleBranch"是圣诞树的中枝叶，该上枝叶仅通过边框属性即可实现。边框的属性依次是：宽度为200px、是直线、颜色为green（未显示的边框颜色都为透明） 3. "base"是圣诞树的树干，该树干仅通过左外边距实现居中于中枝叶。树干的宽度、高度分别为70px、200px，颜色为gray。 注意： 1. 上枝叶、树干的居中都是通过左外边距实现的 2. 没有显示的边框，其属性都是透明（属性） 3. 仅通过border属性完成边框的所有属性设置!
```js
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <title></title>
    <style>
        .topbranch {
            width: 0px;
            height: 0px;
            /*
                        * TODO: 上枝叶效果
                        */

            border: 100px solid transparent;
            border-bottom-color: green;
            float: left;
            margin-left: 100px;
        }

        .middleBranch {
            width: 0px;
            height: 0px;
            /*
                        * TODO: 中枝叶效果
                        */
            width: 0;
            border: 200px solid transparent;
            border-bottom-color: green;


        }

        .base {
            /*
                        * TODO: 树干效果
                        */
            width: 70px;
            height: 200px;
            background-color: gray;
            margin-left: 165px;

        }
    </style>
</head>

<body>
    <section class="topbranch"></section>
    <section class="middleBranch"></section>
    <section class="base"></section>
</body>

</html>
```

## FEd20 基本数据类型检测
描述: 请补全JavaScript函数，要求以字符串的形式返回参数的类型。 注意：只需检测基本数据类型。 代码：
```js
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <style>
       /* 填写样式 */
    </style>
</head>

<body>
    <!-- 填写标签 -->
    <script type="text/javascript">
        // 填写JavaScript
        function _typeof(value) {
        return typeof(value)
}
    </script>
</body>

</html>
```
## FEd21 检测复杂数据类型
描述: 请补全JavaScript函数，要求以Boolean的形式返回第一个参数是否属于第二个参数对象的实例。 代码：
```js
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <style>
       /* 填写样式 */
    </style>
</head>

<body>
    <!-- 填写标签 -->
    <script type="text/javascript">
        // 填写JavaScript
        function _instanceof(left,right) {
        return (left instanceof right)
}
    </script>
</body>

</html>
```

## FEd22 数据类型转换
描述: 请补全JavaScript函数，要求以字符串的形式返回两个数字参数的拼接结果。 示例： 1. _splice(223,233) -> "223233" 2. _splice(-223,-233) -> "-223-233" 代码：
```js
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <style>
       /* 填写样式 */
    </style>
</head>

<body>
    <!-- 填写标签 -->
    <script type="text/javascript">
        // 填写JavaScript
        function _splice(left,right) {
            left1=left.toString();
            right1=right.toString();
        return left1.concat('',right1)
}
    </script>
</body>

</html>
```

## FEd23 阶乘
描述: 请补全JavaScript函数，要求返回数字参数的阶乘。 注意：参数为大于等于0的整数。 代码：
```js
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <style>
       /* 填写样式 */
    </style>
</head>

<body>
    <!-- 填写标签 -->
    <script type="text/javascript">
        // 填写JavaScript
        function _factorial(number) {
   if(number===1) return number
   else return number*_factorial(number-1)
}
    </script>
</body>

</html>
```

## FEd24 字符串字符统计
描述: 统计字符串中每个字符的出现频率，返回一个 Object, key为统计字符，value 为出现频率 1. 不限制 key 的顺序 2. 输入的字符串参数不会为空 3. 忽略空白字符
输入描述： 'hello world'
输出描述： {h: 1, e: 1, l: 3, o: 2, w: 1, r: 1, d: 1}
代码：

```js
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <style>
       /* 填写样式 */
    </style>
</head>

<body>
    <!-- 填写标签 -->
    <script type="text/javascript">
        // 填写JavaScript
        function count(str) {
            let arr=Array.from(str);
            let obj={};
            arr.forEach(function(item,i){
                if(item!=''){
                    if(!obj.hasOwnProperty(item)){
                        obj[item]=1;
                    }
                    else{
                        obj[item] +=1;
                    }
                }
            })
            return obj;
}
    </script>
</body>

</html>
```
## FEd25 从大到小排序
描述： 请补全JavaScript函数，要求将数组参数中的数字从大到小进行排序并返回。 代码：
```js
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <style>
       /* 填写样式 */
    </style>
</head>

<body>
    <!-- 填写标签 -->
    <script type="text/javascript">
        // 填写JavaScript
        function _sort(array) {
            array.sort(function(a,b){return b-a});
            return array;
}
    </script>
</body>

</html>
```

## FEd26 对象属性键名
描述: 请补全JavaScript函数，要求以数组的形式输出对象各个属性的键名。 示例： 1. _keys({name:'nowcoder',age:7}) -> ['name','age'] 注意：只需考虑对象属性均为原始数据类型的情况。 代码：
```js
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <style>
       /* 填写样式 */
    </style>
</head>

<body>
    <!-- 填写标签 -->
    <script type="text/javascript">
        // 填写JavaScript
        function _keys(object) {
  return Object.keys(object)
}
    </script>
</body>

</html>
```

## 手写JS代码

```js
1.手写Object.create
function create(obj){
    funticon F(){}
    F.prototype=obj
    return new F()
}
将传入的对象作为原型

2.手写instanceof
function myInstanceof(left,right){
    let proto=Object.getPrototypeOf(left),
        prototype=right.prototype;
    
    while(true){
        if(!proto)return false;
        if(proto===prototype)return true;

        proto=Object.getPrototypeOf(proto);
    }
}
1. 首先获取类型的原型
2. 然后获得对象的原型
3. 然后一直循环判断对象的原型是否等于类型的原型，直到对象原型为 null，因为原型链最终为 null

3.手写new函数
fuction ObjectFactory(){
    let newObjct=null;
    let constructor=Array.prototype.shift.call(arguments);
    let result=null;
    if(typeof consturctor !== "function"){
        console.error("type error")
        return;
    }

    newObject = Object.create(constructor.prototype);
    result=constructor.apply(newObject,arguments);
    let flag=result&&(typeof result==="object"||typeof result==="function");
    return flag?result:newObject;
}
objectFactory(构造函数，初始化参数)
（1）首先创建了一个新的空对象
（2）设置原型，将对象的原型设置为函数的 prototype 对象。
（3）让函数的 this 指向这个对象，执行构造函数的代码（为这个新对象添加属性）
（4）判断函数的返回值类型，如果是值类型，返回创建的对象。如果是引用类型，就返回这个引用类型的对象。

4.手写Promise函数
const PENDING='pending'
const RESOLVED='resolved'
const REJECTED='rejected'

5.手写防抖函数
function debounce(fn,wait){
    let timer=null;
    return function(){
        let context=this,
            args=arguments;
        if(timer){
            clearTimeout(timer);
            timer=null;
        }
        timer=setTimeout(()=>{
            fn.apply(context,args);
        },wait)
    }
}

6.手写节流函数
function throttle(fn,delay){
    let curTime=Date.now();
    return function(){
        let context=this;
            args=arguments;
            nowTime=Date.now();
        if(nowTime-curTime>=delay){
            curTime=Date.now;
            return fn.apply(context,args);
        }
    }
}

7.手写call函数
Function.prototype.myCall=function(context){
    if(typeof this !== "function"){
        console.error("type error");
    }
    let args=[...arguments].slice(1);
        result=null;
    context=context||window;
    context.fn=this;
    result=context.fn(...args);
    delete context.fn;
    return result
}
1. 判断调用对象是否为函数，即使我们是定义在函数的原型上的，但是可能出现使用 call 等方式调用的情况。
2. 判断传入上下文对象是否存在，如果不存在，则设置为 window 。
3. 处理传入的参数，截取第一个参数后的所有参数。
4. 将函数作为上下文对象的一个属性。
5. 使用上下文对象来调用这个方法，并保存返回结果。
6. 删除刚才新增的属性。
7. 返回结果。


8.手写apply函数
Function.prototype.myApply=Function(context){
    if(typeof this !=== "function"){
        console.error("type error")
    }
    let result=null;
    context=context||window;
    context.fn=this;
    if(arguments[1]){
        result=context.fn(...arguments[1]);
    }
    else{
        result=context.fn();
    }
    delete context.fn;
    return result;
}

9.数组去重
function uniqueArray(array){
    let map={};
    let arr=[];
    for(let i=0;i<array.length;i++){
        if(!map.hasOwnProperty([array[1]])){
            map[array[i]]=1;
            arr.push(array[i]);
        }
    }
    return arr;
}

Array.from(new Set(array))

10.数组flat方法
function _flat(arr,depth){
    if(!Array.isArray(arr)||depth<=0){
        return arr;
    }
    return arr.reduce((prev,cur)=>{
        if(Array.isArray(cur)){
            return prev.concat(_flat(cur,depth-1))
        }
        else{
            return prev.concat;
        }
    },[])
}

11.循环打印红绿灯
function task=(timer,light)=>
    new Promise((resolve,reject)=>{
        setTimeout(()=>{
            if(light==='red'){
                red();
            }
            else if(light==='green'){
                green();
            }
            else if(light==='yellow'){
                yellow();
            }
            resolve();
        },timer)
    })

const step=()=>{
    task(3000,red)
        .then(()=>task(2000,green))
        .then(()=>task(1000,yellow))
        .then(step)
}
step()
```
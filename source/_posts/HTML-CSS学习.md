---
title: HTML-CSS学习
date: 2024-11-05 10:31:31
tags:
  - Frontend
  - Web Development
---

css的样式文件
# html5+css3->H5C3

软件 vscode 像素大厨

## 

### 网页：

HTML格式，通过浏览器阅读，图片、文字、音频等元素，HTML文件

# HTML：

超文本标记语言 代码>浏览器>网页
HTML中不区分大小写，但是我们一般都使用小写
HTML中的注释不能嵌套
HTML标签必须结构完整，要么成对出现，要么自结束标签
HTML标签可以嵌套，但是不能交叉嵌套
HTML标签中的属性必须有值，且值必须加引号(双引号单引号都可以)
### Web标准：

结构（HTML）、表现（CSS）、行为（JavaScript）相分离

### HTML标签：

#### 语法规范：
```<html></html>```成对出现，双标签
```<br />```单标签 
标签关系：包含、并列

#### 结构标签：
```<html>```标签 页面中最大的标签，称为根标签
```<head>```头部标签，里面必须设置```<title>```标签
```<title>```网页标题
```<body>```包含文档所有内容

VScode工具生成骨架标签：
```<!DOCTYPE>```标签：文档类型声明，HTML版本，在文档中的最前面``` <!DOCTYPE html>```
lang语言：```<html lang="en">```  en ——英文 zh-CN——中文
charset字符集：```<meta charset="UTF-8"/>```

#### 常用标签
标签语义
##### 标题标签
```<h1>-<h6>```
作为标题使用，按照重要性递减
特点：文字加粗变大，独占一行
##### 段落标签
```<p>```定义段落 文本在一个段落中会根据浏览器窗口大小自动换行
##### 换行标签
```<br />```单标签 强制换行 
##### 文本格式化标签
**粗体**、*斜体*、```<u>下划线</u>```
加粗：```<strong></strong>   <b></b>```
倾斜：```<em></em>   <i></i>```
删除线：```<del></del>    <s></s>```
下划线：```<ins></ins>    <u></u>```
##### div、span标签
```<div>、<span>```没有语义，用于装内容
```<div>```标签用于布局，一行只能放一个```<div>```
```<span>```标签用于布局，一行可以放多个```<span>```
##### 图像标签
1.图像标签```<img src="" />```
src属性：用于指定图像文件的路径、文件名
其他属性：
alt属性：图像显示不出来的时候用文字代替
title属性：鼠标放在图片上提示的文字
像素属性：
width属性：图像宽度
height属性：图像高度
border属性：图像边框
1）图像标签可以有多个属性，必须写在标签名后面
2）属性之间不分先后顺序，以空格分开
3）key=“value” 键值对
##### 路径
###### 1.目录文件夹
根目录：打开目录文件夹的第一层就是根目录
######2.路径
1.相对路径：以引用文件所在位置为参 考基础，建立出的目录路径
/
同一级路径：图像文件位于HTML文件同一级``` <img src="baidu.gif"/>```
下一级路径：图像文件位于HTML文件下一级```<img src="images/baidu.gif"/>```  
上一级路径：图像文件位于HTML文件上一级```<img src="../baidu.gif"/>```

2.绝对路径:目录下的绝对位置，直接到达目标位置，通常是从盘符开始的路径
\	或网络中的图片绝对路径
##### 超链接标签
```<a>```:从一个页面链接到另一个页面
1.语法格式
```<a href="跳转目标" target="目标窗口的弹出方式"> 文本或图像 </a>```
href 用于指定链接目标的url地址，必须属性
target： _self 默认当前页面打开    _blank在新窗口打开
2.链接分类

1.外部链接 href="http://...."
2.内部链接：网站内部页面之间的相互链接 href="index.html" 直接链接内部页面名称
3.空链接 ```<a href="#">首页</a>```
4.下载链接 href地址为一个.exe文件或.zip压缩包
5.网页元素链接 文本、图像、表格、音频、视频都可以添加超链接
6.锚点链接 点击链接可以快速定位到页面的某个位置

- 在链接文本的href属性中设置属性为#名字的形式，如```<a href="#two">第二集</a>```
- 在目标位置标签处添加id属性，如```<h3 id="two">第二集介绍</h3>``` 
####注释标签
便于阅读和理解而不显示在页面中的注释文字

#### 特殊字符
空格符``` &nbsp;```
小于号``` &lt```;
大于号``` &gt```;
#### 表格标签
表格：
1.显示、展示数据
2.基本语法

```<table>```
```  <tr>    定义表格中的行```
```   <td>单元格内的文字</td> 定义单元格的内容 table data```
```  </tr>```
```</table>```

3.表头标签

```<table>```
```  <tr>```
```    <th>姓名</th>  table head 加粗居中```
```  </tr>```
```</table>```

4.表格属性
align (left cengter right) 表格相对周围元素的对齐方式
border 1or"" 规定表格单元是否有边框
cellpadding 像素值，规定表格单元边沿与其内容之间的空白，默认为1
cellspacing 像素值，规定单元格之间的空白，默认为2
width 像素值或百分比，规定表格的宽度
5.结构标签
区域
```<thead>```标签头部，内部必须包含```<tr>```标签
```<tbody>```标签主体
6.合并单元格
把多个单元格合并为一个单元格
跨行合并：rowspan=“合并单元格个数” 最上侧单元格为目标单元格，写合并代码``` <td rowspan="2">```
跨列合并：colspan=“合并单元格个数” 最左侧单元格为目标单元格，写合并代码``` <td colspan="2">```
#### 列表标签
用于布局，整齐有序
##### 无序列表
```<ul>```标签表示HTML页面中的无序列表，一般以项目符号呈现列表项，列表项使用```<li>```标签定义
```<ul>```
```  <li>列表项1</li>```
```  ...```
```</ul>```
1.没有顺序之分
2.```<ul>```中只能嵌套```<li>```，输入其他标签或文字不被允许
3.```<li>```中可以放任何元素
4.无序列表会有自己的样式属性，用css设置
##### 有序列表
```<ol>```标签用于定义有序列表，列表排序用数字来显示
```<ol>```
```  <li>列表项1</li>```
```  ...```
```</ol>```
##### 自定义列表
用于对术语或名词进行解释和描述
```<dl>```
```  <dt>名词1</dt>   定义项目/名字```
```  <dd>名词1解释1</dd>```
```  <dd>名词1解释2</dd>```
```</dl>```
1.```<dl>```包含里只能```<dt><dd>```
2.通常是一个```<dt>```对应多个```<dd>```
####表单标签
用于收集用户信息
表单的组成
1.表单域
```<form>```标签用于定义表单域，会把范围内的表单元素信息提交给**服务器**
```<form action="url地址" method="提交方式get/post" name="表单域名称">```
    各种表单元素控件
```</form>```
url地址用于指定接收并处理表单数据的服务器程序的地址
2.表单控件（表单元素）
 -``` <input>```表单元素``` <input type="..." /> ```type属性设置不同控件类型
 radio 单选按钮，实现多选一，name值相同时可以多选一
 checkbox 复选框，实现多选
 file 文件域，上传使用的文件
 input除type属性外，其他属性：
 name：定义input元素名称，单选按钮和复选框都要有相同的name值
 value：规定input元素的值
 checked：规定此input元素**首次加载时应被选中**，属性值为checked
 maxlength：规定输入字段中的字符的最大长度
 ```<label>```标签为input元素定义标注，绑定一个表单元素，当点击```<label>```标签内的文本时浏览器自动将光标转到对应的表单元素上
 ```<input type="radio" id="male">```
 ```<label for="male">男</label>```
 - ```<select>```表单元素 有多个选项让用户选择，定义下拉列表
 ```<select>```
     ``` <option>xxx</option>```
     ``` <option>xxx</option>```
```</select>```
1.```<select>```至少包含一对```<option>```
2.在```<option>```中定义selected="selected"，当前项即为默认选中项
- ```<textarea>```表单元素 当用户输入内容较多的情况下，定义多行文本输入
```<textarea rows="xx" cols="xx">```
```文本内容```
```</textarea>```

3.提示信息

www.w3school.com

# CSS
层叠样式表，美化网页布局页面
1.css语法规范
选择器加上一条或多条声明
h1{color:red;}
2.代码风格
- 样式格式书写：展开格式
- 样式大小写：小写
- 空格规范：属性值前冒号后加一个空格，选择器和大括号加一个空格
## 基础选择器
### 标签选择器
按标签名称进行分类
语法：
标签名{
    属性1: 属性值1;
    ...
}
### 类选择器
差异化选择不同标签
.类名{
    属性1: 属性值1;
}
```<div class="red">```
一个标签可以用多个名字
### id选择器
（）#类名{
    属性1: 属性值1；
}
```<div id="red">``` id选择器只能被调用一次
### 通配符选择器
选取页面所有标签元素
* {
    属性1: 属性值1；
}
## 字体属性
font-family定义文本字体
各种字体间必须用英文状态下的逗号隔开
一般情况下有空格隔开的多个单词组成的字体加引号
font-size定义字体大小
20px；标题标签需要单独指定文字大小
font-weight定义文字粗细
normal|bold|bolder|lighter|number
font-style定义文本风格（斜体）
normal|italic
给斜体标签```<em><i>```改为normal不倾斜字体
font复合属性：
font:font-style font-weight **font-size**/line-height **font-family**;固定顺序
## 文本属性
外观、颜色、对齐文本、装饰文本、缩进、行间距
color定义颜色
- 预定义的颜色值
- 十六进制
- RGB代码

text-align属性用于设置文本水平对齐方式
center、left、right
text-decoration定义下划线、删除线、上划线
none|underline|overline|line-through
text-indent定义段落首行缩进
text-indent：20px；2em；（一个em是一个文字大小）
line-height定义行间距离
行间距=上间距+文本高度+下间距


## CSS引入方式
内部样式表：将CSS放入HTML页面```<style>```标签中
行内样式表：标签内部style=“..."
外部样式表：新建一个后缀为.css的样式文件，在HTML文件使用```<link>```标签引入``` <link rel="stylesheet" href=“CSS文件路径“>```

## Emmet语法
使用缩写提高html/css编写速度
...

## 复合选择器
### 后代选择器
可以选择父元素里面的子元素
元素1 元素2 {
    样式声明
}
选择元素1里所有的元素2
### 子选择器
选择元素最近一级子元素 
元素1>元素2{
    样式声明
}
### 并集选择器
选择多组标签定义相同的样式
元素1,元素2{
    样式声明
}
任何选择器都可以作为并集选择器的一部分
### 伪类选择器
向选择器加特殊效果
链接伪类：
a:link 选择所有未被访问的链接
a:visited 选择所以已被访问的链接
a:hover 选择鼠标指针位于其上的链接
a:active 选择活动链接
顺序：l v h a

focus伪类：
用于选取**获得焦点**的表单元素
input:focus{
    background-color:pink;
}
## 元素显示模式
块元素
```<div> <p> <ul>```
- 独占一行
- 高度宽度内外边距可以调整
- 里面可以放行内或级块元素
- 文字类元素里面不能使用级块元素

行元素
```<span><a>```
- 一行可以显示多个
- 高宽直接设置无效
- 行内元素只能容纳文本或其他行内元素

行内块元素
```<img><input><td>```
- 相邻行内块元素在一行上，之间有空白缝隙
- 默认宽度为本身内容宽度
- 高度行高外边距内边距都可以控制

元素显示模式转换
```<a>```
行转换为块：display:block；
块转换为行：display:inline；
转换为行内块元素：display:inline-block;

## 背景
背景颜色
background-color:transparent|color;
背景图片
便于控制 位置，用于logo、装饰性小图片或超大背景图片
background-image:none|url();
背景平铺
backgroung-repeat:repeat|no-repeat|repeat-x|repeat-y
背景图片位置
backgroung-position:x y;
x和y可以使用方位名词（top、center、bottom、left、right）或精确单位（第一个为x坐标，第二个为y坐标）
背景图像固定
background-attachment:scroll|fixed
复合写法：
background:color url repeat attachment position
背景色半透明
background : rgba(0,0,0,0.3)
alpha在0-1之间，表示透明度


优先级：
继承或* 0,0,0,0
元素选择器 0,0,0,1
类选择器 0,0,1，0
ID选择器 0,1，0，0
！important 无限大
复合选择器会涉及到权重叠加，需要计算权重，权重之间没有进位
##盒子模型
页面布局：盒子模型、浮动、定位
盒子：边框、内外边距、实际内容
padding内边距 margin外边距
边框border：边框宽度、边框样式、边框颜色
border-width
border-style:none|hidden|dotted（点线边框）|dashed（虚线)|solid（实线边框）|groove
border-color
边框简写：1px solid red；无顺序 border-top（上边框）
border-collapse:collapse;相邻边框合并在一起
边框会影响盒子大小
内边距padding：边框与内容之间的距离

padding:5px;上下左右
padding:5px 10px;上下 左右
padding:5px 10px 20px;上 左右 下
padding:5px 10px 20px 30px;上 右 下 左

内边距会影响盒子大小，如果盒子本身没有指定width/height属性则padding不会撑开盒子大小
外边距margin：盒子与盒子之间的距离
外边距应用：让盒子水平居中：条件1盒子指定了宽度 条件2左右外边距设置为auto 让行内元素或行内块元素水平居中：给父元素添加text-align:center
嵌套块元素垂直外边距的塌陷：
对于两个嵌套关系（父子关系）的块元素，父元素上外边距同时子元素也有上外边距，此时父元素会塌陷较大的外边距值
解决方案：
- 为父元素定义上边框
- 为父元素定义上内边距
- 为父元素添加overflow:hidden

清除内外边距：
*{
    padding:0;
    margin:0;
}

## 圆角边框
border-radius属性用于设置元素的外边框圆角
border-radius:length;   length取值可以为px或% 50%表示圆形 设置为高度一半为圆角矩形
圆与边框的交集形成圆角效果
##盒子阴影
box-shadow：h-shadow v-shadow blur spread color inset
h-shadow:水平阴影位置必须为负值
v-shadow:垂直阴影位置必须为负值
## 浮动
改变元素标签默认排列方式
**多个块级元素纵向排列找标准流 横向排列找浮动**
选择器{
    float：none|left|right
}
浮动特性：
1.脱离标准流，不再保留原先位置
2.如果多个盒子都设置了浮动，则会按照属性值一行内显示并且顶端对齐排列
3.任何元素添加浮动之后有行内块元素相似的特性 

为了约束浮动元素的位置，通常使用标准流的父元素排列上下位置，之后内部子元素采取浮动排列左右位置 
清除浮动：清除浮动元素造成的排版影响
选择器{
    clear：属性值；
}
left|right|both 
额外标签法（隔墙法）：在浮动元素末尾添加一个空的标签``` <div style="clear:both"></div>``` or``` <div class="clear" ></div>```
父级添加overflow：overflow:hidden;
after伪元素法：
双伪元素清除浮动 

## 定位
定位模式+边偏移
position属性：static|relative|absolute|fixed
边偏移：top left right bottom

1.static静态定位
默认定位方式，无定位
2.relative相对定位
元素移动时相对于原来的位置
选择器
{
    position:relative;
}
原来在标准流的位置继续占有，后面的盒子仍然以标准流的方式对待他
3.absolute绝对定位
移动相动时相对于祖先元素
没有祖先元素或祖先元素没有定位，以浏览器为准定位
如果祖先元素有定位，则以最近一级的有定位祖先元素为参考点移动位置
绝对定位不再占有原先位置
4.fixed固定定位
元素固定在浏览器**可视区**的位置，在浏览器页面滚动时元素位置不改变
和父元素无关，不随滚动条滚动
不占有原先位置
- 固定定位小技巧：固定在版心右侧位置
1.固定定位盒子left：50% 走到浏览器可视区一半的位置
2.固定定位盒子margin-left：版心宽度一半距离 
5.sticky粘性定位
选择器{
    position：sticky；
    top：10px；
}
以浏览器可视窗口为参照点移动元素（固定定位特点）
粘性定位占有原先位置（相对定位特点）
必须添加top、left、right、bottom其中一个才有效

定位叠放次序z-index
数值可以是正整数、负整数或0；默认为auto，数值越大，盒子越靠上
如果属性值相同，按照书写顺序后来居上
数字后面不加单位
只有定位的盒子才有该属性

绝对定位和固定定位和浮动类似
行内元素添加绝对或固定定位，可以直接设置高度和宽度
块级元素添加绝对或者固定定位，如果不给宽度或高度，默认大小是内容大小
浮动元素、定位元素都不会触发外边距合并问题

绝对定位会完全压住盒子
浮动元素只会压住标准流的盒子，不会压住标准流盒子里面的文字和图片

# CSS高级技巧
##精灵图（sprites）
有效减少服务器接收和发送请求的次数，提高页面加载速度
原理：将网页中的一些小背景图像整合到一张大图中，这样服务器只需要一次请求就可以

核心：
1.主要针对背景图片使用，把多个小背景图片整合到一张大图片中
2.移动背景图片位置，可以使用background-position
3.移动距离就是这个目标图片的x和y坐标
4.往上往左移动数值是负值
5.使用精灵图需要精确测量每个小背景图片的大小和位置
##字体图标iconfont
字体图标展示的是图标本质属于字体
适合结构和样式比较简单的小图标
下载网站：
http://icomoon.io
http://www.iconfont.cn/
1.把下载包里面的fonts文件放入页面根目录下
2.在CSS样式中全局声明字体 style.css
3.在html标签内添加小图标 在demo.html文件中查找
4.声明字体ifont-family

字体图标的追加：把压缩包里的selection.json从新上传，然后选中需要的新图标，从新下载压缩包，并替换原来的文件
## CSS三角
网页中的常见三角形直接用CSS画出来，不必做成图片或字体图标
div{
    width:0;
    height:0;
    border:50px solid transparent;
    border-left-color:pink;
}

## CSS用户界面样式
### 更改用户鼠标样式 cursor
li{
    cursor:default;
}
default：小白 默认
pointer：小手
move：移动
text：文本
not-allowed：禁止
###轮廓线 outline
给表单添加outline：0或者outline：none样式之后就可以去掉默认蓝色边框
###防止拖拽文本域 resize
resize:none

## vertical-align属性应用
用于设置图片或表单和文字的垂直对齐 行内元素或行内块元素
vertical-align：baseline|top|middle|bottom
baseline：默认 元素放置在父元素的基线上
top：把元素顶端与行中最高元素的顶端对齐
middle：把此元素放置在父元素的中部
bottom：把元素顶端与行中最低的元素的顶端对齐

## 溢出文字省略号显示
### 单行文本
1.强制一行内显示文本
white-space：nowrap；（默认normal自动换行）
2.超出部分隐藏
overflow：hidden；
3.文字用省略号替代超出的部分
text-overflow：ellipsis；
### 多行文本
适合于webKit浏览器

## CSS初始化
CSS初始化指重设浏览器的样式（CSS reset)
每个网页都必须进行CSS初始化


# HTML5新增特性
header头部标签
nav导航标签
aside侧边栏
section定义文档某个区域
article内容标签
footer尾部标签

audio音频
video视频 支持MP4 WebM Ogg 尽量使用MP4 
属性
autoplay =“autoplay"自动播放
controls=“controls”向用户显示播放控件

# CSS3新增特性
1.属性选择器
 E[att]选择具有att属性的E元素
 E[att="val"]
 E[att^="val"] att属性以val开头的E元素
 E[att$="val"] att属性以val结尾的E元素
 E[att*="val"] att属性值中含有val的E元素

2.结构伪类选择器
根据文档结构选择元素，常用于父级选择器里面的子元素
E:first-child 匹配父元素中的第一个子元素E
E:last-child
E:nth-child(n) 选择某个父元素的一个或多个特定的子元素
**n可以是数字、关键字和公式**
 n是数字，选择第n个元素
 n是关键字：even偶数 odd奇数
 n是公式：n从0开始 2n代表偶数 2n+1代表奇数
n+5 从第五个开始到最后
-n+5 前五个包括第五个
E:first-of-type
E:last-of-type
E:nth-of-type(n)
nth=child:对父元素里面的所有孩子排序选择，先找到第n个孩子，然后看是否匹配
nth-of-child：对父元素里面指定子元素进行排序选择。先匹配E，再根据E找第n个孩子

过渡 和：hover一起使用
transition：要过渡的属性 花费时间 运动曲线 何时开始；
运动曲线：默认为ease（逐渐慢下来） linear  ease-in ease-out ease-in-out
如果要写多个属性，用逗号分割，想要所有属性变化可以用all


转换  可以实现元素位移、旋转、缩放
transform：
2D转换
translate 优点：不会影响其他元素的位置
transform:translate(x,y)
translate中的百分比单位是相对于自身元素的
对行内标签没有效果
rotate 旋转
transform：rotate（度数）
rotate度数单位是deg
角度为正时顺时针负时为逆时针，默认旋转中心为元素中心点
转换中心点：transform-origin：x y；
scale 缩放 优点：不会影响其他盒子可以设置中心点缩放
transform：scale（x，y)
1.x和y是数字，表示缩放倍数
2.只有一个数字表示等比例缩放 
同时使用多个转换，格式为transform：translate() rotate() scale() 位移必须在最前面

动画 animation 设置多个节点来精确控制一个或一组动画
1.先定义动画
用keyframes定义动画
@ketframes 动画名称{
    0%(from){
        width:100px;
    }
    100%(to){
        width:200px;
    }
}
2.再使用动画
animation-name:...;
animation-duration:...;

常用属性：
必须：
**animation-name:...;
animation-duration:...;持续时间**
animation-timing-function 规定默认速度曲线
animation-delay:
animation-iteration-count 规定动画被播放次数 infinite 无限
animation-direction 规定下一周期是否逆向播放 默认为normal alternate逆播放
animation-play-state 规定动画是否正在运行或暂停
animation-fill-mode 规定动画结束后状态 保持forwards回到起始backwards

3D转换 近大远小物体后面遮挡不可见
x轴：水平向右 右边是正值
y轴：垂直向下 下面是正值
z轴：垂直屏幕 往外面是正值
---
title: Vue
date: 2024-11-12 11:45:45
tags:
  - Vue
  - Frontend
  - JavaScript
  - Web Development
---

## 模板语法

Vue 使用一种基于 HTML 的模板语法，能够声明式地将其组件实例的数据绑定到呈现的 DOM 上。所有的 Vue 模板都是语法层面合法的 HTML，可以被符合规范的浏览器和 HTML 解析器解析。

在底层机制中，Vue 会将模板编译成高度优化的 JavaScript 代码。结合响应式系统，当应用状态变更时，Vue 能够智能地推导出需要重新渲染的组件的最少数量，并应用最少的 DOM 操作。

文本插值
最基本的数据绑定形式是文本插值，它使用的是“Mustache”语法 (即双大括号)：

```
<span>Message: {{ msg }}</span>
```

双大括号标签会被替换为相应组件实例中 msg 属性的值。同时每次 msg 属性更改时它也会同步更新。

## 原始 HTML
双大括号会将数据解释为纯文本，而不是 HTML。若想插入 HTML，你需要使用 v-html 指令：
```
<p>Using text interpolation: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

这里遇到了一个新的概念。这里看到的 v-html attribute 被称为一个指令。指令由 v- 作为前缀，表明它们是一些由 Vue 提供的特殊 attribute，它们将为渲染的 DOM 应用特殊的响应式行为。这里我们做的事情简单来说就是：在当前组件实例上，将此元素的 innerHTML 与 rawHtml 属性保持同步。

span 的内容将会被替换为 rawHtml 属性的值，插值为纯 HTML——数据绑定将会被忽略。注意，你不能使用 v-html 来拼接组合模板，因为 Vue 不是一个基于字符串的模板引擎。在使用 Vue 时，应当使用组件作为 UI 重用和组合的基本单元。

## Attribute 绑定
双大括号不能在 HTML attributes 中使用。想要响应式地绑定一个 attribute，应该使用 v-bind 指令：
```
<div v-bind:id="dynamicId"></div>
```
v-bind 指令指示 Vue 将元素的 id attribute 与组件的 dynamicId 属性保持一致。如果绑定的值是 null 或者 undefined，那么该 attribute 将会从渲染的元素上移除。

布尔型 Attribute​
布尔型 attribute 依据 true / false 值来决定 attribute 是否应该存在于该元素上。disabled 就是最常见的例子之一。

v-bind 在这种场景下的行为略有不同：
```
<button :disabled="isButtonDisabled">Button</button>
```

当 isButtonDisabled 为真值或一个空字符串 (即 button disabled=””) 时，元素会包含这个 disabled attribute。而当其为其他假值时 attribute 将被忽略。

动态绑定多个值​
如果你有像这样的一个包含多个 attribute 的 JavaScript 对象：
```
data() {
  return {
    objectOfAttrs: {
      id: 'container',
      class: 'wrapper'
    }
  }
}
```

通过不带参数的 v-bind，你可以将它们绑定到单个元素上：
```
<div v-bind="objectOfAttrs"></div>
```

## 使用 JavaScript 表达式​
至此，我们仅在模板中绑定了一些简单的属性名。但是 Vue 实际上在所有的数据绑定中都支持完整的 JavaScript 表达式：
```
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div :id="`list-${id}`"></div>
```

这些表达式都会被作为 JavaScript ，以当前组件实例为作用域解析执行。

在 Vue 模板内，JavaScript 表达式可以被使用在如下场景上：

1.在文本插值中 (双大括号)

2.在任何 Vue 指令 (以 v- 开头的特殊 attribute) attribute 的值中

受限的全局访问​
模板中的表达式将被沙盒化，仅能够访问到有限的全局对象列表。该列表中会暴露常用的内置全局对象，比如 Math 和 Date。

没有显式包含在列表中的全局对象将不能在模板内表达式中访问，例如用户附加在 window 上的属性。然而，也可以自行在 app.config.globalProperties 上显式地添加它们，供所有的 Vue 表达式使用。

## 指令 Directives
指令是带有 v- 前缀的特殊 attribute。Vue 提供了许多内置指令，包括上面的v-bind 和 v-html。

指令 attribute 的期望值为一个 JavaScript 表达式 (除了少数几个例外，即之后要讨论到的 v-for、v-on 和 v-slot)。一个指令的任务是在其表达式的值变化时响应式地更新 DOM。以 v-if 为例：

```
<p v-if="seen">Now you see me</p>
```

这里，v-if 指令会基于表达式 seen 的值的真假来移除/插入该元素。

## 参数 Arguments​
某些指令会需要一个“参数”，在指令名后通过一个冒号隔开做标识。例如用 v-bind 指令来响应式地更新一个 HTML attribute：
```
<a v-bind:href="url"> ... </a>

<!-- 简写 -->
<a :href="url"> ... </a>
```
这里 href 就是一个参数，它告诉 v-bind 指令将表达式 url 的值绑定到元素的 href attribute 上。在简写中，参数前的一切 (例如 v-bind:) 都会被缩略为一个 : 字符。

另一个例子是 v-on 指令，它将监听 DOM 事件：
```
<a v-on:click="doSomething"> ... </a>

<!-- 简写 -->
<a @click="doSomething"> ... </a>
```
这里的参数是要监听的事件名称：click。v-on 有一个相应的缩写，即 @ 字符。之后也会讨论关于事件处理的更多细节。

## 动态参数
同样在指令参数上也可以使用一个 JavaScript 表达式，需要包含在一对方括号内：
```

<a v-bind:[attributeName]="url"> ... </a>

<!-- 简写 -->
<a :[attributeName]="url"> ... </a>
```

这里的 attributeName 会作为一个 JavaScript 表达式被动态执行，计算得到的值会被用作最终的参数。举例来说，如果你的组件实例有一个数据属性 attributeName，其值为 “href”，那么这个绑定就等价于 v-bind:href。

相似地，你还可以将一个函数绑定到动态的事件名称上：

```
<a v-on:[eventName]="doSomething"> ... </a>

<!-- 简写 -->
<a @[eventName]="doSomething"> ... </a>
```
在此示例中，当 eventName 的值是 “focus” 时，v-on:[eventName] 就等价于 v-on:focus。

动态参数值的限制​
动态参数中表达式的值应当是一个字符串，或者是 null。特殊值 null 意为显式移除该绑定。其他非字符串的值会触发警告。

动态参数语法的限制​
动态参数表达式因为某些字符的缘故有一些语法限制，比如空格和引号，在 HTML attribute 名称中都是不合法的。例如下面的示例：
```
<!-- 这会触发一个编译器警告 -->

<a :['foo' + bar]="value"> ... </a>
```

如果你需要传入一个复杂的动态参数，推荐使用计算属性替换复杂的表达式，也是 Vue 最基础的概念之一。

当使用 DOM 内嵌模板 (直接写在 HTML 文件里的模板) 时，我们需要避免在名称中使用大写字母，因为浏览器会强制将其转换为小写：

```
<a :[someAttr]="value"> ... </a>
```

上面的例子将会在 DOM 内嵌模板中被转换为 :[someattr]。如果你的组件拥有 “someAttr” 属性而非 “someattr”，这段代码将不会工作。单文件组件内的模板不受此限制。

## 修饰符 Modifiers

修饰符是以点开头的特殊后缀，表明指令需要以一些特殊的方式被绑定。例如 .prevent 修饰符会告知 v-on 指令对触发的事件调用 event.preventDefault()：
```
<form @submit.prevent="onSubmit">...</form>
```
最后，可以直观地看到完整的指令语法：

![指令语法图](images/image.png)

# 响应式基础
## 声明响应式状态

选用选项式 API 时，会用 data 选项来声明组件的响应式状态。此选项的值应为返回一个对象的函数。Vue 将在创建新组件实例的时候调用此函数，并将函数返回的对象用响应式系统进行包装。此对象的所有顶层属性都会被代理到组件实例 (即方法和生命周期钩子中的 this) 上。
```js
export default {
  data() {
    return {
      count: 1
    }
  },

  // `mounted` 是生命周期钩子，之后我们会讲到
  mounted() {
    // `this` 指向当前组件实例
    console.log(this.count) // => 1

    // 数据属性也可以被更改
    this.count = 2
  }
}
```
这些实例上的属性仅在实例首次创建时被添加，因此需要确保它们都出现在 data 函数返回的对象上。若所需的值还未准备好，在必要时也可以使用 null、undefined 或者其他一些值占位。

## 声明方法

要为组件添加方法，我们需要用到 methods 选项。它应该是一个包含所有方法的对象：

```js
export default {
  data() {
    return {
      count: 0
    }
  },
  methods: {
    increment() {
      this.count++
    }
  },
  mounted() {
    // 在其他方法或是生命周期中也可以调用方法
    this.increment()
  }
}
```

Vue 自动为 methods 中的方法绑定了永远指向组件实例的 this。这确保了方法在作为事件监听器或回调函数时始终保持正确的 this。不应该在定义 methods 时使用箭头函数，因为箭头函数没有自己的 this 上下文。

## DOM 更新时机

当你修改了响应式状态时，DOM 会被自动更新。但是需要注意的是，DOM 更新不是同步的。Vue 会在“next tick”更新周期中缓冲所有状态的修改，以确保不管你进行了多少次状态修改，每个组件都只会被更新一次。

要等待 DOM 更新完成后再执行额外的代码，可以使用 [nextTick()](https://cn.vuejs.org/api/general#nexttick) 全局 API：
```js
import { nextTick } from 'vue'

export default {
  methods: {
    async increment() {
      this.count++
      await nextTick()
      // 现在 DOM 已经更新了
    }
  }
}
```

# 计算属性
## 计算属性缓存 vs 方法
我们在表达式中像这样调用一个函数也会获得和计算属性相同的结果：
```js
<p>{{ calculateBooksMessage() }}</p>
```

```js
// 组件中
methods: {
  calculateBooksMessage() {
    return this.author.books.length > 0 ? 'Yes' : 'No'
  }
}
```

若我们将同样的函数定义为一个方法而不是计算属性，两种方式在结果上确实是完全相同的，然而，不同之处在于计算属性值会基于其响应式依赖被缓存。一个计算属性仅会在其响应式依赖更新时才重新计算。这意味着只要``` author.books``` 不改变，无论多少次访问``` publishedBooksMessage``` 都会立即返回先前的计算结果，而不用重复执行``` getter``` 函数。

这也解释了为什么下面的计算属性永远不会更新，因为``` Date.now()``` 并不是一个响应式依赖：
```js
computed: {
  now() {
    return Date.now()
  }
}
```

相比之下，方法调用**总是**会在重渲染发生时再次执行函数。

为什么需要缓存呢？想象一下我们有一个非常耗性能的计算属性``` list```，需要循环一个巨大的数组并做许多计算逻辑，并且可能也有其他计算属性依赖于 list。没有缓存的话，我们会重复执行非常多次 ```list``` 的``` getter```，然而这实际上没有必要！如果确定不需要缓存，那么也可以使用方法调用。

## 可写计算属性

计算属性默认是只读的。当尝试修改一个计算属性时，会收到一个运行时警告。只在某些特殊场景中可能才需要用到“可写”的属性，可以通过同时提供 getter 和 setter 来创建：

```js
export default {
  data() {
    return {
      firstName: 'John',
      lastName: 'Doe'
    }
  },
  computed: {
    fullName: {
      // getter
      get() {
        return this.firstName + ' ' + this.lastName
      },
      // setter
      set(newValue) {
        // 注意：我们这里使用的是解构赋值语法
        [this.firstName, this.lastName] = newValue.split(' ')
      }
    }
  }
}
```

现在当再运行 this.fullName = 'John Doe' 时，setter 会被调用而 this.firstName 和 this.lastName 会随之更新。

# Class 与 Style 绑定

数据绑定的一个常见需求场景是操纵元素的 CSS class 列表和内联样式。因为 class 和 style 都是 attribute，我们可以和其他 attribute 一样使用 v-bind 将它们和动态的字符串绑定。但是，在处理比较复杂的绑定时，通过拼接生成字符串是麻烦且易出错的。因此，Vue 专门为 class 和 style 的 v-bind 用法提供了特殊的功能增强。除了字符串外，表达式的值也可以是对象或数组。

## 绑定 HTML class

绑定对象

我们可以给``` :class (v-bind:class 的缩写)``` 传递一个对象来动态切换 class：
```
<div :class="{ active: isActive }"></div> 
```

上面的语法表示 active 是否存在取决于数据属性 isActive 的[真假值](https://developer.mozilla.org/en-US/docs/Glossary/Truthy)。

你可以在对象中写多个字段来操作多个 class。此外，:class 指令也可以和一般的 class attribute 共存。举例来说，下面这样的状态：
```js
data() {
  return {
    isActive: true,
    hasError: false
  }
}
```

配合以下模板：
```js
<div
  class="static"
  :class="{ active: isActive, 'text-danger': hasError }"
></div>
```
渲染的结果会是：
```js
<div class="static active"></div>
```

当 isActive 或者 hasError 改变时，class 列表会随之更新。举例来说，如果 hasError 变为 true，class 列表也会变成 "static active text-danger"。

绑定的对象并不一定需要写成内联字面量的形式，也可以直接绑定一个对象：
```js
data() {
  return {
    classObject: {
      active: true,
      'text-danger': false
    }
  }
}
```
这将渲染：
```js
<div class="active"></div>
```
也可以绑定一个返回对象的[计算属性](https://cn.vuejs.org/guide/essentials/computed)。这是一个常见且很有用的技巧：
```js
data() {
  return {
    isActive: true,
    error: null
  }
},
computed: {
  classObject() {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```
```js
<div :class="classObject"></div>
```
绑定数组

我们可以给```:class``` 绑定一个数组来渲染多个 CSS class：
```js
data() {
  return {
    activeClass: 'active',
    errorClass: 'text-danger'
  }
}
```
```js
<div :class="[activeClass, errorClass]"></div>
```
渲染的结果是：
```js
<div class="active text-danger"></div>
```
如果你也想在数组中有条件地渲染某个 class，你可以使用三元表达式：
```js
<div :class="[isActive ? activeClass : '', errorClass]"></div>
```
```errorClass``` 会一直存在，但 activeClass 只会在 isActive 为真时才存在。

然而，这可能在有多个依赖条件的 class 时会有些冗长。因此也可以在数组中嵌套对象：
```js
<div :class="[{ [activeClass]: isActive }, errorClass]"></div>
```

## 绑定内联样式

绑定对象

```:style``` 支持绑定 JavaScript 对象值，对应的是[HTML 元素的 style 属性](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/style) ：
```js
data() {
  return {
    activeColor: 'red',
    fontSize: 30
  }
}
```
```js
<div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```

尽管推荐使用 camelCase，但 :style 也支持 kebab-cased 形式的 CSS 属性 key (对应其 CSS 中的实际名称)，例如：
```js
<div :style="{ 'font-size': fontSize + 'px' }"></div>
```
直接绑定一个样式对象通常是一个好主意，这样可以使模板更加简洁：
```js
data() {
  return {
    styleObject: {
      color: 'red',
      fontSize: '13px'
    }
  }
}
```
```js
<div :style="styleObject"></div>
```
同样的，如果样式对象需要更复杂的逻辑，也可以使用返回样式对象的计算属性。

绑定数组

我们还可以给 :style 绑定一个包含多个样式对象的数组。这些对象会被合并后渲染到同一元素上：
```js
<div :style="[baseStyles, overridingStyles]"></div>
```

自动前缀

当在``` :style ```中使用了需要[浏览器特殊前缀](https://developer.mozilla.org/en-US/docs/Glossary/Vendor_Prefix)的 CSS 属性时，Vue 会自动为他们加上相应的前缀。Vue 是在运行时检查该属性是否支持在当前浏览器中使用。如果浏览器不支持某个属性，那么将尝试加上各个浏览器特殊前缀，以找到哪一个是被支持的。

样式多值

你可以对一个样式属性提供多个 (不同前缀的) 值，举例来说:
```js
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```
数组仅会渲染浏览器支持的最后一个值。在这个示例中，在支持不需要特别前缀的浏览器中都会渲染为 display: flex。

# 条件渲染

## v-if
v-if 指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回真值时才被渲染。
## v-else
也可以使用 v-else 为 v-if 添加一个“else 区块”
```js
<button @click="awesome = !awesome">Toggle</button>

<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>
```
一个 v-else 元素必须跟在一个 v-if 或者 v-else-if 元素后面，否则它将不会被识别。


## v-else-if
顾名思义，v-else-if 提供的是相应于 v-if 的“else if 区块”。它可以连续多次重复使用：

```js
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```
和 v-else 类似，一个使用 v-else-if 的元素必须紧跟在一个 v-if 或一个 v-else-if 元素后面。

## ```<template>``` 上的``` v-if```
因为 v-if 是一个指令，他必须依附于某个元素。但如果我们想要切换不止一个元素，在这种情况下我们可以在一个``` <template>``` 元素上使用 v-if，这只是一个不可见的包装器元素，最后渲染的结果并不会包含这个``` <template>``` 元素。

## v-show

另一个可以用来按条件显示一个元素的指令是 v-show。其用法基本一样：
```js
<h1 v-show="ok">Hello!</h1>
```
不同之处在于 v-show 会在 DOM 渲染中保留该元素；v-show 仅切换了该元素上名为 display 的 CSS 属性。

v-show 不支持在``` <template> ```元素上使用，也不能和 v-else 搭配使用。


# 列表渲染

## v-for

可以使用 v-for 指令基于一个数组来渲染一个列表。v-for 指令的值需要使用 item in items 形式的特殊语法，其中 items 是源数据的数组，而 item 是迭代项的**别名**：
```js
data() {
  return {
    items: [{ message: 'Foo' }, { message: 'Bar' }]
  }
}
```
```js
<li v-for="item in items">
  {{ item.message }}
</li>
```
在 v-for 块中可以完整地访问父作用域内的属性和变量。v-for 也支持使用可选的第二个参数表示当前项的位置索引。
```js
data() {
  return {
    parentMessage: 'Parent',
    items: [{ message: 'Foo' }, { message: 'Bar' }]
  }
}
<li v-for="(item, index) in items">
  {{ parentMessage }} - {{ index }} - {{ item.message }}
</li>
Parent - 0 - Foo

Parent - 1 - Bar
```

v-for 变量的作用域和下面的 JavaScript 代码很类似：
```js
const parentMessage = 'Parent'
const items = [
  /* ... */
]

items.forEach((item, index) => {
  // 可以访问外层的 `parentMessage`
  // 而 `item` 和 `index` 只在这个作用域可用
  console.log(parentMessage, item.message, index)
})
```

## v-for 与对象
也可以使用 v-for 来遍历一个对象的所有属性。遍历的顺序会基于对该对象调用 Object.values() 的返回值来决定。
```js
data() {
  return {
    myObject: {
      title: 'How to do lists in Vue',
      author: 'Jane Doe',
      publishedAt: '2016-04-10'
    }
  }
}
<ul>
  <li v-for="value in myObject">
    {{ value }}
  </li>
</ul>
```
可以通过提供第二个参数表示属性名 (例如 key)：
```js
<li v-for="(value, key) in myObject">
  {{ key }}: {{ value }}
</li>
```
第三个参数表示位置索引：
```js
<li v-for="(value, key, index) in myObject">
  {{ index }}. {{ key }}: {{ value }}
</li>
```
## 在 v-for 里使用范围值
v-for 可以直接接受一个整数值。在这种用例中，会将该模板基于 1...n 的取值范围重复多次。
```js
<span v-for="n in 10">{{ n }}</span>
```
注意此处 n 的初值是从 1 开始而非 0。

## v-for 与 v-if
当它们同时存在于一个节点上时，v-if 比 v-for 的优先级更高。这意味着 v-if 的条件将无法访问到 v-for 作用域内定义的变量别名：

```js
<!--
 这会抛出一个错误，因为属性 todo 此时
 没有在该实例上定义
-->
<li v-for="todo in todos" v-if="!todo.isComplete">
  {{ todo.name }}
</li>
```
在外先包装一层 ```<template> ```再在其上使用 v-for 可以解决这个问题 (这也更加明显易读)：
```js
<template v-for="todo in todos">
  <li v-if="!todo.isComplete">
    {{ todo.name }}
  </li>
</template>
```
## 数组变化侦测
### 变更方法
Vue 能够侦听响应式数组的变更方法，并在它们被调用时触发相关的更新。这些变更方法包括：
- push()
- pop()
- shift()
- unshift()
- splice()
- sort()
- reverse()
### 替换一个数组
变更方法，顾名思义，就是会对调用它们的原数组进行变更。相对地，也有一些不可变 (immutable) 方法，例如``` filter()```，```concat()``` 和 ```slice()```，这些都不会更改原数组，而总是返回一个新数组。当遇到的是非变更方法时，我们需要将旧的数组替换为新的：

```js
this.items = this.items.filter((item) => item.message.match(/Foo/))
```
你可能认为这将导致 Vue 丢弃现有的 DOM 并重新渲染整个列表——幸运的是，情况并非如此。Vue 实现了一些巧妙的方法来最大化对 DOM 元素的重用，因此用另一个包含部分重叠对象的数组来做替换，仍会是一种非常高效的操作。

### 展示过滤或排序后的结果
有时，我们希望显示数组经过过滤或排序后的内容，而不实际变更或重置原始数据。在这种情况下，你可以创建返回已过滤或已排序数组的计算属性。

举例来说：
```js
data() {
  return {
    numbers: [1, 2, 3, 4, 5]
  }
},
computed: {
  evenNumbers() {
    return this.numbers.filter(n => n % 2 === 0)
  }
}
<li v-for="n in evenNumbers">{{ n }}</li>
```
在计算属性不可行的情况下 (例如在多层嵌套的 v-for 循环中)，你可以使用以下方法：
```js
data() {
  return {
    sets: [[ 1, 2, 3, 4, 5 ], [6, 7, 8, 9, 10]]
  }
},
methods: {
  even(numbers) {
    return numbers.filter(number => number % 2 === 0)
  }
}
<ul v-for="numbers in sets">
  <li v-for="n in even(numbers)">{{ n }}</li>
</ul>
```
# 事件处理
## 监听事件
我们可以使用 v-on 指令 (简写为 @) 来监听 DOM 事件，并在事件触发时执行对应的 JavaScript。用法：v-on:click="handler" 或 @click="handler"。

事件处理器 (handler) 的值可以是：

1.内联事件处理器：事件被触发时执行的内联 JavaScript 语句 (与 onclick 类似)。
2.方法事件处理器：一个指向组件上定义的方法的属性名或是路径
## 内联事件处理器
内联事件处理器通常用于简单场景，例如：
```js
data() {
  return {
    count: 0
  }
}
1
2
<button @click="count++">Add 1</button>
<p>Count is: {{ count }}</p>
```
## 方法事件处理器
随着事件处理器的逻辑变得愈发复杂，内联代码方式变得不够灵活。因此 v-on 也可以接受一个方法名或对某个方法的调用。

举例来说：
```js
data() {
  return {
    name: 'Vue.js'
  }
},
methods: {
  greet(event) {
    // 方法中的 `this` 指向当前活跃的组件实例
    alert(`Hello ${this.name}!`)
    // `event` 是 DOM 原生事件
    if (event) {
      alert(event.target.tagName)
    }
  }
}
<!-- `greet` 是上面定义过的方法名 -->
<button @click="greet">Greet</button>
```
方法事件处理器会自动接收原生 DOM 事件并触发执行。在上面的例子中，我们能够通过被触发事件的 event.target 访问到该 DOM 元素。

## 在内联事件处理器中访问事件参数
有时我们需要在内联事件处理器中访问原生 DOM 事件。你可以向该处理器方法传入一个特殊的 $event 变量，或者使用内联箭头函数：
```js
<!-- 使用特殊的 $event 变量 -->
<button @click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>

<!-- 使用内联箭头函数 -->
<button @click="(event) => warn('Form cannot be submitted yet.', event)">
  Submit
</button>
```
```js
methods: {
  warn(message, event) {
    // 这里可以访问 DOM 原生事件
    if (event) {
      event.preventDefault()
    }
    alert(message)
  }
}
```
## 事件修饰符
在处理事件时调用 event.preventDefault() 或 event.stopPropagation() 是很常见的。尽管我们可以直接在方法内调用，但如果方法能更专注于数据逻辑而不用去处理 DOM 事件的细节会更好。

为解决这一问题，Vue 为 v-on 提供了事件修饰符。修饰符是用 . 表示的指令后缀，包含以下这些：

- .stop
- .prevent
- .self
- .capture
- .once
- .passive
.capture、.once 和 .passive 修饰符与[原生 addEventListener 事件](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener#options)相对应：

```js
<!-- 添加事件监听器时，使用 `capture` 捕获模式 -->
<!-- 例如：指向内部元素的事件，在被内部元素处理前，先被外部处理 -->
<div @click.capture="doThis">...</div>

<!-- 点击事件最多被触发一次 -->
<a @click.once="doThis"></a>

<!-- 滚动事件的默认行为 (scrolling) 将立即发生而非等待 `onScroll` 完成 -->
<!-- 以防其中包含 `event.preventDefault()` -->
<div @scroll.passive="onScroll">...</div>
```

.passive 修饰符一般用于触摸事件的监听器，可以用来[改善移动端设备的滚屏性能](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener#%E4%BD%BF%E7%94%A8_passive_%E6%94%B9%E5%96%84%E6%BB%9A%E5%B1%8F%E6%80%A7%E8%83%BD)。

## 按键修饰符
在监听键盘事件时，我们经常需要检查特定的按键。Vue 允许在 v-on 或 @ 监听按键事件时添加按键修饰符。
```js
<!-- 仅在 `key` 为 `Enter` 时调用 `submit` -->
<input @keyup.enter="submit" />
```
你可以直接使用[```KeyboardEvent.key```](https://developer.mozilla.org/zh-CN/docs/Web/API/UI_Events/Keyboard_event_key_values)  暴露的按键名称作为修饰符，但需要转为 kebab-case 形式。

```js
<input @keyup.page-down="onPageDown" />
```
在上面的例子中，仅会在 $event.key 为 'PageDown' 时调用事件处理。

### 按键别名
Vue 为一些常用的按键提供了别名：

- .enter
- .tab
- .delete (捕获“Delete”和“Backspace”两个按键)
- .esc
- .space
- .up
- .down
- .left
- .right
### 系统按键修饰符
你可以使用以下系统按键修饰符来触发鼠标或键盘事件监听器，只有当按键被按下时才会触发。

- .ctrl
- .alt
- .shift
- .meta
### .exact 修饰符
 .exact 修饰符允许精确控制触发事件所需的系统修饰符的组合。

```js
<!-- 当按下 Ctrl 时，即使同时按下 Alt 或 Shift 也会触发 -->
<button @click.ctrl="onClick">A</button>

<!-- 仅当按下 Ctrl 且未按任何其他键时才会触发 -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- 仅当没有按下任何系统按键时触发 -->
<button @click.exact="onClick">A</button>
```
## 鼠标按键修饰符
- .left
- .right
- .middle
这些修饰符将处理程序限定为由特定鼠标按键触发的事件。

# 表单输入绑定
在前端处理表单时，常常需要将表单输入框的内容同步给 JavaScript 中相应的变量。手动连接**值绑定和更改事件监听器**可能会很麻烦：
```js
<input
  :value="text"
  @input="event => text = event.target.value">
```
v-model 指令帮我们简化了这一步骤：
```js
<input v-model="text">
```
另外，v-model 还可以用于各种不同类型的输入，```<textarea>```、```<select>``` 元素。它会根据所使用的元素自动使用对应的 DOM 属性和事件组合：

文本类型的``` <input>``` 和``` <textarea> ```元素会绑定 value property 并侦听 input 事件；
```<input type="checkbox">``` 和``` <input type="radio">``` 会绑定 checked property 并侦听 change 事件；
```<select>``` 会绑定 value property 并侦听 change 事件。
注意

v-model 会忽略任何表单元素上初始的 value、checked 或 selected attribute。它将始终将当前绑定的 JavaScript 状态视为数据的正确来源。应该在 JavaScript 中使用[data](https://cn.vuejs.org/api/options-state#data) 选项来声明该初始值。

## 基本用法
### 文本
```js
<p>Message is: {{ message }}</p>
<input v-model="message" placeholder="edit me" />
```
### 多行文本
```js
<span>Multiline message is:</span>
<p style="white-space: pre-line;">{{ message }}</p>
<textarea v-model="message" placeholder="add multiple lines"></textarea>
```
注意在``` <textarea>``` 中是不支持插值表达式的。需要使用 v-model 来替代。

### 复选框
单一的复选框，绑定布尔类型值：
```js
<input type="checkbox" id="checkbox" v-model="checked" />
<label for="checkbox">{{ checked }}</label>
```

### 单选按钮
```js
<div>Picked: {{ picked }}</div>

<input type="radio" id="one" value="One" v-model="picked" />
<label for="one">One</label>

<input type="radio" id="two" value="Two" v-model="picked" />
<label for="two">Two</label>
```
### 选择器
单个选择器的示例如下：
```js
<div>Selected: {{ selected }}</div>

<select v-model="selected">
  <option disabled value="">Please select one</option>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
```
## 值绑定
对于单选按钮，复选框和选择器选项，v-model 绑定的值通常是静态的字符串 (或者对复选框是布尔值)：

```js
<!-- `picked` 在被选择时是字符串 "a" -->
<input type="radio" v-model="picked" value="a" />

<!-- `toggle` 只会为 true 或 false -->
<input type="checkbox" v-model="toggle" />

<!-- `selected` 在第一项被选中时为字符串 "abc" -->
<select v-model="selected">
  <option value="abc">ABC</option>
</select>
```
但有时我们可能希望将该值绑定到当前组件实例上的动态数据。这可以通过使用 v-bind 来实现。此外，使用 v-bind 还使我们可以将选项值绑定为非字符串的数据类型。

### 复选框
```js
<input
  type="checkbox"
  v-model="toggle"
  true-value="yes"
  false-value="no" />
```
true-value 和 false-value 是 Vue 特有的 attributes，仅支持和 v-model 配套使用。这里 toggle 属性的值会在选中时被设为 'yes'，取消选择时设为 'no'。你同样可以通过 v-bind 将其绑定为其他动态值：
```js
<input
  type="checkbox"
  v-model="toggle"
  :true-value="dynamicTrueValue"
  :false-value="dynamicFalseValue" />
  ```
## 修饰符
### .lazy
默认情况下，v-model 会在每次 input 事件后更新数据 ([IME 拼字阶段的状态](https://cn.vuejs.org/guide/essentials/forms#vmodel-ime-tip)例外)。你可以添加 lazy 修饰符来改为在每次 change 事件后更新数据：

```js
<!-- 在 "change" 事件后同步更新而不是 "input" -->
<input v-model.lazy="msg" />
```
### .number
如果你想让用户输入自动转换为数字，你可以在 v-model 后添加 .number 修饰符来管理输入：

```js
<input v-model.number="age" />
```
如果该值无法被 parseFloat() 处理，那么将返回原始值。

number 修饰符会在输入框有 type="number" 时自动启用。

### .trim
如果你想要默认自动去除用户输入内容中两端的空格，你可以在 v-model 后添加 .trim 修饰符：
```js
<input v-model.trim="msg" />
```

# 生命周期钩子
每个 Vue 组件实例在创建时都需要经历一系列的初始化步骤，比如设置好数据侦听，编译模板，挂载实例到 DOM，以及在数据改变时更新 DOM。在此过程中，它也会运行被称为生命周期钩子的函数，让开发者有机会在特定阶段运行自己的代码。

## 注册周期钩子
举例来说，mounted 钩子可以用来在组件完成初始渲染并创建 DOM 节点后运行代码：
```js
export default {
  mounted() {
    console.log(`the component is now mounted.`)
  }
}
```

还有其他一些钩子，会在实例生命周期的不同阶段被调用，最常用的是 mounted、updated 和 unmounted。

所有生命周期钩子函数的 this 上下文都会自动指向当前调用它的组件实例。注意：避免用箭头函数来定义生命周期钩子，因为如果这样的话你将无法在函数中通过 this 获取组件实例。

## 生命周期图示
下面是实例生命周期的图表。你现在并不需要完全理解图中的所有内容，但以后它将是一个有用的参考。

![组件生命周期图示](images/image2.png)


# 侦听器
## 基本示例
计算属性允许我们声明性地计算衍生值。然而在有些情况下，我们需要在状态变化时执行一些“副作用”：例如更改 DOM，或是根据异步操作的结果去修改另一处的状态。

在选项式 API 中，我们可以使用 [watch 选项](https://cn.vuejs.org/api/options-state#watch)在每次响应式属性发生变化时触发一个函数。

```js
export default {
  data() {
    return {
      question: '',
      answer: 'Questions usually contain a question mark. ;-)',
      loading: false
    }
  },
  watch: {
    // 每当 question 改变时，这个函数就会执行
    question(newQuestion, oldQuestion) {
      if (newQuestion.includes('?')) {
        this.getAnswer()
      }
    }
  },
  methods: {
    async getAnswer() {
      this.loading = true
      this.answer = 'Thinking...'
      try {
        const res = await fetch('https://yesno.wtf/api')
        this.answer = (await res.json()).answer
      } catch (error) {
        this.answer = 'Error! Could not reach the API. ' + error
      } finally {
        this.loading = false
      }
    }
  }
}

<p>
  Ask a yes/no question:
  <input v-model="question" :disabled="loading" />
</p>
<p>{{ answer }}</p>
```
## 深层侦听器
watch 默认是浅层的：被侦听的属性，仅在被赋新值时，才会触发回调函数——而嵌套属性的变化不会触发。如果想侦听所有嵌套的变更，你需要深层侦听器：
```js
export default {
  watch: {
    someObject: {
      handler(newValue, oldValue) {
        // 注意：在嵌套的变更中，
        // 只要没有替换对象本身，
        // 那么这里的 `newValue` 和 `oldValue` 相同
      },
      deep: true
    }
  }
}
```
## 即时回调的侦听器
watch 默认是**懒执行**的：仅当数据源变化时，才会执行回调。但在某些场景中，我们希望在创建侦听器时，立即执行一遍回调。举例来说，我们想**请求一些初始数据，然后在相关状态更改时重新请求数据**。

我们可以用一个对象来声明侦听器，这个对象有 handler 方法和 immediate: true 选项，这样便能强制回调函数立即执行：

```js
export default {
  // ...
  watch: {
    question: {
      handler(newQuestion) {
        // 在组件实例创建时会立即调用
      },
      // 强制立即执行回调
      immediate: true
    }
  }
  // ...
}
```
回调函数的初次执行就发生在 created 钩子之前。Vue 此时已经处理了 data、computed 和 methods 选项，所以这些属性在第一次调用时就是可用的。

## 回调的触发时机
当更改了响应式状态，它可能会同时触发 Vue 组件更新和侦听器回调。

类似于组件更新，用户创建的侦听器回调函数也会被批量处理以避免重复调用。例如，如果我们同步将一千个项目推入被侦听的数组中，我们可能不希望侦听器触发一千次。

默认情况下，侦听器回调会在父组件更新 (如有) **之后**、所属组件的 DOM 更新之前被调用。**这意味着如果你尝试在侦听器回调中访问所属组件的 DOM，那么 DOM 将处于更新前的状态**。

### Post Watchers
如果想在侦听器回调中能访问被 Vue 更新之后的所属组件的 DOM，你需要指明 flush: 'post' 选项：
```js
export default {
  // ...
  watch: {
    key: {
      handler() {},
      flush: 'post'
    }
  }
}
```

### 同步侦听器
你还可以创建一个同步触发的侦听器，它会在 Vue 进行任何更新之前触发：
```js
export default {
  // ...
  watch: {
    key: {
      handler() {},
      flush: 'sync'
    }
  }
}
```
### this.$watch()
我们也可以使用组件实例的 $watch() 方法来命令式地创建一个侦听器：
```js
export default {
  created() {
    this.$watch('question', (newQuestion) => {
      // ...
    })
  }
}
```

如果要在特定条件下设置一个侦听器，或者只侦听响应用户交互的内容，这方法很有用。它还允许你提前停止该侦听器。

# 模板引用
虽然 Vue 的声明性渲染模型抽象了大部分对 DOM 的直接操作，但在某些情况下，我们仍然需要直接访问底层 DOM 元素。要实现这一点，我们可以使用特殊的 ref attribute：
```js
<input ref="input">
```
ref 是一个特殊的 attribute，和 v-for 章节中提到的 key 类似。它**允许我们在一个特定的 DOM 元素或子组件实例被挂载后，获得对它的直接引用**。这可能很有用，比如说在组件挂载时将焦点设置到一个 input 元素上，或在一个元素上初始化一个第三方库。

## 访问模板引用
挂载结束后引用都会被暴露在 this.$refs 之上：
```js
<script>
export default {
  mounted() {
    this.$refs.input.focus()
  }
}
</script>

<template>
  <input ref="input" />
</template>
```

注意，你只可以在**组件挂载后**才能访问模板引用。如果你想在模板中的表达式上访问 $refs.input，在初次渲染时会是 undefined。因为在初次渲染前这个元素还不存在。

## 函数模板引用
除了使用字符串值作名字，ref attribute 还可以绑定为一个函数，会在每次组件更新时都被调用。该函数会收到元素引用作为其第一个参数：
```js
<input :ref="(el) => { /* 将 el 赋值给一个数据属性或 ref 变量 */ }">
```
注意我们这里需要使用**动态的 :ref**绑定才能够传入一个函数。当绑定的元素被卸载时，函数也会被调用一次，此时的 el 参数会是 null。你当然也可以绑定一个组件方法而不是内联函数。

# 组件基础
组件允许我们将 UI 划分为独立的、可重用的部分，并且可以对每个部分进行单独的思考。在实际应用中，组件常常被组织成一个层层嵌套的树状结构：

![组件树](images/image3.png)


这和嵌套 HTML 元素的方式类似，Vue 实现了自己的组件模型，使我们可以在每个组件内封装自定义内容与逻辑。Vue 同样也能很好地配合原生 Web Component。

## 定义一个组件
当使用构建步骤时，我们一般会将 Vue 组件定义在一个单独的 .vue 文件中，这被叫做[单文件组件 (简称 SFC)](https://cn.vuejs.org/guide/scaling-up/sfc)：

```js
<script>
export default {
  data() {
    return {
      count: 0
    }
  }
}
</script>

<template>
  <button @click="count++">You clicked me {{ count }} times.</button>
</template>
```
## 使用组件
要使用一个子组件，我们需要在父组件中导入它。假设我们把计数器组件放在了一个叫做 ButtonCounter.vue 的文件中，这个组件将会以默认导出的形式被暴露给外部。
```js
<script>
import ButtonCounter from './ButtonCounter.vue'

export default {
  components: {
    ButtonCounter
  }
}
</script>

<template>
  <h1>Here is a child component!</h1>
  <ButtonCounter />
</template>
```
若要将导入的组件暴露给模板，我们需要在 components 选项上注册它。这个组件将会以其注册时的名字作为模板中的标签名。

也可以全局地注册一个组件，使得它在当前应用中的任何组件上都可以使用，而不需要额外再导入。

组件可以被重用任意多次：
```js
<h1>Here is a child component!</h1>
<ButtonCounter />
<ButtonCounter />
<ButtonCounter />
```
每当点击这些按钮时，每一个组件都维护着自己的状态，是不同的 count。这是因为每当你使用一个组件，就创建了一个新的实例。

在单文件组件中，推荐为子组件使用 PascalCase 的标签名，以此来和原生的 HTML 元素作区分。虽然原生 HTML 标签名是不区分大小写的，但 Vue 单文件组件是可以在编译中区分大小写的。我们也可以使用 /> 来关闭一个标签。

如果你是直接在 DOM 中书写模板 (例如原生``` <template>``` 元素的内容)，模板的编译需要遵从浏览器中 HTML 的解析行为。在这种情况下，你应该需要使用 kebab-case 形式并显式地关闭这些组件的标签。
```js
<!-- 如果是在 DOM 中书写该模板 -->
<button-counter></button-counter>
<button-counter></button-counter>
<button-counter></button-counter>
```
## 传递 props
如果我们正在构建一个博客，我们可能需要一个表示博客文章的组件。我们希望所有的博客文章分享相同的视觉布局，但有不同的内容。要实现这样的效果自然必须向组件中传递数据，例如每篇文章标题和内容，这就会使用到 props。

Props 是一种特别的 attributes，在组件上声明注册。要传递给博客文章组件一个标题，我们必须在组件的 props 列表上声明它。这里要用到[props](https://cn.vuejs.org/api/options-state#props) 选项：
```js
<!-- BlogPost.vue -->
<script>
export default {
  props: ['title']
}
</script>

<template>
  <h4>{{ title }}</h4>
</template>
```
当一个值被传递给 prop 时，它将成为**该组件实例上的一个属性**。该属性的值可以像其他组件属性一样，在模板和组件的 this 上下文中访问。

一个组件可以有任意多的 props，默认情况下，所有 prop 都接受任意类型的值。

当一个 prop 被注册后，可以像这样以自定义 attribute 的形式传递数据给它：
```js
<BlogPost title="My journey with Vue" />
<BlogPost title="Blogging with Vue" />
<BlogPost title="Why Vue is so fun" />
```
在实际应用中，我们可能在父组件中会有如下的一个博客文章数组：
```js
export default {
  // ...
  data() {
    return {
      posts: [
        { id: 1, title: 'My journey with Vue' },
        { id: 2, title: 'Blogging with Vue' },
        { id: 3, title: 'Why Vue is so fun' }
      ]
    }
  }
}
```
这种情况下，我们可以使用 v-for 来渲染它们：
```js
<BlogPost
  v-for="post in posts"
  :key="post.id"
  :title="post.title"
 />
```
## 监听事件
有时候子组件需要与父组件进行交互。例如，要在此处实现无障碍访问的需求，将博客文章的文字能够放大，而页面的其余部分仍使用默认字号。

在父组件中，我们可以添加一个 postFontSize 数据属性来实现这个效果：
```js
data() {
  return {
    posts: [
      /* ... */
    ],
    postFontSize: 1
  }
}
```

在模板中用它来控制所有博客文章的字体大小：
```js
<div :style="{ fontSize: postFontSize + 'em' }">
  <BlogPost
    v-for="post in posts"
    :key="post.id"
    :title="post.title"
   />
</div>
```
然后，给 <BlogPost> 组件添加一个按钮：
```js
<!-- BlogPost.vue, 省略了 <script> -->
<template>
  <div class="blog-post">
    <h4>{{ title }}</h4>
    <button>Enlarge text</button>
  </div>
</template>
```
这个按钮目前还没有做任何事情，我们想要点击这个按钮来告诉父组件它应该放大所有博客文章的文字。要解决这个问题，组件实例提供了一个自定义事件系统。父组件可以通过 v-on 或 @ 来选择性地监听子组件上抛的事件，就像监听原生 DOM 事件那样：
```js
<BlogPost
  ...
  @enlarge-text="postFontSize += 0.1"
 />
 ```
子组件可以通过调用内置的[$emit 方法](https://cn.vuejs.org/api/component-instance#emit)，通过传入事件名称来抛出一个事件：
```js
<!-- BlogPost.vue, 省略了 <script> -->
<template>
  <div class="blog-post">
    <h4>{{ title }}</h4>
    <button @click="$emit('enlarge-text')">Enlarge text</button>
  </div>
</template>
```
因为有了 @enlarge-text="postFontSize += 0.1" 的监听，父组件会接收这一事件，从而更新 postFontSize 的值。
```js
<!-- BlogPost.vue -->
<script>
export default {
  props: ['title'],
  emits: ['enlarge-text']
}
</script>
```
这声明了一个组件可能触发的所有事件，还可以对事件的参数进行验证。同时，这还可以让 Vue 避免将它们作为原生事件监听器隐式地应用于子组件的根元素。

## 通过插槽来分配内容
一些情况下我们会希望能和 HTML 元素一样向组件中传递内容：

这可以通过 Vue 的自定义``` <slot> ```元素来实现：
```js
<!-- AlertBox.vue -->
<template>
  <div class="alert-box">
    <strong>This is an Error for Demo Purposes</strong>
    <slot />
  </div>
</template>

<style scoped>
.alert-box {
  /* ... */
}
</style>
```
如上所示，我们使用``` <slot>``` 作为一个占位符，父组件传递进来的内容就会渲染在这里。

## 动态组件
有些场景会需要在两个组件间来回切换，比如 Tab 界面：

可以通过 Vue 的``` <component>``` 元素和特殊的 is attribute 实现的：
```js
<!-- currentTab 改变时组件也改变 -->
<component :is="currentTab"></component>
```
在上面的例子中，被传给 :is 的值可以是以下几种：

- 被注册的组件名
- 导入的组件对象
你也可以使用 is attribute 来创建一般的 HTML 元素。

当使用 ```<component :is="...">``` 来在多个组件间作切换时，被切换掉的组件会被卸载。我们可以通过[``` <keepAlive> 组件```](https://cn.vuejs.org/guide/built-ins/keep-alive)强制被切换掉的组件仍然保持“存活”的状态。

# 组件注册
一个 Vue 组件在使用前需要先被“注册”，这样 Vue 才能在渲染模板时找到其对应的实现。组件注册有两种方式：全局注册和局部注册。

## 全局注册
我们可以使用 Vue 应用实例的 .component() 方法，让组件在当前 Vue 应用中全局可用。
```js
import { createApp } from 'vue'

const app = createApp({})

app.component(
  // 注册的名字
  'MyComponent',
  // 组件的实现
  {
    /* ... */
  }
)
```
如果使用**单文件组件**，你可以注册被导入的 .vue 文件：
```js
import MyComponent from './App.vue'

app.component('MyComponent', MyComponent)
```
.component() 方法可以被链式调用：
```js
app
  .component('ComponentA', ComponentA)
  .component('ComponentB', ComponentB)
  .component('ComponentC', ComponentC)
  ```
全局注册的组件可以在此应用的任意组件的模板中使用：
```js
<!-- 这在当前应用的任意组件中都可用 -->
<ComponentA/>
<ComponentB/>
<ComponentC/>
```
所有的子组件也可以使用全局注册的组件，这意味着这三个组件也都可以在彼此内部使用。

## 局部注册
全局注册虽然很方便，但有以下几个问题：

1. 全局注册，但并没有被使用的组件无法在生产打包时被自动移除 (也叫“tree-shaking”)。如果你全局注册了一个组件，即使它并没有被实际使用，它仍然会出现在打包后的 JS 文件中。
2. 全局注册在大型项目中使项目的依赖关系变得不那么明确。在父组件中使用子组件时，不太容易定位子组件的实现。和使用过多的全局变量一样，这可能会影响应用长期的可维护性。
相比之下，局部注册的组件需要在使用它的父组件中显式导入，并且只能在该父组件中使用。它的优点是使组件之间的依赖关系更加明确，并且对 tree-shaking 更加友好。

局部注册需要使用 components 选项：
```js
<script>
import ComponentA from './ComponentA.vue'

export default {
  components: {
    ComponentA
  }
}
</script>

<template>
  <ComponentA />
</template>
```
对于每个 components 对象里的属性，它们的 key 名就是注册的组件名，而值就是相应组件的实现。上面的例子中使用的是 ES2015 的缩写语法，等价于：
```js
export default {
  components: {
    ComponentA: ComponentA
  }
  // ...
}
```
**局部注册的组件在后代组件中不可用**。在这个例子中，ComponentA 注册后仅在当前组件可用，而在任何的子组件或更深层的子组件中都不可用。

# Props
## Props 声明
一个组件需要显式声明它所接受的 props，这样 Vue 才能知道外部传入的哪些是 props，哪些是透传 attribute 。

props 需要使用 props 选项来定义：
```js
export default {
  props: ['foo'],
  created() {
    // props 会暴露到 `this` 上
    console.log(this.foo)
  }
}
```
除了使用字符串数组来声明 props 外，还可以使用对象的形式：
```js
export default {
  props: {
    title: String,
    likes: Number
  }
}
```
对于以对象形式声明的每个属性，key 是 prop 的名称，而值则是该 prop 预期类型的构造函数。比如，如果要求一个 prop 的值是 number 类型，则可使用 Number 构造函数作为其声明的值。

对象形式的 props 声明不仅可以一定程度上作为组件的文档，而且如果其他开发者在使用你的组件时传递了错误的类型，也会在浏览器控制台中抛出警告。

## 传递 prop 的细节
Prop 名字格式
如果一个 prop 的名字很长，应使用 camelCase 形式，因为它们是合法的 JavaScript 标识符，可以直接在模板的表达式中使用，也可以避免在作为属性 key 名时必须加上引号。

```js
export default {
  props: {
    greetingMessage: String
  }
}

<span>{{ greetingMessage }}</span>
```
虽然理论上也可以在向子组件传递 props 时使用 camelCase 形式 (使用 [DOM 内模板](https://cn.vuejs.org/guide/essentials/component-basics#in-dom-template-parsing-caveats)时例外)，但实际上为了和 HTML attribute 对齐，我们通常会将其写为 kebab-case 形式：
```js
<MyComponent greeting-message="hello" />
```
对于组件名推荐使用 [PascalCase](https://cn.vuejs.org/guide/components/registration#component-name-casing)，因为这提高了模板的可读性，能帮助我们区分 Vue 组件和原生 HTML 元素。然而对于传递 props 来说，使用 camelCase 并没有太多优势，因此我们推荐更贴近 HTML 的书写风格。

### 使用一个对象绑定多个 prop
如果你想要将一个对象的所有属性都当作 props 传入，你可以使用没有参数的 v-bind，即只使用 v-bind 而非 :prop-name。例如，这里有一个 post 对象：
```js
export default {
  data() {
    return {
      post: {
        id: 1,
        title: 'My Journey with Vue'
      }
    }
  }
}
```
以及下面的模板：
```js
<BlogPost v-bind="post" />
```
而这实际上等价于：
```
<BlogPost :id="post.id" :title="post.title" />
```
## 单向数据流
所有的 props 都遵循着单向绑定原则，props 因父组件的更新而变化，自然地将新的状态向下流往子组件，而不会逆向传递。这避免了子组件意外修改父组件的状态的情况，不然应用的数据流将很容易变得混乱而难以理解。

另外，每次父组件更新后，所有的子组件中的 props 都会被更新到最新值，这意味着不应该**在子组件中去更改一个 prop。
```js
export default {
  props: ['foo'],
  created() {
    // ❌ 警告！prop 是只读的！
    this.foo = 'bar'
  }
}
```
导致想要更改一个 prop 的需求通常来源于以下两种场景：

1. **prop 被用于传入初始值；而子组件想在之后将其作为一个局部数据属性**。在这种情况下，最好是新定义一个局部数据属性，从 props 上获取初始值即可：
```js
export default {
  props: ['initialCounter'],
  data() {
    return {
      // 计数器只是将 this.initialCounter 作为初始值
      // 像下面这样做就使 prop 和后续更新无关了
      counter: this.initialCounter
    }
  }
}
```
2. **需要对传入的 prop 值做进一步的转换**。在这种情况中，最好是基于该 prop 值定义一个计算属性：
```js
export default {
  props: ['size'],
  computed: {
    // 该 prop 变更时计算属性也会自动更新
    normalizedSize() {
      return this.size.trim().toLowerCase()
    }
  }
}
```
### 更改对象 / 数组类型的 props
当对象或数组作为 props 被传入时，虽然子组件无法更改 props 绑定，但仍然可以更改对象或数组内部的值。这是因为 JavaScript 的对象和数组是按引用传递，对 Vue 来说，阻止这种更改需要付出的代价异常昂贵。

这种更改的主要缺陷是它允许了子组件**以某种不明显的方式影响父组件的状态**，可能会使数据流在将来变得更难以理解。在最佳实践中，应该尽可能避免这样的更改，除非父子组件在设计上本来就需要紧密耦合。在大多数场景下，子组件应该抛出一个事件来通知父组件做出改变。

## Prop 校验
Vue 组件可以更细致地声明对传入的 props 的校验要求。比如我们上面已经看到过的类型声明，如果传入的值不满足类型要求，Vue 会在浏览器控制台中抛出警告来提醒使用者。这在开发给其他开发者使用的组件时非常有用。

一些补充细节：

- 所有 prop 默认都是可选的，除非声明了 required: true。
- 除 Boolean 外的未传递的可选 prop 将会有一个**默认值 undefined**。
- Boolean 类型的未传递 prop 将被转换为 false。这可以通过为它设置 default 来更改——例如：设置为 default: undefined 将与非布尔类型的 prop 的行为保持一致。
- 如果声明了 default 值，那么在 prop 的值被解析为 undefined 时，无论 prop 是未被传递还是显式指明的 undefined，都会改为 default 值。
- 当 prop 的校验失败后，Vue 会抛出一个控制台警告 (在开发模式下)。

# 组件事件
## 触发与监听事件
在组件的模板表达式中，可以直接使用 $emit 方法触发自定义事件 (例如：在 v-on 的处理函数中)：
```js
<!-- MyComponent -->
<button @click="$emit('someEvent')">Click Me</button>
```
$emit() 方法在组件实例上也同样以 this.$emit() 的形式可用：
```js
export default {
  methods: {
    submit() {
      this.$emit('someEvent')
    }
  }
}
```
父组件可以通过 v-on (缩写为 @) 来监听事件：
```js
<MyComponent @some-event="callback" />
```
同样，组件的事件监听器也支持 .once 修饰符：
```js
<MyComponent @some-event.once="callback" />
```
像组件与 prop 一样，事件的名字也提供了自动的格式转换。注意这里我们触发了一个以 camelCase 形式命名的事件，但在父组件中可以使用 kebab-case 形式来监听。与 prop 大小写格式一样，在模板中我们也推荐使用 kebab-case 形式来编写监听器。

和原生 DOM 事件不一样，组件触发的事件没有冒泡机制。你只能监听直接子组件触发的事件。平级组件或是跨越多层嵌套的组件间通信，应使用一个外部的事件总线，或是使用一个全局状态管理方案。

## 事件参数
有时候我们会需要在触发事件时附带一个特定的值。举例来说，我们想要``` <BlogPost> ```组件来管理文本会缩放得多大。在这个场景下，我们可以给 $emit 提供一个额外的参数：
```js
<button @click="$emit('increaseBy', 1)">
  Increase by 1
</button>
```
然后我们在父组件中监听事件，我们可以先简单写一个内联的箭头函数作为监听器，此函数会接收到事件附带的参数：
```js
<MyButton @increase-by="(n) => count += n" />
```
或者，也可以用一个组件方法来作为事件处理函数：
```js
<MyButton @increase-by="increaseCount" />
```
该方法也会接收到事件所传递的参数：

```js
methods: {
  increaseCount(n) {
    this.count += n
  }
}
```
## 声明触发的事件
组件可以显式地通过 emits 选项来声明它要触发的事件：
```js
export default {
  emits: ['inFocus', 'submit']
}
```
这个 emits 选项和 defineEmits() 宏还支持对象语法。通过 TypeScript 为参数指定类型，它允许我们对触发事件的参数进行验证：
```js
export default {
  emits: {
    submit(payload: { email: string, password: string }) {
      // 通过返回值为 `true` 还是为 `false` 来判断
      // 验证是否通过
    }
  }
}
```
# 组件 v-model
## 基本用法
v-model 可以在组件上使用以实现双向绑定。

首先让我们回忆一下 v-model 在原生元素上的用法：
```js
<input v-model="searchText" />
```
在代码背后，模板编译器会对 v-model 进行更冗长的等价展开。因此上面的代码其实等价于下面这段：
```js
<input
  :value="searchText"
  @input="searchText = $event.target.value"
/>
```
而当使用在一个组件上时，v-model 会被展开为如下的形式：
```js
<CustomInput
  :model-value="searchText"
  @update:model-value="newValue => searchText = newValue"
/>
```
要让这个例子实际工作起来，```<CustomInput>``` 组件内部需要做两件事：

1. 将内部原生 <input> 元素的 value attribute 绑定到 modelValue prop
2. 当原生的 input 事件触发时，触发一个携带了新值的 update:modelValue 自定义事件
这里是相应的代码：
```js
<!-- CustomInput.vue -->
<script>
export default {
  props: ['modelValue'],
  emits: ['update:modelValue']
}
</script>

<template>
  <input
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
  />
</template>
```
现在 v-model 可以在这个组件上正常工作了：
```
<CustomInput v-model="searchText" />
```
另一种在组件内实现 v-model 的方式是使用一个可写的，同时具有 getter 和 setter 的 computed 属性。get 方法需返回 modelValue prop，而 set 方法需触发相应的事件：
```js
<!-- CustomInput.vue -->
<script>
export default {
  props: ['modelValue'],
  emits: ['update:modelValue'],
  computed: {
    value: {
      get() {
        return this.modelValue
      },
      set(value) {
        this.$emit('update:modelValue', value)
      }
    }
  }
}
</script>

<template>
  <input v-model="value" />
</template>
```
## v-model 的参数
组件上的 v-model 也可以接受一个参数：
```js
<MyComponent v-model:title="bookTitle" />
```
在这种情况下，子组件应该使用 title prop 和 update:title 事件来更新父组件的值，而非默认的 modelValue prop 和 update:modelValue 事件：
```js
<!-- MyComponent.vue -->
<script>
export default {
  props: ['title'],
  emits: ['update:title']
}
</script>

<template>
  <input
    type="text"
    :value="title"
    @input="$emit('update:title', $event.target.value)"
  />
</template>
```
# 透传 Attributes
## Attributes 继承
“透传 attribute”指的是传递给一个组件，却没有被该组件声明为 props 或 emits 的 attribute 或者 v-on 事件监听器。最常见的例子就是 class、style 和 id。

当一个组件以单个元素为根作渲染时，透传的 attribute 会自动被添加到根元素上。举例来说，假如我们有一个 ```<MyButton> ```组件，它的模板长这样：

```js
<!-- <MyButton> 的模板 -->
<button>Click Me</button>
```
一个父组件使用了这个组件，并且传入了 class：
```js
<MyButton class="large" />
```
最后渲染出的 DOM 结果是：
```js
<button class="large">Click Me</button>
```
这里，```<MyButton>``` 并没有将 class 声明为一个它所接受的 prop，所以 class 被视作透传 attribute，自动透传到了``` <MyButton> ```的根元素上。

对 class 和 style 的合并
如果一个子组件的根元素已经有了 class 或 style attribute，它会和从父组件上继承的值合并。如果我们将之前的 ```<MyButton>``` 组件的模板改成这样：
```js
<!-- <MyButton> 的模板 -->
<button class="btn">Click Me</button>
```
则最后渲染出的 DOM 结果会变成：
```js
<button class="btn large">Click Me</button>
```
## v-on 监听器继承
同样的规则也适用于 v-on 事件监听器：
```js
<MyButton @click="onClick" />
```
click 监听器会被添加到``` <MyButton>``` 的根元素，即那个原生的 ```<button>``` 元素之上。当原生的 ```<button>``` 被点击，会触发父组件的 onClick 方法。同样的，如果原生 button 元素自身也通过 v-on 绑定了一个事件监听器，则这个监听器和从父组件继承的监听器都会被触发。

## 禁用 Attributes 继承
如果不想要一个组件自动地继承 attribute，可以在组件选项中设置 inheritAttrs: false。

最常见的需要禁用 attribute 继承的场景就是 attribute 需要应用在根节点以外的其他元素上。通过设置 inheritAttrs 选项为 false，可以完全控制透传进来的 attribute 被如何使用。

这些透传进来的 attribute 可以在模板的表达式中直接用 $attrs 访问到。

```js
<span>Fallthrough attribute: {{ $attrs }}</span>
```
这个 $attrs 对象包含了除组件所声明的 props 和 emits 之外的所有其他 attribute，例如 class，style，v-on 监听器等等。

有几点需要注意：

- 和 props 有所不同，透传 attributes 在 JavaScript 中保留了它们原始的大小写，所以像 foo-bar 这样的一个 attribute 需要通过 ```$attrs['foo-bar']``` 来访问。
- 像 @click 这样的一个 v-on 事件监听器将在此对象下被暴露为一个函数 $attrs.onClick。
再次使用一下之前小节中的 ```<MyButton>``` 组件例子。有时候我们可能为了样式，需要在 ```<button>``` 元素外包装一层 ```<div>```：
```js
<div class="btn-wrapper">
  <button class="btn">Click Me</button>
</div>
```
我们想要所有像 class 和 v-on 监听器这样的透传 attribute 都应用在内部的 ```<button>``` 上而不是外层的``` <div>``` 上。我们可以通过设定 inheritAttrs: false 和使用 v-bind="$attrs" 来实现：
```js
<div class="btn-wrapper">
  <button class="btn" v-bind="$attrs">Click Me</button>
</div>
```
小提示：没有参数的 v-bind 会将一个对象的所有属性都作为 attribute 应用到目标元素上。

# 插槽 Slots
## 插槽内容与出口
在之前的章节中，我们已经了解到组件能够接收任意类型的 JavaScript 值作为 props，但组件要如何接收模板内容呢？在某些场景中，我们可能想要为**子组件传递一些模板片段**，让子组件在它们的组件中渲染这些片段。

举例来说，这里有一个``` <FancyButton>``` 组件，可以像这样使用：
```js
<FancyButton>
  Click me! <!-- 插槽内容 -->
</FancyButton>
```
而 <FancyButton> 的模板是这样的：
```js
<button class="fancy-btn">
  <slot></slot> <!-- 插槽出口 -->
</button>
```
```<slot> ```元素是一个插槽出口 (slot outlet)，标示了父元素提供的插槽内容 (slot content) 将在哪里被渲染。

![插槽图示](images/image4.png)

最终渲染出的 DOM 是这样：
```js
<button class="fancy-btn">Click me!</button>
```
通过使用插槽，```<FancyButton>``` 仅负责渲染外层的 ```<button>``` (以及相应的样式)，而其内部的内容由父组件提供。

理解插槽的另一种方式是和下面的 JavaScript 函数作类比，其概念是类似的：
```js
// 父元素传入插槽内容
FancyButton('Click me!')

// FancyButton 在自己的模板中渲染插槽内容
function FancyButton(slotContent) {
  return `<button class="fancy-btn">
      ${slotContent}
    </button>`
}
```
插槽内容可以是任意合法的模板内容，不局限于文本。例如我们可以传入多个元素，甚至是组件：
```js
<FancyButton>
  <span style="color:red">Click me!</span>
  <AwesomeIcon name="plus" />
</FancyButton>
```
通过使用插槽，```<FancyButton>``` 组件更加灵活和具有可复用性。现在组件可以用在不同的地方渲染各异的内容，但同时还保证都具有相同的样式。

## 渲染作用域
插槽内容可以访问到父组件的数据作用域，因为插槽内容本身是在父组件模板中定义的。举例来说：
```
<span>{{ message }}</span>
<FancyButton>{{ message }}</FancyButton>
```
这里的两个``` {{ message }}``` 插值表达式渲染的内容都是一样的。

插槽内容**无法访问**子组件的数据。**Vue 模板中的表达式只能访问其定义时所处的作用域**，这和 JavaScript 的词法作用域规则是一致的。换言之：

父组件模板中的表达式只能访问父组件的作用域；子组件模板中的表达式只能访问子组件的作用域。

## 默认内容
在外部没有提供任何内容的情况下，可以为插槽指定默认内容。比如有这样一个 <SubmitButton> 组件：

```
<button type="submit">
  <slot></slot>
</button>
```
如果我们想在父组件没有提供任何插槽内容时在``` <button>``` 内渲染“Submit”，只需要将“Submit”写在``` <slot> ```标签之间来作为默认内容：
```
<button type="submit">
  <slot>
    Submit <!-- 默认内容 -->
  </slot>
</button>
```
现在，当我们在父组件中使用``` <SubmitButton>``` 且没有提供任何插槽内容时：
```js
<SubmitButton />
```
“Submit”将会被作为默认内容渲染：
```js
<button type="submit">Submit</button>
```
但如果我们提供了插槽内容：
```js
<SubmitButton>Save</SubmitButton>
```
那么被显式提供的内容会取代默认内容：
```js
<button type="submit">Save</button>
```
## 具名插槽
有时在一个组件中包含多个插槽出口是很有用的。举例来说，在一个 ```<BaseLayout> ```组件中，有如下模板：
```
<div class="container">
  <header>
    <!-- 标题内容放这里 -->
  </header>
  <main>
    <!-- 主要内容放这里 -->
  </main>
  <footer>
    <!-- 底部内容放这里 -->
  </footer>
</div>
```
对于这种场景，```<slot> ```元素可以有一个特殊的 attribute name，用来给各个插槽分配唯一的 ID，以确定每一处要渲染的内容：
```js
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```
这类带 name 的插槽被称为具名插槽 (named slots)。没有提供 name 的 ```<slot> ```出口会隐式地命名为“default”。

在父组件中使用``` <BaseLayout>``` 时，我们需要一种方式将多个插槽内容传入到各自目标插槽的出口。此时就需要用到具名插槽了：

要为具名插槽传入内容，我们需要使用一个含 v-slot 指令的``` <template> ```元素，并将目标插槽的名字传给该指令：
```
<BaseLayout>
  <template v-slot:header>
    <!-- header 插槽的内容放这里 -->
  </template>
</BaseLayout>
```
v-slot 有对应的简写 #，因此 ```<template v-slot:header>``` 可以简写为``` <template #header>```。其意思就是“将这部分模板片段传入子组件的 header 插槽中”。

![具名插槽图示](iamges/image5.png)

下面我们给出完整的、向``` <BaseLayout> ```传递插槽内容的代码，指令均使用的是缩写形式：

```js
<BaseLayout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>

  <template #default>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>

  <template #footer>
    <p>Here's some contact info</p>
  </template>
</BaseLayout>
```
当一个组件同时接收默认插槽和具名插槽时，所有位于顶级的非 ```<template>``` 节点都被隐式地视为默认插槽的内容。所以上面也可以写成：

```js
<BaseLayout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>

  <!-- 隐式的默认插槽 -->
  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template #footer>
    <p>Here's some contact info</p>
  </template>
</BaseLayout>
```
现在 ```<template>``` 元素中的所有内容都将被传递到相应的插槽。最终渲染出的 HTML 如下：
```js
<div class="container">
  <header>
    <h1>Here might be a page title</h1>
  </header>
  <main>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </main>
  <footer>
    <p>Here's some contact info</p>
  </footer>
</div>
```
## 作用域插槽
在上面的渲染作用域中，插槽的内容无法访问到子组件的状态。

然而在某些场景下插槽的内容可能想要同时使用父组件域内和子组件域内的数据。要做到这一点，我们需要一种方法来让子组件在渲染时将一部分数据提供给插槽。

可以像对组件传递 props 那样，向一个插槽的出口上传递 attributes：
```js
<!-- <MyComponent> 的模板 -->
<div>
  <slot :text="greetingMessage" :count="1"></slot>
</div>
```
当需要接收插槽 props 时，默认插槽和具名插槽的使用方式有一些小区别。下面我们将先展示默认插槽如何接受 props，通过子组件标签上的 v-slot 指令，直接接收到了一个插槽 props 对象：
```js
<MyComponent v-slot="slotProps">
  {{ slotProps.text }} {{ slotProps.count }}
</MyComponent>
```

![scoped slots diagram](images/image6.png) 


子组件传入插槽的 props 作为了 v-slot 指令的值，可以在插槽内的表达式中访问。

你可以将作用域插槽类比为一个传入子组件的函数。子组件会将相应的 props 作为参数传给它：
```js
MyComponent({
  // 类比默认插槽，将其想成一个函数
  default: (slotProps) => {
    return `${slotProps.text} ${slotProps.count}`
  }
})

function MyComponent(slots) {
  const greetingMessage = 'hello'
  return `<div>${
    // 在插槽函数调用时传入 props
    slots.default({ text: greetingMessage, count: 1 })
  }</div>`
}
```
实际上，这已经和作用域插槽的最终代码编译结果、以及手动编写[渲染函数](https://cn.vuejs.org/guide/extras/render-function)时使用作用域插槽的方式非常类似了。

v-slot="slotProps" 可以类比这里的函数签名，和函数的参数类似，我们也可以在 v-slot 中使用解构：
```js
<MyComponent v-slot="{ text, count }">
  {{ text }} {{ count }}
</MyComponent>
```
## 具名作用域插槽
具名作用域插槽的工作方式也是类似的，插槽 props 可以作为 v-slot 指令的值被访问到：v-slot:name="slotProps"。当使用缩写时是这样：
```js
<MyComponent>
  <template #header="headerProps">
    {{ headerProps }}
  </template>

  <template #default="defaultProps">
    {{ defaultProps }}
  </template>

  <template #footer="footerProps">
    {{ footerProps }}
  </template>
</MyComponent>
```

向具名插槽中传入 props：
```js
<slot name="header" message="hello"></slot>
```
注意插槽上的 name 是一个 Vue 特别保留的 attribute，不会作为 props 传递给插槽。因此最终 headerProps 的结果是 { message: 'hello' }。

如果你同时使用了具名插槽与默认插槽，则需要为默认插槽使用**显式的 ```<template>```** 标签。尝试直接为组件添加 v-slot 指令将导致编译错误。这是为了避免因默认插槽的 props 的作用域而困惑。举例：
```js
<!-- <MyComponent> template -->
<div>
  <slot :message="hello"></slot>
  <slot name="footer" />
</div>

<!-- 该模板无法编译 -->
<MyComponent v-slot="{ message }">
  <p>{{ message }}</p>
  <template #footer>
    <!-- message 属于默认插槽，此处不可用 -->
    <p>{{ message }}</p>
  </template>
</MyComponent>
```
为默认插槽使用显式的 ```<template>``` 标签有助于更清晰地指出 message 属性在其他插槽中不可用：
```js
<MyComponent>
  <!-- 使用显式的默认插槽 -->
  <template #default="{ message }">
    <p>{{ message }}</p>
  </template>

  <template #footer>
    <p>Here's some contact info</p>
  </template>
</MyComponent>
```
# 依赖注入
## Prop 逐级透传问题
通常情况下，当我们需要从父组件向子组件传递数据时，会使用 props。想象一下这样的结构：有一些多层级嵌套的组件，形成了一棵巨大的组件树，而某个深层的子组件需要一个较远的祖先组件中的部分数据。在这种情况下，如果仅使用 props 则必须将其沿着组件链逐级传递下去，这会非常麻烦：

![Prop 逐级透传的过程图示](images/image7.png)


注意，虽然这里的 ```<Footer>``` 组件可能根本不关心这些 props，但为了使``` <DeepChild>``` 能访问到它们，仍然需要定义并向下传递。如果组件链路非常长，可能会影响到更多这条路上的组件。这一问题被称为“prop 逐级透传”，显然是我们希望尽量避免的情况。

provide 和 inject 可以帮助我们解决这一问题 [1](https://cn.vuejs.org/guide/components/provide-inject#footnote-1)。一个父组件相对于其所有的后代组件，会作为**依赖提供者**。任何后代的组件树，无论层级有多深，都可以**注入**由父组件提供给整条链路的依赖。

![Provide/inject 模式](images/image8.png)


## Provide (提供)
要为组件后代提供数据，需要使用到 provide 选项：
```js
export default {
  provide: {
    message: 'hello!'
  }
}
```
对于 provide 对象上的每一个属性，后代组件会用其 key 为注入名查找期望注入的值，属性的值就是要提供的数据。

如果我们需要提供依赖当前组件实例的状态 (比如那些由 data() 定义的数据属性)，那么可以以函数形式使用 provide：
```js
export default {
  data() {
    return {
      message: 'hello!'
    }
  },
  provide() {
    // 使用函数的形式，可以访问到 `this`
    return {
      message: this.message
    }
  }
}
```
然而，请注意这**不会**使注入保持响应性。

# 组合式函数
## 什么是“组合式函数”？
在 Vue 应用的概念中，“组合式函数”(Composables) 是一个利用 Vue 的组合式 API 来封装和复用**有状态逻辑**的函数。

当构建前端应用时，我们常常需要复用公共任务的逻辑。例如为了在不同地方格式化时间，我们可能会抽取一个可复用的日期格式化函数。这个函数封装了**无状态的逻辑**：它在接收一些输入后立刻返回所期望的输出。复用无状态逻辑的库有很多，比如你可能已经用过的 [lodash](https://lodash.com/)或是 [date-fns](https://date-fns.org/)。

相比之下，有状态逻辑负责管理会随时间而变化的状态。一个简单的例子是跟踪当前鼠标在页面中的位置。在实际应用中，也可能是像触摸手势或与数据库的连接状态这样的更复杂的逻辑。

## 鼠标跟踪器示例
如果我们要直接在组件中使用组合式 API 实现鼠标跟踪功能，它会是这样的：
```js
<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const x = ref(0)
const y = ref(0)

function update(event) {
  x.value = event.pageX
  y.value = event.pageY
}

onMounted(() => window.addEventListener('mousemove', update))
onUnmounted(() => window.removeEventListener('mousemove', update))
</script>

<template>Mouse position is at: {{ x }}, {{ y }}</template>
```
但是，如果我们想在多个组件中复用这个相同的逻辑呢？我们可以把这个逻辑以一个组合式函数的形式提取到外部文件中：
```js
// mouse.js
import { ref, onMounted, onUnmounted } from 'vue'

// 按照惯例，组合式函数名以“use”开头
export function useMouse() {
  // 被组合式函数封装和管理的状态
  const x = ref(0)
  const y = ref(0)

  // 组合式函数可以随时更改其状态。
  function update(event) {
    x.value = event.pageX
    y.value = event.pageY
  }

  // 一个组合式函数也可以挂靠在所属组件的生命周期上
  // 来启动和卸载副作用
  onMounted(() => window.addEventListener('mousemove', update))
  onUnmounted(() => window.removeEventListener('mousemove', update))

  // 通过返回值暴露所管理的状态
  return { x, y }
}
```
下面是它在组件中使用的方式：
```js
<script setup>
import { useMouse } from './mouse.js'

const { x, y } = useMouse()
</script>

<template>Mouse position is at: {{ x }}, {{ y }}</template>
```
# 单文件组件
## 介绍
Vue 的单文件组件 (即 *.vue 文件，英文 Single-File Component，简称 **SFC**) 是一种特殊的文件格式，使我们能够将一个 Vue 组件的模板、逻辑与样式封装在单个文件中。下面是一个单文件组件的示例：
```js
<script>
export default {
  data() {
    return {
      greeting: 'Hello World!'
    }
  }
}
</script>

<template>
  <p class="greeting">{{ greeting }}</p>
</template>

<style>
.greeting {
  color: red;
  font-weight: bold;
}
</style>
```
Vue 的单文件组件是网页开发中 HTML、CSS 和 JavaScript 三种语言经典组合的自然延伸。```<template>、<script> 和 <style>``` 三个块在同一个文件中封装、组合了组件的视图、逻辑和样式。完整的语法定义可以查阅[单文件组件语法说明](https://cn.vuejs.org/api/sfc-spec)。

## 为什么要使用单文件组件
使用单文件组件必须使用构建工具，但作为回报带来了以下优点：

- 使用熟悉的 HTML、CSS 和 JavaScript 语法编写模块化的组件
- [让本来就强相关的关注点自然内聚](https://cn.vuejs.org/guide/scaling-up/sfc#what-about-separation-of-concerns)
- 预编译模板，避免运行时的编译开销
- [组件作用域的 CSS](https://cn.vuejs.org/api/sfc-css-features)
- [在使用组合式 API 时语法更简单](https://cn.vuejs.org/api/sfc-script-setup)
- 通过交叉分析模板和逻辑代码能进行更多编译时优化
- [更好的 IDE 支持](https://cn.vuejs.org/guide/scaling-up/tooling#ide-support)，提供自动补全和对模板中表达式的类型检查
- 开箱即用的模块热更新 (HMR) 支持
单文件组件是 Vue 框架提供的一个功能，并且在下列场景中都是官方推荐的项目组织方式：

- 单页面应用 (SPA)
- 静态站点生成 (SSG)
- 任何值得引入构建步骤以获得更好的开发体验 (DX) 的项目
当然，在一些轻量级场景下使用单文件组件会显得有些杀鸡用牛刀。因此 Vue 同样也可以在无构建步骤的情况下以纯 JavaScript 方式使用。

## 单文件组件是如何工作的
Vue 单文件组件是一个框架指定的文件格式，因此必须交由 @vue/compiler-sfc 编译为标准的 JavaScript 和 CSS，一个编译后的单文件组件是一个标准的 JavaScript(ES) 模块，这也意味着在构建配置正确的前提下，你可以像导入其他 ES 模块一样导入单文件组件：
```js
import MyComponent from './MyComponent.vue'

export default {
  components: {
    MyComponent
  }
}
```
单文件组件中的``` <style> ```标签一般会在开发时注入成原生的``` <style>``` 标签以支持热更新，而生产环境下它们会被抽取、合并成单独的 CSS 文件。

# 工具链
## 项目脚手架
### Vite
Vite 是一个轻量级的、速度极快的构建工具，对 Vue 单文件组件提供第一优先级支持。

要使用 Vite 来创建一个 Vue 项目，非常简单：

```js
$ npm create vue@latest
```

这个命令会安装和执行 create-vue，它是 Vue 提供的官方脚手架工具。跟随命令行的提示继续操作即可。

- 要学习更多关于 Vite 的知识，请查看 Vite 官方文档。
- 若要了解如何为一个 Vite 项目配置 Vue 相关的特殊行为，比如向 Vue 编译器传递相关选项，请查看 @vitejs/plugin-vue 的文档。
上面提到的两种在线演练场也支持将文件作为一个 Vite 项目下载。

### Vue CLI
Vue CLI 是官方提供的基于 Webpack 的 Vue 工具链，它现在处于维护模式。我们建议使用 Vite 开始新的项目，除非你依赖特定的 Webpack 的特性。在大多数情况下，Vite 将提供更优秀的开发体验。

关于从 Vue CLI 迁移到 Vite 的资源：

- VueSchool.io 的 Vue CLI -> Vite 迁移指南
- 迁移支持工具 / 插件
  
### 浏览器内模板编译注意事项
当以无构建步骤方式使用 Vue 时，组件模板要么是写在页面的 HTML 中，要么是内联的 JavaScript 字符串。在这些场景中，为了执行动态模板编译，Vue 需要将模板编译器运行在浏览器中。相对的，如果我们使用了构建步骤，由于提前编译了模板，那么就无须再在浏览器中运行了。为了减小打包出的客户端代码体积，Vue 提供了多种格式的“构建文件”以适配不同场景下的优化需求。

- 前缀为 vue.runtime.* 的文件是只包含运行时的版本：不包含编译器，当使用这个版本时，所有的模板都必须由构建步骤预先编译。
- 名称中不包含 .runtime 的文件则是完全版：即包含了编译器，并支持在浏览器中直接编译模板。然而，体积也会因此增长大约 14kb。
默认的工具链中都会使用仅含运行时的版本，因为所有单文件组件中的模板都已经被预编译了。如果因为某些原因，在有构建步骤时，你仍需要浏览器内的模板编译，你可以更改构建工具配置，将 vue 改为相应的版本``` vue/dist/vue.esm-bundler.js```。

# 路由
## 客户端 vs. 服务端路由
服务端路由指的是服务器根据**用户访问的 URL 路径返回不同的响应结果**。当我们在一个传统的服务端渲染的 web 应用中点击一个链接时，浏览器会从服务端获得全新的 HTML，然后重新加载整个页面。

然而，在单页面应用中，客户端的 JavaScript 可以拦截页面的跳转请求，动态获取新的数据，然后**在无需重新加载的情况下更新当前页面**。这样通常可以带来更顺滑的用户体验，尤其是在更偏向“应用”的场景下，因为这类场景下用户通常会在很长的一段时间中做出多次交互。

在这类单页应用中，“路由”是在**客户端**执行的。一个客户端路由器的职责就是利用诸如 History API 或是 hashchange 事件这样的浏览器 API 来管理应用当前应该渲染的视图。

# 状态管理
## 什么是状态管理？
理论上来说，每一个 Vue 组件实例都已经在“管理”它自己的响应式状态了。我们以一个简单的计数器组件为例：
```js
<script>
export default {
  // 状态
  data() {
    return {
      count: 0
    }
  },
  // 动作
  methods: {
    increment() {
      this.count++
    }
  }
}
</script>

<!-- 视图 -->
<template>{{ count }}</template>
```
它是一个独立的单元，由以下几个部分组成：

- 状态：驱动整个应用的数据源；
- 视图：对状态的一种声明式映射；
- 交互：状态根据用户在视图中的输入而作出相应变更的可能方式。
下面是“单向数据流”这一概念的简单图示：

![state flow diagram](images/image9.png)

然而，当我们有**多个组件共享一个共同的状态**时，就没有这么简单了：

多个视图可能都依赖于同一份状态。
来自不同视图的交互也可能需要更改同一份状态。
对于情景 1，一个可行的办法是将共享状态“提升”到共同的祖先组件上去，再通过 props 传递下来。然而在深层次的组件树结构中这么做的话，很快就会使得代码变得繁琐冗长。这会导致另一个问题：[Prop 逐级透传问题](https://cn.vuejs.org/guide/components/props.html#prop-%E5%8F%82%E6%85%A2%E9%97%AE%E9%A2%98)。

对于情景 2，我们经常发现自己会直接通过模板引用获取父/子实例，或者通过触发的事件尝试改变和同步多个状态的副本。但这些模式的健壮性都不甚理想，很容易就会导致代码难以维护。

一个更简单直接的解决方案是抽取出组件间的共享状态，放在一个全局单例中来管理。这样我们的组件树就变成了一个大的“视图”，而任何位置上的组件都可以访问其中的状态或触发动作。

## 用响应式 API 做简单状态管理
在选项式 API 中，响应式数据是用 data() 选项声明的。在内部，data() 的返回值对象会通过 reactive() 这个公开的 API 函数转为响应式。

如果你有一部分状态需要在多个组件实例间共享，你可以使用 reactive() 来创建一个响应式对象，并将它导入到多个组件中：
```js
// store.js
import { reactive } from 'vue'

export const store = reactive({
  count: 0
})

<!-- ComponentA.vue -->
<script>
import { store } from './store.js'

export default {
  data() {
    return {
      store
    }
  }
}
</script>

<template>From A: {{ store.count }}</template>

<!-- ComponentB.vue -->
<script>
import { store } from './store.js'

export default {
  data() {
    return {
      store
    }
  }
}
</script>

<template>From B: {{ store.count }}</template>
```
现在每当 store 对象被更改时，```<ComponentA>``` 与``` <ComponentB> ```都会自动更新它们的视图。现在我们有了单一的数据源。

然而，这也意味着任意一个导入了 store 的组件都可以随意修改它的状态：
```js
<template>
  <button @click="store.count++">
    From B: {{ store.count }}
  </button>
</template>
```
虽然这在简单的情况下是可行的，但从长远来看，可以被任何组件任意改变的全局状态是不太容易维护的。为了确保改变状态的逻辑像状态本身一样集中，建议在 store 上定义方法，方法的名称应该要能表达出行动的意图：
```js
// store.js
import { reactive } from 'vue'

export const store = reactive({
  count: 0,
  increment() {
    this.count++
  }
})
```
```js
<template>
  <button @click="store.increment()">
    From B: {{ store.count }}
  </button>
</template>
```
# 服务端渲染 (SSR)
## 总览
### 什么是 SSR？
Vue.js 是一个用于构建客户端应用的框架。默认情况下，Vue 组件的职责是在浏览器中生成和操作 DOM。然而，Vue 也支持将组件在服务端直接渲染成 HTML 字符串，作为服务端响应返回给浏览器，最后在浏览器端将静态的 HTML“激活”(hydrate) 为能够交互的客户端应用。

一个由服务端渲染的 Vue.js 应用也可以被认为是“同构的”(Isomorphic) 或“通用的”(Universal)，因为应用的大部分代码同时运行在服务端和客户端。

### 为什么要用 SSR？
与客户端的单页应用 (SPA) 相比，SSR 的优势主要在于：

- 更快的首屏加载：这一点在慢网速或者运行缓慢的设备上尤为重要。服务端渲染的 HTML 无需等到所有的 - JavaScript 都下载并执行完成之后才显示，所以你的用户将会更快地看到完整渲染的页面。除此之外，数据获取过-程在首次访问时在服务端完成，相比于从客户端获取，可能有更快的数据库连接。这通常可以带来更高的核心 Web 指标评分、更好的用户体验，而对于那些“首屏加载速度与转化率直接相关”的应用来说，这点可能至关重要。
- 统一的心智模型：你可以使用相同的语言以及相同的声明式、面向组件的心智模型来开发整个应用，而不需要在后端模板系统和前端框架之间来回切换。
- 更好的 SEO：搜索引擎爬虫可以直接看到完全渲染的页面。
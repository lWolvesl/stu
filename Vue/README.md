# VUE

[回到主页](../README.md)

- [1、起步](#1%E8%B5%B7%E6%AD%A5)
  - [1.1 安装](#11-%E5%AE%89%E8%A3%85)
  - [1.2 基础文件结构](#12-%E5%9F%BA%E7%A1%80%E6%96%87%E4%BB%B6%E7%BB%93%E6%9E%84)
- [2、基础](#2%E5%9F%BA%E7%A1%80)
  - [2.1 实例](#21-%E5%AE%9E%E4%BE%8B)
    - [2.1.1 DOM 中的根组件模板](#211-dom-%E4%B8%AD%E7%9A%84%E6%A0%B9%E7%BB%84%E4%BB%B6%E6%A8%A1%E6%9D%BF)
    - [2.1.2 应用配置](#212-%E5%BA%94%E7%94%A8%E9%85%8D%E7%BD%AE)
  - [2.2 基础模版语法](#22-%E5%9F%BA%E7%A1%80%E6%A8%A1%E7%89%88%E8%AF%AD%E6%B3%95)
    - [2.2.1 文本插值](#221-%E6%96%87%E6%9C%AC%E6%8F%92%E5%80%BC)
    - [2.2.2 原始HTML](#222-%E5%8E%9F%E5%A7%8Bhtml)
    - [2.2.3 Attribute 绑定](#223-attribute-%E7%BB%91%E5%AE%9A)
      - [简写](#%E7%AE%80%E5%86%99)
      - [布尔形](#%E5%B8%83%E5%B0%94%E5%BD%A2)
      - [绑定多个值](#%E7%BB%91%E5%AE%9A%E5%A4%9A%E4%B8%AA%E5%80%BC)
    - [2.2.4 使用JS表达式](#224-%E4%BD%BF%E7%94%A8js%E8%A1%A8%E8%BE%BE%E5%BC%8F)
      - [仅支持表达式](#%E4%BB%85%E6%94%AF%E6%8C%81%E8%A1%A8%E8%BE%BE%E5%BC%8F)
      - [调动函数](#%E8%B0%83%E5%8A%A8%E5%87%BD%E6%95%B0)
      - [受限的全局访问](#%E5%8F%97%E9%99%90%E7%9A%84%E5%85%A8%E5%B1%80%E8%AE%BF%E9%97%AE)
    - [2.2.5 指令](#225-%E6%8C%87%E4%BB%A4)
      - [修饰符 Modifiers](#%E4%BF%AE%E9%A5%B0%E7%AC%A6-modifiers)
  - [2.2 响应式基础](#22-%E5%93%8D%E5%BA%94%E5%BC%8F%E5%9F%BA%E7%A1%80)

## 1、起步

### 1.1 安装

`start`:[Vue](https://cn.vuejs.org/)

`demo`:[Demo/Vue](https://gitea.wolves.top/demo/vue.git)

[API](https://cn.vuejs.org/api/application.html)

### 1.2 基础文件结构

![image-20230401225136029](http://cos.wolves.top/img/202304012251105_repeat_1680360696294__935701.png)

- `vite.config.js`

```js
import { fileURLToPath, URL } from 'node:url'

import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  }
})

```

这段代码是一个 Vite 配置文件，用于配置 Vue 3 项目的构建和运行环境。具体来说，它做了以下几件事情：

- 引入了 `node:url` 模块中的 `fileURLToPath` 和 `URL` 方法；
- 引入了 `vite` 和 `vue` 模块中的方法；
- 定义了一个默认的导出对象，其中包含了 `plugins` 和 `resolve` 两个属性；
- `plugins` 属性是 Vite 插件数组，这里只包含了 Vue.js 官方提供的 `@vitejs/plugin-vue` 插件；
- `resolve` 属性是一个用于配置模块解析器的对象，这里使用 `alias` 属性将 `@` 符号映射为 `'./src'` 目录的绝对路径，这样就可以使用 `@/components` 的形式引入组件，而无需写成 `../../components` 等相对路径。

在 `alias` 属性中，`new URL('./src', import.meta.url)` 构造了一个指向 `./src` 目录的完整 URL 对象，然后通过 `fileURLToPath` 将其转化为该目录的本地文件路径，再将其作为 `@` 的别名。

- `main.js`

```js
import { createApp } from 'vue'
import App from './App.vue'

import './assets/main.css'

createApp(App).mount('#app')
```

这段代码使用了 Vue 3 的模块化方式，是`vue`的组件，通过引入 `createApp` 和 `App` 组件的方式创建了一个 Vue 应用，并将其挂载到 id 为 `app` 的 DOM 元素上。

具体来说，它做了以下几件事：

- 引入了 `createApp` 函数和 `App.vue` 组件；

- 引入了 `main.css` 样式文件；

- 使用 `createApp(App)` 创建了一个 Vue 应用实例；

- 调用 `mount('#app')` 方法将应用实例挂载到 id 为 `app` 的 DOM 元素上。这里假设 HTML 中已经有一个 id 为 `app` 的元素。

- `createApp(App).mount('#app')`可改写为

  ```js
  const app = createApp(App)		//创建app
  app.mount('#app')							//绑定到指定元素中
  ```

  
## 2、基础

### 2.1 实例

#### 2.1.1 DOM 中的根组件模板

当在未采用构建流程的情况下使用 `Vue` 时，我们可以在挂载容器中直接书写根组件模板：

```html
<div id="app">
  <button @click="count++">{{ count }}</button>
</div>
```

```js
import { createApp } from 'vue'

const app = createApp({
  data() {
    return {
      count: 0
    }
  }
})

app.mount('#app')
```

当根组件没有设置 `template` 选项时，Vue 将自动使用容器的` innerHTML` 作为模板。

#### 2.1.2 应用配置

应用实例会暴露一个 `.config` 对象允许我们配置一些应用级的选项，例如定义一个应用级的错误处理器，用来捕获所有子组件上的错误：

```js
app.config.errorHandler = (err) => {
  /* 处理错误 */
}
```

应用实例还提供了一些方法来注册应用范围内可用的资源，例如注册一个组件：

```js
app.component('TodoDeleteButton', TodoDeleteButton)
```

这使得 `TodoDeleteButton` 在应用的任何地方都是可用的。我们会在指南的后续章节中讨论关于组件和其他资源的注册。你也可以在 API 参考中浏览应用实例[API参考](https://cn.vuejs.org/api/application.html)的完整列表。

确保在挂载应用实例之前完成所有应用配置！

### 2.2 基础模版语法

```text
Vue 使用一种基于 HTML 的模板语法，使我们能够声明式地将其组件实例的数据绑定到呈现的 DOM 上。所有的 Vue 模板都是语法层面合法的 HTML，可以被符合规范的浏览器和 HTML 解析器解析。

在底层机制中，Vue 会将模板编译成高度优化的 JavaScript 代码。结合响应式系统，当应用状态变更时，Vue 能够智能地推导出需要重新渲染的组件的最少数量，并应用最少的 DOM 操作。

如果你对虚拟 DOM 的概念比较熟悉，并且偏好直接使用 JavaScript，你也可以结合可选的 JSX 支持直接手写渲染函数而不采用模板。但请注意，这将不会享受到和模板同等级别的编译时优化。
```

#### 2.2.1 文本插值

```js
<span>Message: {{ msg }}</span>
```

双大括号标签会被替换为相应组件实例中 msg 属性的值。同时每次 msg 属性更改时它也会同步更新。

#### 2.2.2 原始HTML

双大括号会将数据解释为纯文本，而不是 HTML。若想插入 HTML，你需要使用 `v-html` 指令：

```template
<p>Using text interpolation: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

![image-20230401231040758](http://cos.wolves.top/img/image-20230401231040758_repeat_1680361841014__867434.png)

这里我们遇到了一个新的概念。这里看到的 `v-html` attribute 被称为一个指令。指令由 v- 作为前缀，表明它们是一些由 Vue 提供的特殊 attribute，你可能已经猜到了，它们将为渲染的 DOM 应用特殊的响应式行为。这里我们做的事情简单来说就是：在当前组件实例上，将此元素的` innerHTML `与 `rawHtml `属性保持同步。

`span` 的内容将会被替换为 `rawHtml` 属性的值，插值为纯 HTML——数据绑定将会被忽略。注意，你不能使用` v-html` 来拼接组合模板，因为 Vue 不是一个基于字符串的模板引擎。在使用 Vue 时，应当使用组件作为 UI 重用和组合的基本单元。

```
安全警告

在网站上动态渲染任意 HTML 是非常危险的，因为这非常容易造成 XSS 漏洞。请仅在内容安全可信时再使用 v-html，并且永远不要使用用户提供的 HTML 内容。
```

#### 2.2.3 Attribute 绑定

双大括号不能在 HTML attributes 中使用。想要响应式地绑定一个 attribute，应该使用 `v-bind` 指令：

```html
<div v-bind:id="dynamicId"></div>
```

`v-bind` 指令指示 Vue 将元素的` id` attribute 与组件的 `dynamicId` 属性保持一致。如果绑定的值是 `null` 或者 `undefined`，那么该 `attribute` 将会从渲染的元素上移除。

##### 简写

```html
<div :id="dynamicId"></div>
```

##### 布尔形

布尔型 attribute 依据 `true / false` 值来决定 `attribute `是否应该存在于该元素上。`disabled` 就是最常见的例子之一。

`v-bind` 在这种场景下的行为略有不同：

```html
<button :disabled="isButtonDisabled">Button</button>
```

当` isButtonDisabled` 为真值或一个空字符串 (即 <button disabled="">) 时，元素会包含这个 `disabled `attribute。而当其为其他假值时 attribute 将被忽略。

##### 绑定多个值

```js
data() {
  return {
    objectOfAttrs: {
      id: 'container',
      class: 'wrapper'
    }
  }
}
```

```html
<div v-bind="objectOfAttrs"></div>
```

#### 2.2.4 使用JS表达式

Vue 实际上在所有的数据绑定中都支持完整的 JavaScript 表达式：

```html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div :id="`list-${id}`"></div>
```

在 Vue 模板内，JavaScript 表达式可以被使用在如下场景上：

- 在文本插值中 (双大括号)
- 在任何 Vue 指令 (以 `v-` 开头的特殊 attribute) attribute 的值中

##### 仅支持表达式

每个绑定仅支持单一表达式，也就是一段能够被求值的 JavaScript 代码。一个简单的判断方法是是否可以合法地写在 `return` 后面。

因此，下面的例子都是无效的：

```html
<!-- 这是一个语句，而非表达式 -->
{{ var a = 1 }}

<!-- 条件控制也不支持，请使用三元表达式 -->
{{ if (ok) { return message } }}
```

##### 调动函数

可以在绑定的表达式中使用一个组件暴露的方法：

```html
<span :title="toTitleDate(date)">
  {{ formatDate(date) }}
</span>
```

##### 受限的全局访问

模板中的表达式将被沙盒化，仅能够访问到有限的全局对象列表。该列表中会暴露常用的内置全局对象，比如 `Math` 和 `Date`。

没有显式包含在列表中的全局对象将不能在模板内表达式中访问，例如用户附加在 `window` 上的属性。然而，你也可以自行在 `app.config.globalProperties` 上显式地添加它们，供所有的 Vue 表达式使用。

#### 2.2.5 指令

##### 修饰符 Modifiers

修饰符是以点开头的特殊后缀，表明指令需要以一些特殊的方式被绑定。例如 `.prevent` 修饰符会告知 `v-on `指令对触发的事件调用 `event.preventDefault()：`

```html
<form @submit.prevent="onSubmit">...</form>
```

![image-20230401233429668](http://cos.wolves.top/img/image-20230401233429668.png)

### 2.2 响应式基础

选用选项式 API 时，会用 `data` 选项来声明组件的响应式状态。此选项的值应为返回一个对象的函数。Vue 将在创建新组件实例的时候调用此函数，并将函数返回的对象用响应式系统进行包装。此对象的所有顶层属性都会被代理到组件实例 (即方法和生命周期钩子中的 `this`) 上。

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

## 3、疑难

#### 3.1 Props和data()

Vue提供了两种不同的存储变量：`props`和`data`。

在Vue中，`props`(或properties)是我们将数据从父组件向下传递到其子组件的方式。

当我们使用组件构建应用程序时，最终会构建一个称为树的数据结构。 类似于家谱，具有：

- 父母
- 孩子
- 祖先
- 子孙

数据从根组件(位于最顶端的组件)沿着树向下流动。就像基因是如何代代相传的一样，父组件也会将自己的`props`传给了他们的孩子。

当我们从组件内部访问`props`时，我们并不拥有它们，所以我们不能更改它们(就像你不能改变你父母给你的基因一样)。

props 和 data 都是响应式的

#### 3.2 计算属性`computed`和方法`methods`

简单来说，`computed`中的方法被调用过后，将会将返回值缓存，如果作用域内的值没有发生变化，调用方法不会执行，而是直接返回缓存值

而`methods`内的方法只要被调用就会被执行，无论作用域内的值是否发生变化

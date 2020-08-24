## VUE





<h3>介绍</h3>

vue是一个**渐进式**框架

- 渐进式意味着可以将vue作为应用的一部分嵌入其中
- 也可以使用vue的核心库以及生态系统构建项目



<h4>安装</h4>

1. 直接CDN引入

- 开发环境  包含了有帮助的命令行警告

```html
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

- 生产环境  优化了尺寸和速度

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.11"></script>
```

2. 下载和引入

![img](D:\Git\BLOGS\docs\picture\vue_download.png)

3. NPM安装



<h3>MVVM</h3>

> Model View ViewModel





<h3>基础语法</h3>

<h4>data</h4>

data中写属性



<h4>methods</h4>

定义方法





<h4>v-for</h4>

遍历语法

```vue
    <ul>
      <li v-for="item in movies"> {{item}}</li>
    </ul>
```



<h4>v-on</h4>

监听事件语法

```vue
  <div id="app">
    <h2>当前计数: {{counter}}</h2>
    <button v-on:click="counter++">+</button>
    <button v-on:click="counter--">-</button>
  </div>
```


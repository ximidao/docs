# Headline

> An awesome project.

```markdown
* [En](/)
* [中文](/zh-cn/)
```

_index.html_

```markdown
* [En](/)
* [中文](/zh-cn/)
```

# 多页文档
如果需要创建多个页面，或者需要多级路由的网站，在 docsify 里也能很容易的实现。例如创建一个 guide.md 文件，那么对应的路由就是 `/#/guide`。

假设你的目录结构如下：
```
-| docs/
  -| README.md
  -| guide.md
  -| zh-cn/
    -| README.md
    -| guide.md
```

# 定制侧边栏
默认情况下，侧边栏会根据当前文档的标题生成目录。也可以设置文档链接，通过 Markdown 文件生成，效果如当前的文档的侧边栏。

首先配置 `loadSidebar` 选项，具体配置规则见配置项#loadSidebar。

# 配置项
你可以配置在 `window.$docsify` 里。

```html
<script>
  window.$docsify = {
    repo: 'docsifyjs/docsify',
    maxLevel: 3,
    coverpage: true
  }
</script>
```
## el
类型：`String`
默认值：`#app`
docsify 初始化的挂载元素，可以是一个 CSS 选择器，默认为 #app 如果不存在就直接绑定在 body 上。
```js
window.$docsify = {
  el: '#app'
};
```

# Markdown 配置

内置的 Markdown 解析器是 [marked](https://github.com/markedjs/marked)，可以修改它的配置。同时可以直接配置 `renderer`。

```js
window.$docsify = {
  markdown: {
    smartypants: true,
    renderer: {
      link: function() {
        // ...
      }
    }
  }
}
```

?> 完整配置参数参考 [marked 文档](https://github.com/markedjs/marked#options-1)

# 自定义插件

docsify 提供了一套插件机制，其中提供的钩子（hook）支持处理异步逻辑，可以很方便的扩展功能。

## 完整功能

```js
window.$docsify = {
 plugins: [
  function (hook, vm) {
    hook.init(function() {
      // 初始化时调用，只调用一次，没有参数。
    })

    hook.beforeEach(function(content) {
      // 每次开始解析 Markdown 内容时调用
      // ...
      return content
    })

    hook.afterEach(function(html, next) {
      // 解析成 html 后调用。beforeEach 和 afterEach 支持处理异步逻辑
      // ...
      // 异步处理完成后调用 next(html) 返回结果
      next(html)
    })

    hook.doneEach(function() {
      // 每次路由切换时数据全部加载完成后调用，没有参数。
      // ...
    })

    hook.mounted(function() {
      // 初始化完成后调用 ，只调用一次，没有参数。
    })

    hook.ready(function() {
      // 初始化并第一次加完成数据后调用，没有参数。
    })
  }
 ]
}
```

!> 如果需要用 docsify 的内部方法，可以通过 `window.Docsify` 获取，通过 `vm` 获取当前实例。

## 例子


### footer

给每个页面的末尾加上 `footer`

```js
window.$docsify = {
  plugins: [
    function (hook) {
      var footer = [
        '<hr/>',
        '<footer>',
        '<span><a href="https://github.com/QingWei-Li">cinwell</a> &copy;2017.</span>',
        '<span>Proudly published with <a href="https://github.com/docsifyjs/docsify" target="_blank">docsify</a>.</span>',
        '</footer>'
      ].join('')

      hook.afterEach(function (html) {
        return html + footer
      })
    }
  ]
}
```


### Edit Button

```js
window.$docsify = {
  plugins: [
    function(hook, vm) {
      hook.beforeEach(function (html) {
        var url = 'https://github.com/docsifyjs/docsify/blob/master/docs' + vm.route.file
        var editHtml = '(' + url + ')\n'

        return editHtml
          + html
          + '\n----\n'
          + 'Last modified {docsify-updated} '
          + editHtml
      })
    }
  ]
}
```
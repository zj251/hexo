---
title: Codemirror使用指南 
top_img: https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=2230569090,239285595&fm=26&gp=0.jpg
cover: https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201808%2F27%2F20180827195215_grcek.jpeg&refer=http%3A%2F%2Fb-ssl.duitang.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1615030181&t=5998569aff324a5d909490b2b31d933a

---

> 在开发vue项目的过程中，需要一个可以自定义规则的代码编辑器，最终选用了`Vue-Codemirror`来进行开发。`Vue-Codemirror`是基于`Codemirror`，并适用于Vue的代码编辑器。

 - [官方文档](https://codemirror.net/)
 - npm
 	- [Codemirror](https://www.npmjs.com/package/codemirror)
    - [Vue-Codemirror](https://www.npmjs.com/package/vue-codemirror)
 - [demo](https://github.com/zj251/codemirror-demo)
 ### 功能介绍
 1. 多种主题支持
 2. 多种语言支持（json、yaml、xml、jsx、sql、css、c、c++等）
 3. 语法校验
 4. 代码折叠
 5. 代码比对以及合并
 6. 代码补全
 7. ... 等功能
 
 ---
 
 ### Install
 - npm
 ```
 npm install vue-codemirror --save
 
 ```
 引入
```
import 'codemirror/lib/codemirror.css'
import VueCodemirror from 'vue-codemirror';
```
 -cdn

 ```
 <link rel="stylesheet" href=".../codemirror/5.59.1/lib/codemirror.css">
 
<script type="text/javascript" src=".../codemirror/5.59.1/lib/codemirror.js></script>
<script type="text/javascript" src=".../vue-codemirror/4.0.6/dist/vue-codemirror.js"></script>
 ```
 ---
 ### Mount
 - global
 ```
  import Vue from 'vue';
  import VueCodemirror from 'vue-codemirror';

  Vue.use(VueCodemirror，{...options}）
 ```
 - component
 
```
  import Vue from 'vue';
  import Component from 'vue-class-component';
  import { codemirror } from 'vue-codemirror';
  
   @Component({
        name: 'Demo',
        components: {
            codemirror
        }
    })

```
---

### Create
```
<template>
   <codemirror
      ref="myCm"
      :value="info.content"
      :options="cmOptions"
      @input="changes">
   </codemirror>
</template>


<script>
import Vue from 'vue'
import Component from 'vue-class-component'
import { codemirror } from 'vue-codemirror'

@Component({
  name: 'Demo',
  components: {
    codemirror
  }
})

export default class Demo extends Vue {
  cmOptions = {
    mode: 'application/json', // 模式
    theme: '3024-day', // 主题
    indentUnit: 4, // 缩进多少个空格
    tabSize: 4, // 制表符宽度
    lineNumbers: true, // 是否显示行号
    lineWrapping: true, // 是否默认换行
    firstLineNumber: 3, // 在哪个数字开始计数行。默认值为1
    readOnly: false, // 禁止用户编辑编辑器内容
    line: true,
    smartIndent: true // 智能缩进
  }
}
</script>
    
```
当设置`mode`的时候，需要引入相应`mode.js`。同样的，设置theme的时候，也需要引入相应的`theme.css`。由于`json`属于JavaScript的一种，所以也得引入`JavaScript`。
- cdn
```
<link rel="stylesheet" href=".../codemirror/5.59.1/theme/3024-day.css">
    
<script type="text/javascript" src=".../codemirror/5.59.1/mode/javascript/javascript.js"></script>
```

- npm
```
import 'codemirror/theme/3024-day.css'
import 'codemirror/mode/javascript/javascript.js'
```

如果`mode`需要支持`yaml`,则需要引入`yaml.js`
```
import 'codemirror/mode/yaml/yaml.js'
```
---
### Codemirror lint
json需要引入`json-lint.js`，如果是yaml，则需要引入`yaml-lint.js`。但是，Codemirror支持的语法校验类型比较少，只支持`css、html、javascript、json、yaml、coffeescript`

```
<link rel="stylesheet" href=".../codemirror/5.59.1/addon/lint/lint.css">

<script src=".../codemirror/5.59.1/addon/lint/lint.js"></script>
<script src=".../codemirror/5.59.1/addon/lint/json-lint.js"></script>
<script src="https://unpkg.com/jsonlint@1.6.3/web/jsonlint.js"></script>
```
同时还需要在`options`进行相应配置
```
this.cmOptions = {
    lineNumbers: true,
    mode: "application/json",
    gutters: ["CodeMirror-lint-markers"],
    lint: true
}
```
 
 ![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d83a4cf97aa94e198cba41d98e26cbee~tplv-k3u1fbpfcp-watermark.image)

---

### Codemirror Merge
首先，需要引入`diff_math_patch`，然后引入codemirror相应的`merge.js`以及`merge.css`
```
  <link rel="stylesheet" href=".../codemirror/5.59.1/addon/merge/merge.css">

  <script src="https://cdnjs.cloudflare.com/ajax/libs/diff_match_patch/20121119/diff_match_patch.js"></script>
  <script src=".../codemirror/5.59.1/addon/merge/merge.js"></script>

```
然后，需要去配置options
```
mergeOptions = {
    mode: 'application/json', // 模式
    theme: 'yeti', // 主题
    lineNumbers: true, // 是否显示行号
    origLeft: null,
    showDifferences: true, // 当为true(默认值)时, 更改的文本片段将高亮显示
    collapseIdentical: false, // 当为true(默认为false)时，未修改的文本段将被折叠。
    connect: 'center', // 设置用于连接更改的代码块的样式
    value: '{\n    "k1":"v1",\n    "k2":"v2",\n    "k3":"v3",\n    "k4":"v4",\n    "k5":"v5",\n    "k6":"v6",\n    "k7":"v7"\n}', // 左侧老文件
    orig: '{\n    "k1":"v1",\n    "k3":"v3",\n    "k4":"v4",\n    "k7":"v7"\n}' // 右侧新文件
  }

```
![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cfd2216b8051430686e1f9e30d8aa7cc~tplv-k3u1fbpfcp-watermark.image)

---
### Codemirror Fold
首先，需要去引相关折叠功能的相关包

```
 <link rel="stylesheet" href=".../codemirror/addon/fold/foldgutter.css">
 
<script src=".../codemirror/addon/fold/foldcode.js"></script>
<script src=".../codemirror/addon/fold/foldgutter.js"></script>
<script src=".../codemirror/addon/fold/xml-fold.js"></script>
```

其次，需要去配置`options`
```
foldOptions = {
    mode: 'text/html', // 模式
    theme: 'mdn-like', // 主题
    lineNumbers: true, // 是否显示行号
    lineWrapping: true,
    foldGutter: true, // 支持折叠
    gutters: ['CodeMirror-linenumbers', 'CodeMirror-foldgutter']
 }

```


折叠前的展示

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6ddd27a2718b4e5dac61c661c3ac5b0b~tplv-k3u1fbpfcp-watermark.image)

折叠后的展示

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/17d26e50b0bd45f6a45f0f57fd90772d~tplv-k3u1fbpfcp-watermark.image)

> codemirror具体的一些属性方法，大家有需要了解的可以查阅其相应的官方文档以及github。
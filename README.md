# @jiker/markdown-it  

![](https://img.shields.io/badge/-jiker-brightgreen)
![](https://img.shields.io/npm/dw/@jiker/markdown-it)
![](https://img.shields.io/npm/v/@jiker/markdown-it)
![](https://img.shields.io/npm/l/@jiker/markdown-it)
![](https://img.shields.io/npm/collaborators/@jiker/markdown-it)

> A Javascript Markdown to HTML converter.

## Installation
```
npm install @jiker/markdown-it --save
```  

## Usage examples
引入 `index.js` ：
```js
import MarkdownIt from "@jiker/markdown-it";
import "@jiker/markdown-it/styles/markdown-it.css";
const html = MarkdownIt("## md");
```  

引入 `vue` 组件：
```html
<markdown-it :md="md"></markdown-it>
```  

```js
import MarkdownIt from "@jiker/markdown-it/MarkdownIt.vue";

export default {
  data() {
    return {
      md: "## md"
    };
  },
  components: {
    MarkdownIt
  }
}
```  

## Plugin list  
* highlight.js
* markdown-it
* markdown-it-abbr
* markdown-it-container
* markdown-it-deflist
* markdown-it-emoji
* markdown-it-footnote
* markdown-it-ins
* markdown-it-katex
* markdown-it-mark
* markdown-it-sub
* markdown-it-sup
* markdown-it-task-lists

## License
[MIT](http://git.weilaigongzuo.com/npm/markdown-it/blob/master/LICENSE)

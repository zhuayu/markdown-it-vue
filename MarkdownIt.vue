<template>
  <div class="jiker-markdown-it">
    <div v-html="render(md)"></div>
  </div>
</template>

<script>
import MarkdownIt from "markdown-it";
import hljs from "highlight.js";
import "highlight.js/styles/github.css";
import MarkdownItSuperscript from "markdown-it-sup";
import MarkdownItSubscript from "markdown-it-sub";
import MarkdownItInsert from "markdown-it-ins";
import MarkdownItMark from "markdown-it-mark";
import MarkdownItTasklists from "markdown-it-task-lists";
import MarkdownItFootnote from "markdown-it-footnote";
import MarkdownItEmoji from "markdown-it-emoji";
import MarkdownItContainer from "markdown-it-container";
import MarkdownItDeflist from "markdown-it-deflist";
import MarkdownItAbbreviation from "markdown-it-abbr";
import MarkdownItKatex from "markdown-it-katex";

const optMarkdownIt = {
  html: true
};

const md = new MarkdownIt(optMarkdownIt)
  .use(MarkdownItSuperscript)
  .use(MarkdownItSubscript)
  .use(MarkdownItInsert)
  .use(MarkdownItMark)
  .use(MarkdownItTasklists, { enabled: true })
  .use(MarkdownItFootnote)
  .use(MarkdownItEmoji)
  .use(MarkdownItContainer, "hljs-left")
  .use(MarkdownItContainer, "hljs-center")
  .use(MarkdownItContainer, "hljs-right")
  .use(MarkdownItDeflist)
  .use(MarkdownItAbbreviation)
  .use(MarkdownItKatex);

// 代码高亮
md.renderer.rules.fence = (tokens, idx) => {
  const token = tokens[idx];
  const code = token.content.trim();
  const lang = token.info ? [token.info] : undefined;
  return `<pre><code class="hljs">${
    hljs.highlightAuto(code, lang).value
  }</code></pre>`;
};

// 新窗口打开链接
const defaultRender =
  md.renderer.rules.link_open ||
  function(tokens, idx, options, env, self) {
    return self.renderToken(tokens, idx, options);
  };

md.renderer.rules.link_open = function(tokens, idx, options, env, self) {
  let aIndex = tokens[idx].attrIndex("target");
  if (aIndex < 0) {
    tokens[idx].attrPush(["target", "_blank"]);
  } else {
    tokens[idx].attrs[aIndex][1] = "_blank";
  }
  return defaultRender(tokens, idx, options, env, self);
};

export default {
  props: {
    md: {
      type: String,
      default: ""
    }
  },
  methods: {
    render(val) {
      return md.render(val);
    }
  }
}
</script>

<style lang="less">
  /* KATEX 公式： */
  @import "https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.5.1/katex.min.css";

  .jiker-markdown-it {
    /* 任务列表： */
    .contains-task-list {
      padding: 0 !important;
      .task-list-item {
        list-style: none !important;
      }
    }
    /* 布局： */
    .hljs-center {
      text-align: center;
    }
    .hljs-right {
      text-align: right;
    }
    .hljs-left {
      text-align: left;
    }
    /* 定义型列表： */
    dt {
      font-style: italic;
      font-weight: 600;
    }
  }

  /* 默认： */
  .jiker-markdown-it {
    position: relative;
    z-index: 1;
    padding: 22px 15px 40px;
    min-height: 300px;
    font-size: 16px;
    font-family: arial, sans-serif;
    line-height: 1.6;
    color: #333;
    background: transparent;
    outline: none;
    word-wrap: break-word;
    outline: none;
    cursor: text;
    > :first-child {
      margin-top: 0 !important;
    }
    a {
      color: #4298BA;
      text-decoration: none;
      word-break: break-all;
    }
    a:visited {
      color: #4298BA;
    }
    a:hover {
      color: #0F769F;
    }
    a:active {
      color: #9E792E;
    }
    a:hover,
    a:active {
      outline: 0;
    }
    h1,
    h2,
    h3,
    h4,
    h5,
    h6 {
      font-weight: normal;
      margin: 40px 0 20px;
      color: #000000;
    }
    h1 {
      font-size: 24px;
    }
    h2 {
      font-size: 22px;
    }
    h3 {
      font-size: 20px;
    }
    h4 {
      font-size: 18px;
    }
    h5 {
      font-size: 16px;
    }
    h6 {
      font-size: 16px;
    }
    p,
    div {
      word-wrap: break-word;
      margin: 0 0 15px 0;
      color: #333;
      word-wrap: break-word;
    }
    b,
    strong {
      font-weight: bold;
    }
    i,
    em {
      font-style: italic;
    }
    u {
      text-decoration: underline;
    }
    strike,
    del {
      text-decoration: line-through;
    }
    ul,
    ol {
      list-style: disc outside none;
      margin: 15px 0;
      padding: 0 0 0 40px;
      line-height: 1.6;
      ul,
      ol {
        padding-left: 30px;
      }
      ul {
        list-style: circle outside none;
        ul {
          list-style: square outside none;
        }
      }
    }
    ol {
      list-style: decimal;
    }
    blockquote {
      border-left: 6px solid #ddd;
      padding: 5px 0 5px 10px;
      margin: 15px 0 15px 15px;
      > :first-child {
        margin-top: 0;
      }
    }
    code {
      display: inline-block;
      padding: 0 4px;
      margin: 0 5px;
      background: #eeeeee;
      border-radius: 3px;
      font-size: 13px;
      font-family: 'monaco', 'Consolas', "Liberation Mono", Courier, monospace;
    }
    pre {
      padding: 10px 5px 10px 10px;
      margin: 15px 0;
      display: block;
      line-height: 18px;
      background: #F0F0F0;
      border-radius: 3px;
      font-size: 13px;
      font-family: 'monaco', 'Consolas', "Liberation Mono", Courier, monospace;
      white-space: pre-wrap !important;
      word-wrap: break-word !important;
      overflow-x: auto;
      code {
        display: block;
        padding: 0;
        margin: 0;
        background: none;
        border-radius: 0;
      }
    }
    hr {
      display: block;
      height: 0px;
      border: 0;
      border-top: 1px solid #ccc;
      margin: 15px 0;
      padding: 0;
    }
    table {
      width: 100%;
      table-layout: fixed;
      border-collapse: collapse;
      border-spacing: 0;
      margin: 15px 0;
      thead {
        background-color: #f9f9f9;
      }
      td,
      th {
        min-width: 40px;
        height: 30px;
        border: 1px solid #ccc;
        vertical-align: top;
        padding: 2px 4px;
        text-align: left;
        box-sizing: border-box;
      }
      td.active,
      th.active {
        background-color: #ffffee;
      }
    }
    img {
      margin: 0 5px;
      vertical-align: middle;
      cursor: pointer;
    }
  }
</style>

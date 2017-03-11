<template>
  <div id="app">
    <StyleEditor ref="styleEditor" :code="currentStyle"></StyleEditor>
    <ResumeEditor ref="resumeEditor" :markdown="currentMarkdown" :enableHtml="enableHtml"></ResumeEditor>
  </div>
</template>

<script>
  import StyleEditor from './components/StyleEditor'
  import ResumeEditor from './components/ResumeEditor'
  import './assets/reset.css'

  export default {
  name: 'app',
    components: {
      StyleEditor,
      ResumeEditor
},
data() {
  return {
        interval: 39,
        currentStyle: '',
        enableHtml: false,
        fullStyle: [
          `/*
* 你好,我叫靳肖健.
* 都说找工作金三银四，可是今年的前端届却不一定，找个好工作,难啊。
* 不过！我会努力的！加油！
* 自我介绍马上开启!
* 先开启前端空间 》》》
*/

/* 首先给所有元素加上过渡效果 */
* {
  -webkit-transition: all 1s;
  transition: all 1s;
}
/* 白色背景太单调了，我喜欢蓝色，我们来点背景 */
html {
  color: rgb(222, 222, 222);
  background: rgb(8, 104, 199);
}
/* 文字离边框太近了 */
.styleEditor {
  padding: 2em;
  border: 1px solid;
  margin: 0.5em;
  overflow: auto;
  width: 45vw;
  height: 90vh;
  background: rgba(0,0,0,0.9);
}
/* 代码高亮 */
.token.selector {
  color: rgb(133, 153, 0);
}

.token.property {
  color: rgb(187, 137, 0);
}

.token.punctuation {
  color: rgb(255, 235, 59);
}

.token.function {
  color: rgb(42, 161, 152);
}
/* 接下来我给自己准备一个编辑器 */
.resumeEditor {
  position: fixed;
  right: 0;
  top: 0;
  padding: .5em;
  margin: .5em;
  width: 48vw;
  height: 90vh;
  border: 1px solid;
  background: white;
  color: #222;
  overflow: auto;
}
/* 好了，我开始写简历了 */


`,
          `
/* 写好了,但不美观
 * 用开源工具把 Markdown 翻译成 HTML 
 */
`
          ,
          `
/* 再对 HTML 加点样式 */
.resumeEditor {
  padding: 2em;
  border: 5px solid rgb(103, 58, 183);;
  color: rgb(96, 125, 139);
}

.resumeEditor h2 {
  color: rgb(63, 81, 181);
  display: inline-block;
  border-bottom: 1px solid;
  margin: 1em 0 .5em;
}

.resumeEditor ul,.resumeEditor ol {
  list-style: none;
}

.resumeEditor ul> li::before {
  content: '☆';
  margin-right: .5em;
  color: rgb(255, 152, 0);
}

.resumeEditor ol {
  counter-reset: section;
}

.resumeEditor ol li::before {
  counter-increment: section;
  content: counters(section, ".") ")";
  margin-right: .5em;
  color: rgb(255, 152, 0);
}

.resumeEditor blockquote {
  margin: 1em;
  padding: .5em;
  background: #ddd;
}

a {
  text-decoration: none;
  color: rgb(255, 152, 0);
}

.styleEditor {
  display: none;
}

.resumeEditor {
  position: inherit;
  width: auto;
  height: auto;
  background: rgba(255,255,255,0.9);
}
`],

currentMarkdown: '',
        fullMarkdown: `靳肖健
----

前端工程师

技能
----

* 靠谱的原生能力（html、css、javascript）
* 应用前端开发框架解决的问题能力也不错
  (擅长vue、zepto,还熟悉:jQuery、bootstrap，
  其他常用框架不敢说熟悉，但都接触过)
* 后端能力还可以吧(php、mysql、node)

工作经历
----

1. 西安浪美网络科技服务有限公司（前端）
2. 西安裕日软件有限公司（页面重构）

毕业学校
----

西北工业大学 (985) 本科

链接
----

* [GitHub](http://jxjweb.top/)
* [技术博客](http://webjxj.sc2yun.com/)



`
}
    },
created() {
this.makeResume()
},

methods: {
makeResume: async function () {
        await this.progressivelyShowStyle(0)
        await this.progressivelyShowResume()
        await this.progressivelyShowStyle(1)
        await this.showHtml()
        await this.progressivelyShowStyle(2)
},
showHtml: function () {
return new Promise((resolve, reject) => {
          this.enableHtml = true
          resolve()
})
      },
progressivelyShowStyle(n) {
return new Promise((resolve, reject) => {
          let interval = this.interval
          let showStyle = (async function () {
            let style = this.fullStyle[n]
            if (!style) { return
}
            // 计算前n 个 style 的字符总数
            let length = this.fullStyle.filter((_, index) => index <= n).map((item) => item.length).reduce((p, c) => p + c, 0)
            let prefixLength = length - style.length
            if (this.currentStyle.length < length) {
let l = this.currentStyle.length - prefixLength
              let char = style.substring(l, l + 1) || ' '
              this.currentStyle += char
              if (style.substring(l - 1, l) === '\n' && this.$refs.styleEditor) {
                this.$nextTick(() => {
                  this.$refs.styleEditor.goBottom()
})
              }

setTimeout(showStyle, interval)
}

else {
resolve()
}
          }).bind(this)
          showStyle()
})
      },
progressivelyShowResume() {
return new Promise((resolve, reject) => {
          let length = this.fullMarkdown.length
          let interval = this.interval
          let showResume = () => {
            if (this.currentMarkdown.length < length) {
              this.currentMarkdown = this.fullMarkdown.substring(0, this.currentMarkdown.length + 1)
              let lastChar = this.currentMarkdown[this.currentMarkdown.length - 1]
              let prevChar = this.currentMarkdown[this.currentMarkdown.length - 2]
              console.log(prevChar)
              if (prevChar === '\n' && this.$refs.resumeEditor) {
                this.$nextTick(() => this.$refs.resumeEditor.goBottom())
}

setTimeout(showResume, interval)
}

else {
resolve()
}
          }

showResume()
})
      }
    }
  }

</script>

<style scoped>
  #app {
font-family: 'Avenir', Helvetica, Arial, sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
}

html {
min-height: 100vh;
}

* {
-webkit-transition: all .3s;
transition: all .3s;
}
</style>

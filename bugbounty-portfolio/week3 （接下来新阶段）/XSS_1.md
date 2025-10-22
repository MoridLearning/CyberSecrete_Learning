# Day11 学习笔记

**日期**：2025.10.22
**目标**：对XSS有一个基础认知，并且进行反射型注入实操

---

## XSS概念：
- XSS（Cross-Site Scripting，跨站脚本攻击）是一种常见的前端漏洞攻击方式。攻击者通过在网页中注入恶意脚本（通常是 JavaScript），诱导浏览器在受害者的上下文中执行该脚本，从而窃取敏感信息（如 Cookie、Session、Token）、劫持用户会话或篡改网页内容。

## XSS分类：
- 反射型（Reflected XSS）
	- 攻击代码通过 URL 参数传入，服务器在响应中直接反射出来，受害者点击恶意链接即可触发。
	- 特点：一次性攻击、需诱导用户访问。
	- 示例：`http://example.com/search?q=<script>alert(1)</script>`

- 存储型（Stored XSS）
	- 攻击代码被永久存储在服务器（如评论区、数据库）中，每当页面渲染该内容时，恶意脚本就会被执行。
	- 特点：持续性强、影响范围广。

- DOM 型（DOM-based XSS）
	- 攻击代码不经过服务器，而是由前端 JavaScript 在浏览器中操作 DOM 时动态注入触发。
	- 特点：发生在客户端逻辑层面（前端漏洞）。

---  

## XSS注入流程：

### 确认回显位置：
- 如输入`abc`，观察`abc`在何处回显（标签内容/属性值……），从而推断使用何种方式注入

### A.回显在 HTML body （例：`<div> ...abc123... <div>`）
- payload：`<script>alert(1)</script>`
- 验证：Response 中出现未转义 <script> → Forward 到浏览器看是否自动弹窗。
- 若被转义（看到 `&lt;script&gt;`） → 走 B 或 C。

### B.回显在 HTML 属性值（例：`<input value="abc123">`）
- payload（闭合引号 + 注入事件）：
	- 双引号情况（常见）：`" onmouseover=alert(1) x="`
	- 单引号情况：`' onmouseover=alert(1) x='`
- 说明：第一个引号关闭原属性，空格分隔新属性，x= 用作占位/重开属性值。
- 验证：Response 中出现类似 <input ... value="" onmouseover=alert(1) x=""> 
- 若输入的 `" `被转码/乱码（看到 `&quot; / â`） → 改用 C（img onerror）或 URL-encode 你的引号再试。

### C. 当 `<script>` 被过滤或 `<` 被转义（看到 `&lt;`）
- payload：`<img src=x onerror=alert(1)>`（图片加载失败会立即触发，无需交互）
- 验证：Response 中出现未转义 `<img src=x onerror=alert(1)>` → Forward 到浏览器看弹窗。
- 若 onerror 被过滤 → 试 D。

### 快速决策树：
- body → `<script>`；
- 被转义 → `img onerror`；
- 属性 → `闭合注入 (" onmouseover=... x=")`；
- 严格过滤 → `svg onload / 编码绕过`。

### 常用 payload 速查（便于复制）：
- <script>alert(1)</script>
- <img src=x onerror=alert(1)>
- " onmouseover=alert(1) x=" / ' onmouseover=alert(1) x='
- <svg onload=alert(1)>
- javascript:alert(1)（href 情况）


### 名词解释：
- 回显（Echo）：服务器把你输入的内容再输出响应给你
- alert(pattern)：浏览器的调试函数，函数被调用时会出现一个弹窗，弹窗的内容为括号内的pattern
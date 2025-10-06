# Day8 学习笔记

**日期**：2025.10.5
**目标**：学习http标头基础知识，抓包一次post请求

---

## 请求头

### Host：
- 告诉服务器访问的是哪个域名（同一台服务器可能有多个站点）

### User-Agent：
- 浏览器/客户端的身份标识，包含操作系统、浏览器版本

### Referer：
- 前一个网页的地址，记录请求是从哪个页面发起的

### Content-Type：
- 说明请求或响应的内容格式（如 application/json、text/html）

### Accept / Accept-Language / Accept-Encoding：
- 告诉服务器客户端能接受什么类型的响应

### Content-Length：
- 请求体或响应体的字节大小

### Authorization：
- 携带用户的身份凭证，告诉服务器「我是谁」（如 Bearer token）

#### Bearer Token（最常见）：
- 格式：Authorization: Bearer <token>
- token 是一串随机生成的字符串（例如 JWT），相当于临时的身份证。
- 用户登录一次 → 服务器返回一个 token → 之后所有请求都带着这个 token → 服务器验证后允许访问。
- 好处：避免明文传输密码，token 可以设置过期时间、权限范围。

### Cookie / Set-Cookie：
- 浏览器和服务器之间维持状态的关键
- Cookie跟网页绑定，浏览器访问网站时会自动把对应网站的 cookie 附带在请求里。

#### Cookie的运用流程：
- 第一次访问，服务器返回响应头里带 Set-Cookie：
- `Set-Cookie: sessionid=abc123; Path=/; HttpOnly `
- 浏览器收到后，把这段信息存下来
- 第二次访问，浏览器自动带上：
- `Cookie: sessionid=abc123 `
- 服务器根据sessionid识别出你是谁

### Cache-Control（常和 Last-Modified / ETag 一起出现）：
- 控制资源是否可缓存、缓存多久，例如 `Cache-Control: max-age=3600 `（缓存 1 小时）。

### Last-Modified：
- 资源的最后修改日期，用于比较同一个资源的多个版本

### ETag：
- 标识资源版本的唯一字符串

### `Host ` / `:authority `：
- HTTP/1.1：用 Host 表示目标域名
- HTTP/2：用 :authority 表示目标域名（作用等同于Host）
# Day9 学习笔记

**日期**：2025.10.6
**目标**：使用 BurpSuite 拦截一个带参数的 GET 请求，在 HTTP history 中观察结果，修改 GET 参数并放行，观察响应变化

---

## 测试网站：
- `http://httpbin.org/get?q=test&lang=zh `

## 拦截到的请求头：
```
GET /get?q=test&lang=zh HTTP/2 
Host: httpbin.org
……
```

### 解释：
- 请求头里的`/get`是路径，域名在下方的`Host`中
- 请求头里的`GET`是请求方法，同类型的还有`POST`等
- `?`表示之后的内容为参数，大致格式为`?key1=value1&key2=value2`，`&`用来连接两个参数 
- 上文示例，参数`q`的值为`test`，参数`lang`的值为`zh`

## 精准拦截指定网站：
- 在`设置-scope`中添加指定的网址，可以指定拦截指定网址
- 设置好scope后可在HTTP history顶栏的过滤器中勾选`只显示范围内`，避免嘈杂流量干扰

## 特殊情况：
- 浏览器直连服务器：
- HTTP请求头路径为相对路径
```
GET /get?q=test HTTP/1.1
Host: httpbin.org
```

- 浏览器经由代理连接服务器：
- HTTP请求头路径为绝对路径（因为浏览器不知道代理最终会连到哪个目标，就把完整 URL 给 Burp，由 Burp 再去帮你转发。）
```
GET http://httpbin.org/get?q=test HTTP/1.1
```
### 两种场景
- 场景 A：开着 Clash，上游代理转发
- 你在 Burp 配置了上游代理 (Upstream proxy)，所以 Burp 收到浏览器的请求后，会自己帮忙把请求再发给 Clash（或者 Clash 之后的目标网站）。
- `对浏览器来说：Burp 就像是“最终服务器”，它不需要知道后面还有 Clash，于是就用**相对路径**`
- 因为浏览器只需要把请求交给 Burp，由 Burp 去处理后续转发。
- 所以你看到的是：
```
GET /get?q=test HTTP/1.1
Host: httpbin.org
```

- 场景 B：没有 Clash，本地端口直连 Burp (8080)
- `浏览器认为 127.0.0.1:8080 就是一个 HTTP 代理（它不会猜 Burp 是终点）`
- 按照 HTTP 规范，浏览器必须把**完整 URL**放在请求行里告诉代理：
```
GET http://httpbin.org/get?q=test HTTP/1.1
```
- `因为如果只给 `/get?q=test`，代理根本不知道要去哪个主机（Host）。`
- 所以在这种直连代理的模式下，你看到的是**绝对路径**。
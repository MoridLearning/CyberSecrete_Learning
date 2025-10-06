# Day9 学习笔记

**日期**：2025.10.6
**目标**：使用 BurpSuite 拦截一个带参数的 GET 请求，在 HTTP history 中观察结果，修改 GET 参数并放行，观察响应变化

---

## 测试网站：
- `http://httpbin.org/get?q=test&lang=zh `

## 拦截到的请求头：
```GET /get?q=test&lang=zh HTTP/2 
Host: httpbin.org
……```

### 解释：
- 请求头里的`/get`是路径，域名在下方的`Host`中
- 请求头里的`GET`是请求方法，同类型的还有`POST`等
- `?`表示之后的内容为参数，大致格式为`?key1=value1&key2=value2`，`&`用来连接两个参数 
- 上文示例，参数`q`的值为`test`，参数`lang`的值为`zh`

## 精准拦截指定网站：
- 在`设置-scope`中添加指定的网址，可以指定拦截指定网址
- 设置好scope后可在HTTP history顶栏的过滤器中勾选`只显示范围内`，避免嘈杂流量干扰
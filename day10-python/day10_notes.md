# Day10 学习笔记

**日期**：2025.10.11
**目标**：使用 Python 读取 HTTP 请求文本文件，提取其中的 Host / User-Agent（或 HTTP/2 中的 :authority / user-agent）字段，将结果写入 log.txt

---

## Python函数：

### open:
- `open(file，mode，encoding)`		打开文件（需要手动关闭）
- mod：r（只读）/ w（覆盖写入）/ a（追加）
- encoding="utf-8"

### with open() …… as f：
- 自动管理文件关闭，并将文件别名为"f"

### readlines：
- `file.readlines()`		将文件内容按行提取并整合到一个列表中
- `file.`**readline()**		仅读取文件中第一行内容并保留为字符串

### write：
- `file.write("文本")`		将文本内容写入文件中，覆盖还是追加取决于打开文件的方式

### startswith：
- `line.startswith(str)`		文本内容是否以'str'开头

### strip：
- `line.strip()`		删除开头和结尾的空白以及换行符
- `lstrip`和`rstrip`		分别删除开头和结尾的空白

## 实操代码：
```
#TODO 从request.txt文件中提取"Host"和"User-Agent“并写入log.txt

with open("request.txt","r",encoding="utf-8") as f:
    lines = f.readlines()

Host = Ua = None
for line in lines:
    if line.startswith(":authority:"):
        Host = line.strip()
    elif line.startswith("user-agent:"):
        Ua = line.strip()

with open("log.txt","w",encoding="utf-8") as f:
    f.write(f"{Host}\n{Ua}\n")

print("已提取字段并写入'log.txt'")
```

### 代码优良学习点：
- `Host = Ua = None`提前定义两个变量
- 防止文件中没找到对应内容是出现变量未定义报错
- 让代码逻辑更清晰，提高代码可读性，预先声明，读代码时可以一眼明确到后面内容与这两个变量有关
- 后续需要时可以提供错误提示
	```
	if not host:
    		print("⚠️ 未找到 Host 字段")
	```
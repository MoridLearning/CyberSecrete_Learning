# Day 4 学习笔记

**日期**：2025.9.28
**目标**：阅读《Automate the Boring Stuff with Python》Ch1–2（中文翻译版可查）学习python基础知识，并写脚本列出当前目录下所有 .txt 文件。

---

```

## *python基础知识*

- 变量名只包含字母、数字、下划线，并且不能以数字打头（且大小写敏感）

- 布尔运算符优先级是：not > and > or （非>与>或）

- 含有elif的代码块中，只要*有一个条件成立*，后面的条件就都被跳过了 （要合理安排elif条件的顺序，必要的时候用多个if来代替elif）

- break跳出循环，continue跳出当前循环进入下一次循环

- `range(fir1 ,sec2 ,thi3)`，可接受三个参数分别代表起始，终止，步长 （含起始不含终止，默认起始为0，步长为1）

- `import random`，每次需以`random.randint()`调用。
- `from random import randint`，可直接调用randint。

## *os库*

- os模块：python内置的操作系统接口

### *相关函数*：

- `os.getcwd()`		"get current working directory"	获取当前工作目录路径
- `os.listdir(path)`	 	列出指定目录下的所有文件和子目录，并返回一个列表 （默认为当前目录，且不会遍历子目录）
- `.endswith(suffix)`	判断字符串是否以suffix为后缀 并返回一个布尔值（`file_name.endswith(".txt")`，判断文件名是否以txt为后缀）
- `os.walk(path) `		遍历path下（含path）的所有目录，并在每个目录返回一个包含root（当前目录路径），dirs（当前目录下的子目					录列表），files（当前目录下的文件列表）的三元组

## 今日*实操代码*：

- 遍历当前文件夹中所有以txt为后缀的文件：

	import os
	path = os.getcwd(".")		#'.'表示当前目录，类似的用法有'..'表示上一级目录
	files = os.listdir(path)		
	for file_name in files:
		if file_name.endswith(".txt")：
			print(file_name)		#只会输出文件名，不会呈现完整路径

- 遍历当前文件夹及子目录中的所有以txt为后缀的文件：

	import os
	for path,dirs,files in os.walk(os.getcwd()):
		for file in files:
			if file.endsiwth(".txt"):
				print(os.path.join(path,file))		#呈现完整路径

```

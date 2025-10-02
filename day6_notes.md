# Day 6 学习笔记

**日期**：2025.10.2
**目标**：完成bandit7和bandit8，掌握grep的基础用法和结合用法

---

## 今日接触到的Linux指令：
- grep、sort、uniq、管道的基础概念

### **grep**的基础用法：
- `grep 	"关键词"		[文件名]`		>> 在文件中查找所有包含**关键词**的行，并展示**整行内容**

- `grep 	-i `		>> 忽视关键词的大小写
- `grep 	-n `		>> 显示匹配行号
- `grep 	-c `		>> 只显示匹配数量
- `grep 	-A N `	>> 输出匹配行的同时输出匹配行**前N行**
- 		-B N `	>> 输出匹配行的同时输出匹配行**后N行**
- 		-C N `	>> 输出匹配行的同时输出匹配行**前后各N行**
- `grep 	-r `		>>递归搜索目录下**所有文件**

- `grep 	" 1 "		... `		>> 避免误匹配到10、11之类的结果，精准定位到独立的1


### **sort**：
- `sort 	[文件名] `			>> 把文件按**行**以字母顺序(a-z)、数字顺序(0-9）排列

- `sort 	-u `				>> 去重，用法类似于**uniq**
- `sort 	-n `				>> 按照数字大小排序


### **uniq**：
- `uniq 	[文件名] `			>> 去除文件中的重复行，但是只能去除**相邻重复行**，故一般需要搭配**sort**使用

- `uniq 	-c `				>> 显示重复行**重复次数**
输出如下：
```
     10 0ZdojRHKYMZ2HH75mmFbUIBR8K8DRJTx
     10 1FZh48ZaVPQSphjoyxSMLzIQbXp7oKKT
     10 2hzECtVb4C4PLXGIgYH6eVBLBJcJmym3
     10 2zJA8BRN8FYM2cVW6K7vTIcfpdzfXLLF
     10 3mFwCPqJL0HVwTZ83w5nNV079jFXcfB6
     10 3oYgS0Otrx9fUslMcV8VgGzWgHdh7HEf
      1 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
```


### 管道的基本知识：
- `command 1 | command 2 `		>> 将前面指令处理后的标准数据传递给后方指令处理



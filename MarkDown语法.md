### [官方文档链接](https://markdown.com.cn/basic-syntax/)
### 标题语法
```java
#一级标题
##二级标题
...
######六级标题
// 注：为了兼容考虑，请用一个空格在 # 和标题之间进行分隔。
or
一级标题
========

二级标题
--------
```
### 段落语法
```java
使用空白行分割段落，不要加缩进！
```
### 换行语法
```java
在一行的末尾添加两个或多个空格，然后按回车键,即可创建一个换行。
或使用HTML标签<br>//(推荐)
```
### 强调语法
#### 粗体(Bold)
```java
**加粗**//(推荐)
__加粗__
```
#### 斜体(Italic)
```java
*斜体*//(推荐)
_斜体_
```
### 引用语句
```java
> 块引用
>> 嵌套块引用
//块引用可以包含部分其他 Markdown 格式的元素
```
效果：
> 块引用
> 块引用
>> 嵌套块引用
>>> 高兴点
### 列表语句
#### 有序列表
```java
1. 第一条条目
2. 第二条条目
...
//使用缩进可嵌套
```
#### 无序列表
```java
格式一：
- 第一条条目
- 第二条条目
...
格式二：
* 第一条条目
* 第二条条目
...
格式三：
+ 第一条条目
+ 第二条条目
...
//三种格式请勿混用
//使用缩进可嵌套
```
效果：
- 第一条条目
- 第二条条目
- ...
### 代码语法
```java
`短语代码块`
转义反引号``words`words`words``
//需要缩进时，每行至少缩进4个空格或一个制表符
```
### 分割线语法
```java

***

---

___

//推荐上下均空一行
```
效果：

***

---

___

### 链接语法
#### 基本语法
```java
[超链接显示名](超链接地址 "超链接title")
//例：
//这是一个链接 [Markdown语法](https://markdown.com.cn "最好的markdown教程")。

```
效果：
这是一个链接 [Markdown语法](https://markdown.com.cn "最好的markdown教程")。
#### 网址和Email地址
```java
使用尖括号可以很方便地把URL或者email地址变成可点击的链接。
//例：
//<https://markdown.com.cn>
//<fake@example.com>
```
效果：
<https://markdown.com.cn>
<fake@example.com>
#### 带格式化的链接
```java
粗体
I love supporting the **[EFF](https://eff.org)**.
斜体
This is the *[Markdown Guide](https://www.markdownguide.org)*.
将链接表示为代码
See the section on [`code`](#code).
```
效果：
I love supporting the **[EFF](https://eff.org)**.
This is the *[Markdown Guide](https://www.markdownguide.org)*.
See the section on [`code`](#code).
#### 引用类型链接
分为两部分
- 放在文本中的部分
    `[hobbit-hole][1] `
    `[hobbit-hole] [1] `
- 引用的部分·
    ```java
    1.放在括号中的标签，其后紧跟一个冒号和至少一个空格（例如[label]:）。
    2.链接的URL，可以选择直接写或将其括在尖括号<>中
    3.链接的可选标题，可以将其括在双引号""，单引号''或括号()中。
    例：
    [1]: https://en.wikipedia.org/wiki/Hobbit#Lifestyle "Hobbit lifestyles"
    ```
    
### 图片语法
#### 基本使用
```java
![图片alt](图片链接 "图片title")
例：
![这是图片](https://i0.hdslb.com/bfs/face/ff3f90aca96c3ed3b716cb2d0ee327adbd9052ea.jpg@150w_150h.jpg "b站头像")
```
效果：
![这是图片](https://i0.hdslb.com/bfs/face/ff3f90aca96c3ed3b716cb2d0ee327adbd9052ea.jpg@150w_150h.jpg "b站头像")
#### 图片链接
```java
[![这是图片](https://i0.hdslb.com/bfs/face/ff3f90aca96c3ed3b716cb2d0ee327adbd9052ea.jpg@150w_150h.jpg "b站头像")](https://space.bilibili.com/132533277?spm_id_from=333.1007.0.0)
```
效果：
[![这是图片](https://i0.hdslb.com/bfs/face/ff3f90aca96c3ed3b716cb2d0ee327adbd9052ea.jpg@150w_150h.jpg "b站头像")](https://space.bilibili.com/132533277?spm_id_from=333.1007.0.0)
### 转义字符语法
`略`
[官方文档链接](https://markdown.com.cn/basic-syntax/escaping-characters.html)
### 内嵌HTML标签
请直接使用
```java
<strong>这是加粗字体</strong>
```
效果：
<strong>这是加粗字体</strong>
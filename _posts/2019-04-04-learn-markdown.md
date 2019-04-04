# Markdown 语法教程


### 文档编写记录

版本    |   说明    |   日期   | 编写者 
-------| ----------| ---------| --------
 0.1   | 复习记录markdown语法 |  2019-04-04 |  汪超
 
## 概要
- markdown 是什么
- markdown 原创者
- markdown 怎么使用

### markdown 是什么

markdown 是一种轻量级的 **标记语言** ，以纯文本形式编写，以html格式发布。

### markdown 是谁创造的

由 [Aaron Swartz](https://baike.baidu.com/item/%E4%BA%9A%E4%BC%A6%C2%B7%E6%96%AF%E6%B2%83%E8%8C%A8/4027108?fromtitle=Aaron%20Swartz&fromid=733955&fr=aladdin) 和 John Gruber 共同设计。 
Aaron 就是那位（2013年1月11日）自杀，有着开挂一般人生经历的程序员。 维基百科对他的介绍是：软件工程师、作家、政治组织者、互联网活动家、维基百科人。

### markdown 怎么使用

markdown的语法分类：
- 标题
- 段落
- 区块应用
- 代码区块
- 强调
- 列表
- 分割线
- 链接
- 图片
- 反斜杠
- 符号
- 表格
- 流程图

####1. 标题 

一级标题
=
二级标题
-
#第一级标题 `<h1>`
##第二级标题 `<h2>`
###第三级标题  `<h3>`
####第四级标题  `<h4>`
#####第五级标题  `<h5>`
######第六级标题  `<h6>`


####2. 段落

段落的前后要有 **空行** ， 空行是没有文字内容。 如想在段内强制换行，用两个以上空格加上回车（引用中换行省略回车）


####3. 区块引用

在段落的每行或者第一行使用符号 > ，可以多个嵌套使用
> 区块引用
> > 嵌套引用
> > > 三级嵌套引用
> > > > 四级嵌套引用


####4. 代码区块

```
    fun main(args: Array<String>) {
       println("Hello World!")
       println("sum = ${sum(34, 67)}")
       println("sum = ${sum(34, 67)}")
       println("sum = ${sum(34, 6, 57, 34)}")
       printSum(237, 57)
       printSum(234, 567, 8)
       vars(1, 4, 6, 78, 0, 6, 9, 8)
      }
```
     
####5. 强调   
  
  强调内容两侧分别加上 * 或者 -   
  ```
  *斜体* ,_斜体_  
  **加粗** ， __粗体__
  ```
  
####6.列表
  
  使用. \ + \ - 标记无序列表

####7. 分割线  

分割线用三个或以上* ，===   - 或 _ 

```
****
----
____
======
```


####8.链接

链接分为行内式 和 参考式

[github](http://github.com)   
自动生成链接 <http://www.github.com>   
[github2][1]  
[1]:http://github.com


####9.图片
  类似于链接  只需要加一个 ! 号  
![GitHub set up](http://zh.mweb.im/asset/img/set-up-git.gif)  
  
####10.反斜杠
 
 //n   \\n
 
  
  
####11.符号
  
  `` Ctrl+A ``
  
  
####12.表格
  
第一格表头 | 第二格表头
---------| -------------
内容单元格 第一列第一格 | 内容单元格第二列第一格
内容单元格 第一列第二格 多加文字 | 内容单元格第二列第二格 
 

####13.流程图
 
   st=>start: Start:>https://www.jpjbp.com/
   io=>inputoutput: verification
   op=>operation: Your Operation
   cond=>condition: Yes or No?
   sub=>subroutine: Your Subroutine
   e=>end
   
   st->io->op->cond
   cond(yes)->e
   cond(no)->sub->io


####14.TOC
   Markdown 语法：
```
[TOC]
```


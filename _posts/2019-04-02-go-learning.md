# Go 语言学习


### 文档编写记录

版本    |   说明    |   日期   | 编写者 
-------| ----------| ---------| --------
 0.1   | 第一次学习记录 |  2019-04-02 |  汪超
 
 
 
 ## 一、简介
  
  go是一门编程语言,与c语言（面向过程）、java语言（面向对象）有相似性，算是综合了两者部分有点。go语言编码格式上自动规范，风格统一。
 
 ## 二、学习大纲
 
 - go语言基础知识
 - go语言开发环境搭建
 - go工程配置-依赖包管理工具
 - go工程demo
 
 ## 三、开始学习 
 
 ### 1、go语言基础知识
 
 
 ### 2、windows的go开发环境配置操作 
 
 ```
     1、下载安装go 
     
     2、配置goroot \ gopath 环境变量
     
     3、idea开发软件安装go 插件： plugin 中搜索， 安装后重启
     
     4、在idea的settings中Language and Framework设置 go 变量， goroot 和gopath
     
        如：  GOPATH="D:\dev\gopath"
     
     5、 在idea 的terminal中 查看go env ， 并用 go env -w GOPROXY=”“  ,设置代理 
     
        go env -w GOSUMDB=off  设置校验值 
     
        如：GOPROXY="http://172.16.154.106:30000,https://goproxy.cn/,direct,https://goproxy.io,http://172.16.59.98:10080"
     
     6、运用go mod 命令来初始化golang 工程的依赖配置，  init  \  tidy  \ download 等 
     
        如：打开idea的terminal，然后进入到工程的根目录，执行 go mod tidy 
     
        如果执行不成功，可以检查第7步的配置，然后再次执行
     
     7、idea Languages & Frameworks 中go  ---> go modules 设置enable go modules 
     
     8、现在你可以下载或创建一个go工程玩耍了
 ```

 
 
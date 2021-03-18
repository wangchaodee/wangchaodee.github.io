# Redis


## 数据结构
key / value
string \map \ list \ sets \ sorted sets 

### 底层数据结构 
sdshdr ,   c字符串  , 
```
struct sdshdr {
 int len ;
 int free ;
 char buf[]; // 字符串末尾有个空字符\0 ,  为了用 <string.h>
}

```

## 操作

操作的原子性， 合并的几个操作也是原子性 

## 命令

##集群方案 

codis 集群模式


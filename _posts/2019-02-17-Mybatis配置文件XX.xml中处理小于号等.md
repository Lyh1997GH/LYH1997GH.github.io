---
layout:     post
title:      Mybatis映射文件中SQL处理小于号于大于号
subtitle:   Mybatis的mapper映射文件XX.xml中SQL处理小于号于大于号,防止被IDEA检测为标签。
date:       2019-02-17
author:     Xionghz
header-img: 
catalog: true
tags:
    - Mybatis
---

# Mybatis映射文件中SQL处理小于号大于号等等
#### 1.报错信息

今儿在用 Mybatis 写XXMapper.xml 的sql时，用了`<`(小于号),IDEA直接报错：

[![ksqT9f.png](https://s2.ax1x.com/2019/02/17/ksqT9f.png)](https://imgchr.com/i/ksqT9f)
***
<font color='red'>标签名预期错误</font>:IDEA把`<`当成了一对标签，因缺少另一半而报错。

#### 2.解决方法
将`<`替换成对应的转义符:

```sql
<!-- &lt; 小于号-->
<select id="selectJcList" parameterType="hashmap">
    select num from zs_jc where stat= 'N' and num &lt; 50 and supno =#{suppnoss} and incode=#{incode}
</select>

```

## 3.符号对应表

| <| <=|  >|>= | &|'|  "|
| :--------:| :-----:| :----: | :-----:| :-----:| :-----:| :----: |
| \&lt;| \&lt;=|\&gt;|  \&gt;=  | \&apos;| \&quot;|\&gt;|

****
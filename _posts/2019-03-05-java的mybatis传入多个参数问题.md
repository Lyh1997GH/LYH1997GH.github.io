---
layout:     post
title:      java的MyBatis传入多个参数
subtitle:   java的MyBatis传入多个参数的问题
date:       2019-03-05
author:     Xionghz
header-img: 
catalog: true
tags:
    - Mybatis
---

# MyBatis传入多个参数

#### 一、单个参数
```
public List<XXBean> select***(String id);  

<select id="select***" resultType="XXBean">

　　	select t.* from tableName t where t.id= #{value}  

</select>  
```
***

 * 其中方法名和ID一致，resultType 存放实体类的完整路径,
 * select 后的字段列表要和bean中的属性名一致， 如果不一致的可以用 as 来补充。

#### 二、多参数
```
public List<XXXBean> selectByParams(String xxId, String xxCode);  

<select id="selectByParams" resultType="XXBean">

　　select t.* from tableName where id = #{0} and name = #{1}  

</select>  
```
***

 * 多参数采用索引式方法，和数组有异曲同工之妙：#{index} 索引从0开始。

#### 三、Map封装多参数
```
public List<XXXBean> select***(HashMap map);  

<select id="select***" parameterType="java.util.HashMap" resultType="XXBean">

　　select 字段... from XXX where id=#{xxId} code = #{xxCode}  

</select>  
```
***

 * hashmap 的 key 为 #{key}。
 
#### 四、List封装foreach
```
public List<user> select***(List<user> list);  

<select id="select***" resultType="top.xiong.user">
　　select id,name,age from tablename where id in
　　<foreach item="item" index="index" collection="list" open="(" separator="," close=")">  
　　　　#{item.id}  
　　</foreach>  
</select>  
```
***

 * `collection`:顾名思义集合 list。
 * `item`:别名。
 * `index`:索引。
 * `open`:这组 list 打开时(开始循环)的开头,只有一次。
 * `close`:反之。这组 list 关闭时(结束循环)的结尾,只有一次。
 * `separator`:循环完一次参数后要打印的字符串。

#### 五、多参数传递之注解方式   
```
public AddrInfo selectInfo(@Param("id")int corpId, @Param("user")int addrId);
 
<select id="selectInfo" resultMap="top.xiong.user">
    SELECT * FROM user where id=#{id} and corp_id=#{user}
</select>
```
***
 * `@Param`注解的 value 要和 #{id}一致。

#### 六、多种类型参数
实际所需参数既要包含String类型，又要包含List类型时的处理方法。将参数放入Map，再取出Map中的List遍历。如下：

```
List<String> list = new ArrayList<String>();
list.add("1");
list.add("2");

Map<String, Object> map = new HashMap<String, Object>();
map.put("list", list); //网址id
map.put("siteTag", "0");//网址类型 

public List<SysWeb> getSysInfo(Map<String, Object> map) {
　　return getSqlSession().selectList("sysweb.getSysInfo", map);
}
 
<select id="getSysInfo" parameterType="java.util.Map" resultType="SysWeb">
　　select t.sysSiteId, t.siteName, t1.mzNum as siteTagNum, t1.mzName as siteTag, t.url, t.iconPath
   from ***1 t left join ***2 t1 on t1.mzNum = t.siteTag and t1.mzType = 10
   WHERE t.siteTag = #{siteTag} 
   and t.sysSiteId not in 
   <foreach collection="list" item="item" index="index" open="(" close=")" separator=",">
       #{item}
   </foreach>
 </select>

```
***

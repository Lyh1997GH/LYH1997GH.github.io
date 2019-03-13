---
layout:     post
title:      java-Field类
subtitle:   java.lang.reflect.Field详解反射
date:       2018-10-15
author:     Xionghz
header-img: 
catalog: true
tags:
    - java
---

# java.lang.reflect.Field详解
## 1、Feild是什么
Field是一个类,位于`java.lang.reflect`包下。<br>
在Java反射中 Field 类描述的是 类的属性信息,比如：

```java
package top.xiong.snark.model;

import java.lang.Override;
import top.xiong.snark.annotation.Column;
import top.xiong.snark.annotation.TbName;

@TbName(name = "Commodity")
public class Commodity {
  @Column("id")
  private Long id;

  @Column("commodity_code")
  private String commodity_code;

  public Commodity() {
  }

  public Long getId() {
    return this.id;
  }

  public void setId(Long id) {
    this.id = id;
  }

  public String getCommodity_code() {
    return this.commodity_code;
  }

  public void setCommodity_code(String commodity_code) {
    this.commodity_code = commodity_code;
  }

  @Override
  public String toString() {
    return "Commodity { " + "id =" + id +", commodity_code =" + commodity_code +" + '}';
  }
}

```

这个Field类的实例描述了属性的全部信息。包括：属性名称、属性类型、属性修饰符、属性注解 等等

## 2、如何获取Field类对象
一共有4种方法,全部都在 Class 类中:<br>

* `getFields()`: 获取类中 public 类型的属性
* `getField(String name)`： 获取类特定的方法，name 参数指定了属性的名称
* `getDeclaredFields()`: 获取类中所有的属性`public、protected、default、private`,但不包括继承的属性。此种方法最常用，比如代码生成器。
* `getDeclaredField(String name)`: 获取类特定的方法，name 参数指定了属性的名称

## 3、Field类中常用的方法
_对于类中的属性，我们常用的操作：<br>_

```java
/**
* 生成查询全表的 sql
* @param obj 已经实例化的类
* @return select all
*/
public String getSelectAllCore(Object obj) {
    StringBuilder classField = new StringBuilder();
    //获取此类所有的成员变量 public、protected、default、private
    Field[] fields = obj.getClass().getDeclaredFields();
    //遍历
    for (Field field : fields) {
        //设置 true ，否则会抛异常
        field.setAccessible(true);
        if (field.isAnnotationPresent(Column.class)) {
            //获取成员变量的注解
            Column column = field.getAnnotation(Column.class);
            //当注解为空时获取当前成员变量的值 field.getName
            String value = StringUtil.isEmpty(column.value()) ? field.getName() : column.value();
            classField.append(value).append(",");
        }

    }
    classField.deleteCharAt(classField.length()-1);
    StringBuilder sb= new StringBuilder("select ");
    sb.append(classField);
    sb.append(" from " ).append(getTableName(obj));
    return sb.toString();  
}
    
```
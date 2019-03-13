---
layout:     post
title:      isFile(),exists(),isDirectory()
subtitle:   java-File,isFile() exists() isDirectory()的区别
date:       2018-10-13
author:     Xionghz
header-img: 
catalog: true
tags:
    - java
---
# isFile() exists() isDirectory()的区别
* `import java.io.File`;
* `import java.io.FileFilter`;

## 1. isFile()
_`public boolean isFile()`_

* 注释:此抽象路径名表示的文件是否是一个标准文.如果该文件不是一个目录,并且满足其他与系统有关的标准,那么该文件是标准文件.由 Java 应用程序创建的所有非目录文件一定是标准文件.
* 返回:当且仅当此抽象路径名表示的文件存在且是一个标准文件时,返回 true ;否则返回 false ; <br>
* 异常抛出:`SecurityException`,如果存在安全管理器,且其`SecurityManager.checkRead(java.lang.String)`方法拒绝对文件进行读访问.

## 2.exists()
_`public boolean exists()`_

* 注释:此抽象路径名表示的文件或目录是否存在.
* 返回:当此抽象路径名表示的文件或目录存在时返回 true；否则 false
* 异常抛出:`SecurityException` 如果纯在安全管理器,且其`SecurityManager.checkRead(java.lang.String)`方法拒绝对文件或目录进行写访问.

## 3. isDirectory()
_`public boolean isDirectory()`_

* 注释:是检查此抽象路径是否是文件夹.
* 返回:当此抽象路径名表示文件夹返回 true；否则返回 false
* 异常抛出:`SecurityException`

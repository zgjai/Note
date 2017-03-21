# Go语言介绍

## 概述
**Go**，又称golang，是Google开发的一种 *静态强类型*、*编译型*、*并发型* ，并具有 *垃圾回收* 功能的编程语言。

## 作者
Go的主要设计者有3位：
**Ken Thompson**
Unix设计者之一，C语言设计者之一，UTF-8设计者之一，图灵奖得主

**Rob Pike**
Unix团队成员之一，UTF-8设计者之一

**Robert Griesemer**
曾协助制作Java的HotSpot编译器，和Chrome浏览器的JavaScript引擎V8

## 语法与特性

### Hello World

**01_HelloWorld.go**

    // 声明本文件的package名
    package main
    
    // 倒入文件中使用到的package
    // 注意：如果没有使用导入的package，必须删除或注释掉，否则编译不通过
    import (
    	"fmt"
    )
    
    func main() {
    	fmt.Println("Hello world")
    }
    
**输出**

![](http://o793hh4oz.bkt.clouddn.com/14862923910295.jpg)

    




---
title: iOS 扩展&代理&通知
date: 2018-12-13T17:39:05+08:00 
draft: false
tags: ['iOS']
categories: ['iOS']
---

## 扩展

### 用途

*   声明私有属性
*   声明私有成员变量
*   声明私有方法

### 特点

注意与 `category` 的区别 \* 编译时决议 \* 只以声明的形式存在,多数情况下寄生在宿主类的`. m`中 \* 不能为系统类添加扩展

## 代理

*   准确的来说是一种软件设计模式, 代理模式
*   iOS 中, 系统为我们提供了`@ protocol` 形式
*   代理是一对一的 ![](https://img.52smile.vip/2018-12-13-094939.jpg)
    
*   协议有必须要实现的(@require), 不惜要实现的(option)
    
*   一般用 weak 来规避循环引用

## 通知

*   使用**观察者模式**来实现用于**跨层传递消息**的机制
*   传递方式 **一对多** ![通知](https://img.52smile.vip/2018-12-13-095923.jpg)

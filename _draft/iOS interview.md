---
layout: post
title: iOS面试题
---



[toc]

先做有答案的题目。



## UI

#### UIView和CALayer？

#### 事件传递和响应？

#### 显示原理？

#### 卡顿掉帧？如何优化？TableView优化？

## Language

id vs instancetype?

1. id在编译时不知道真实类型，instancetype知道，可以让编译器报警告。
2. id还可以定义变量，instancetype只能用做返回值类型。

weak vs assign？

1. 都表示一种非拥有关系，但是weak在对象销毁时会置为nil
2. assign可以用于非OC对象和类似CGFloat等简单OC对象，weak必须用于OC对象

copy？

* 怎么用copy关键字？

  1. block使用copy

  2. NSString，NSArry， NSDictionary等使用copy

     因为NSString, NSArray, NSDictionary都有对应的可变类型NSMutableString, NSMutableArray, NSMutableDictionary，copy可以生一份不可修改的备份，防止被修改

* 如何让自己的类用copy修饰？如何重写带copy关键字的setter？

## Runtime

## Memory

## Block

## 多线程

#### 多线程？

#### block?循环引用？

## RunLoop

##网络

#### get和post？

1. get获取，post提交
2. get无请求体，post有
3. get暴露请求，post安全
4. get url长度有限，post长度不受限制

## 设计模式

#### MVVM？

view和model解耦，view和model关系被放在viewmodel中，所以view和model可以重用。


低耦合高内聚。
#### 原则
1. 单一职责
2. 开闭：扩展开放修改关闭
3. 依赖倒置：依赖抽象不依赖具体
4. 最少知道：尽量少与其他类交互
5. 接口隔离：使用多个隔离接口比单接口好
6. 替换：基类出现的地方子类一定可以出现
7. 复用：使用组合不使用继承

## 架构

##算法

## 第三方库

#### 支付宝等sdk集成？

1. 获取AppID
2. 配置密钥？？
3. 集成sdk
4. 调用接口

#### 可能: XMPP？环信


---
layout: post
title: iOS知识树
---

- [iOS](#ios)
	- [语言特性](#语言特性)
		- [分类](#分类)
		- [扩展](#扩展)
		- [分类 vs 扩展](#分类-vs-扩展)
		- [关联对象](#关联对象)
		- [代理 vs 通知](#代理-vs-通知)
			- [通知如何实现？](#通知如何实现)
		- [KVO](#kvo)
		- [KVC](#kvc)
		- [属性关键字](#属性关键字)
			- [assign vs weak:](#assign-vs-weak)
			- [怎么用copy？](#怎么用copy)
				- [浅拷贝 vs 深拷贝？](#浅拷贝-vs-深拷贝)
			- [MRC下retain修饰setter方法？](#mrc下retain修饰setter方法)
			- [id vs instancetype?](#id-vs-instancetype)
		- [Swift语言特性](#swift语言特性)
			- [Swift对象内存模型](#swift对象内存模型)
	- [内存](#内存)
		- [虚拟内存](#虚拟内存)
		- [NSObject数据结构](#nsobject数据结构)
		- [isa](#isa)
			- [isa指向](#isa指向)
			- [isa类型](#isa类型)
				- [NONPOINTER_ISA](#nonpointer_isa)
				- [散列表](#散列表)
					- [引用计数表](#引用计数表)
					- [弱引用表](#弱引用表)
		- [引用计数](#引用计数)
			- [ARC](#arc)
				- [ARC下的dealloc](#arc下的dealloc)
			- [自动释放池](#自动释放池)
				- [自动释放池什么时候释放？](#自动释放池什么时候释放)
			- [循环引用](#循环引用)
			- [objc使用什么机制管理对象内存？](#objc使用什么机制管理对象内存)
			- [如何检查内存问题？原理是什么？](#如何检查内存问题原理是什么)
	- [Runtime](#runtime)
		- [消息传递](#消息传递)
			- [以下代码，输出什么？](#以下代码输出什么)
			- [消息转发](#消息转发)
		- [Method-Swizzling](#method-swizzling)
		- [动态添加方法](#动态添加方法)
		- [动态方法解析](#动态方法解析)
		- [main之前？](#main之前)
	- [Block](#block)
		- [什么是block？](#什么是block)
		- [截获变量](#截获变量)
		- [__block修饰符](#__block修饰符)
		- [block类型](#block类型)
		- [block的循环引用](#block的循环引用)
	- [多线程](#多线程)
		- [同步异步串行并行](#同步异步串行并行)
		- [GCD](#gcd)
		- [NSOperation](#nsoperation)
			- [怎样控制状态？](#怎样控制状态)
		- [NSThread](#nsthread)
		- [锁](#锁)
			- [多线程？](#多线程-1)
			- [block?循环引用？](#block循环引用)
		- [RunLoop](#runloop)
			- [概念](#概念)
			- [数据结构](#数据结构)
			- [(1)事件循环机制](#1事件循环机制)
			- [RunLoop与NSTimer](#runloop与nstimer)
			- [RunLoop与多线程](#runloop与多线程)
	- [UI](#ui)
		- [UIView vs CALayer](#uiview-vs-calayer)
		- [点击事件是如何传递和响应？](#点击事件是如何传递和响应)
		- [图像是如何显示？](#图像是如何显示)
			- [CPU和GPU各做了什么？](#cpu和gpu各做了什么)
			- [为什么会卡顿掉帧？](#为什么会卡顿掉帧)
			- [什么是离屏渲染？为什么会产生离屏渲染？](#什么是离屏渲染为什么会产生离屏渲染)
			- [UIView绘制原理](#uiview绘制原理)
			- [如何提高界面流畅度？](#如何提高界面流畅度)
	- [网络](#网络)
		- [http协议](#http协议)
		- [https与网络安全](#https与网络安全)
			- [tls](#tls)
		- [tcp/udp](#tcpudp)
		- [dns解析](#dns解析)
		- [session/cookie](#sessioncookie)
			- [get和post？](#get和post)
	- [性能优化(Pending)](#性能优化pending)
		- [电量优化？](#电量优化)
	- [设计模式与架构](#设计模式与架构)
			- [原则](#原则)
		- [责任链](#责任链)
		- [桥接](#桥接)
		- [适配器](#适配器)
		- [单例](#单例)
		- [命令](#命令)
		- [策略](#策略)
		- [工厂](#工厂)
		- [状态](#状态)
		- [观察者](#观察者)
		- [代理](#代理)
		- [迭代器](#迭代器)
		- [组合](#组合)
		- [外观](#外观)
		- [模版](#模版)
		- [装饰者](#装饰者)
		- [架构](#架构)
			- [图片缓存？](#图片缓存)
			- [阅读时长统计？](#阅读时长统计)
			- [MVVM？](#mvvm)
			- [组件化？](#组件化)
	- [算法和数据结构](#算法和数据结构)
		- [数据结构](#数据结构-1)
			- [树](#树)
		- [排序算法](#排序算法)
		- [常见算法](#常见算法)
		- [加密算法](#加密算法)
			- [RSA](#rsa)
			- [DH](#dh)
	- [第三方库](#第三方库)
		- [Alamofire?](#alamofire)
		- [SDWebImage?](#sdwebimage)
		- [JSBridge?](#jsbridge)
		- [RxSwift(Pending)](#rxswiftpending)
		- [PromiseKit(Pending)](#promisekitpending)
	- [跨平台(Pending)](#跨平台pending)
		- [Flutter](#flutter)
		- [ReactiveNative](#reactivenative)
	- [参考](#参考)


# iOS

## 语言特性

### 分类 
1. 声明私有方法
2. 按不同的功能分解类
3. 把framework私有方法公开

分类方法添加到方法列表顺序：
后编译的分类方法先添加到方法列表，原类方法最后添加到方法列表，所以最后编译的分类会”覆盖“前面的方法。

[源码][runtime源码]

### 扩展
```
// 扩展
@interface Car()
-(void)fire;
@property (nonatomic) NSUInterge price;
@end
```
1. 声明私有属性
2. 声明私有方法
3. 声明私有成员变量

### 分类 vs 扩展
1. 分类运行时添加，扩展编译时。
2. 可以为系统类添加分类，扩展不行。
4. 类扩展只能在原类.m中。

### 关联对象
为分类动态添加实例变量，方法： 
```
getAssociatedObject
setAssociatedObject
removeAssociatedObjects
```
关联对象的本质：
由AssociationsManager管理，对象上所有关联对象会被保存到一个AssociationMap, 然后保存在一个全局的AssociationsHashMap.

### 代理 vs 通知
1. 代理模式， 观察者模式
2. 代理一对一， 通知一对多
3. 代理借助协议实现， 需要注意循环引用

#### 通知如何实现？
保存一张Map表，key为观察的notificationname，value为一个结构体包含观察的对象和回调的函数，利用观察者模式实现。

### KVO
key-value observing, 观察者模式，通过isa-swizzling（动态创建新子类，重写setter方法，修改isa指针）实现。
重写的setter方法：
```
- (void)setValue:(NSString *)value {
	[self willChangeValueForKey: @"value"];
	_value = value;
	[self didChangeValueForKey: @"value"];
}
```

为什么不用分类而是用继承？
因为分类会覆盖原有set方法。

### KVC
key-value coding，键值编码。
方法：
```
valueForKey
setValue:forKey
```
可以通过KVC访问私有变量，会破坏面向对象。
执行机制：
1. 判断方法或类似方法是否存在, 没有
2. 判断`accessInstanceVariablesDirectly`是否返回Yes, 是找实例变量或类似实例变量, 不是调用`setValue forUndefinedKey`
3. 有类似实例变量，直接使用，没有
5. 调用`setValue:forUndefinedKey`，默认抛异常。
（类似方法是指: getKey, Key, isKey，类似实例变量是指：_key, _isKey, key, isKey）

![kvc](./iOS_interview_image/kvc_process.png)

### 属性关键字

1. 读写: readwrite(default), readonly
2. 原子: atomic(default, get, set线程安全，但是对数组的add，remove等不保证线程安全), nonatomic
3. 引用计数: strong/retain, assign/unsafe_unretained, weak, copy

#### assign vs weak:
1. assign可以用于基本类型， weak只用于OC对象
2. assign和weak不改变引用计数
3. assign可能产生悬挂指针，weak释放自动置为nil

#### 怎么用copy？
1. NSString, NSArray, NSDictionary等因为有对应的Mutable类型，经常用copy。
2. block用copy，block原来在栈区通过copy拷到堆区。

##### 浅拷贝 vs 深拷贝？
1. 浅拷贝增加引用计数，指向同一块地址
2. 深拷贝不会增加引用计数，指向新开辟的内容相同的内存空间

![copy](./iOS_interview_image/copy_mutablecopy.png)



#### MRC下retain修饰setter方法？

```
@property (nonatomic, retain) id obj;
- (void)setObj:(id)obj {
	if (_obj != obj) {
		[_obj release];
		_obj = [_obj retain];
	}
}
```

#### id vs instancetype?
1. id在编译时不知道真实类型，instancetype知道，可以让编译器报警告。
2. id还可以定义变量，instancetype只能用做返回值类型。

### Swift语言特性
1. Swift静态语言，类型在编译时决定，没有isa指针，所有类型被保存到静态空间，实例变量的第一个指针type指向对象类型。
2. Swift所有都是对象。

#### Swift对象内存模型
没有isa指针，对象包括type指针，rc，属性值，
type指针指向保存在静态空间的类对象，类对象保存方法。

## 内存
 
### 虚拟内存
![memory](./iOS_interview_image/memory_layout.png)

### NSObject数据结构
![struct](./iOS_interview_image/runtime_struct.png)

### isa

#### isa指向
![class_metaclass](./iOS_interview_image/class_metaclass.png)

#### isa类型
1. TaggedPointer(适用于number等小对象， 没有isa，本身即是实际值+类型信息)
2. NONPOINTER_ISA（优化的isa指针，保存更多信息）
3. 散列表（引用计数表和弱引用表）

##### NONPOINTER_ISA
![nonpointer_isa](./iOS_interview_image/nonpointer_isa_address_1.png)
![nonpointer](./iOS_interview_image/nonpointer_isa_address_2.png)

##### 散列表
sidetables：hash表，value为sidetable，通过对象指针的hash映射找到所属表。
sidetable包含：
1. 自旋锁：忙等锁，适合轻量访问
2. 引用计数表：hash表，value结构：
3. 弱引用表：hash表

###### 引用计数表
hash表，value结构：
![reference_table](./iOS_interview_image/reference_hash_map.png)

###### 弱引用表
![weak table](./iOS_interview_image/weak_reference_hashmap.png)

### 引用计数

#### ARC
自动管理引用计数。
1. 编译器LLVM自动添加retain和release
2. runtime使weak对象置为nil

##### ARC下的dealloc
![dealloc](./iOS_interview_image/dealloc_process.png)

在`object_dispose`清理weak表，关联对象等。

#### 自动释放池
1. 双向链表，每个节点是一个栈。
2. 和线程对应。
3. 通过哨兵节点判断释放池的边界。

##### 自动释放池什么时候释放？
App启动后，苹果在主线程 RunLoop 里注册了两个 Observer
第一个 Observer是 监听Entry，用来创建自动释放池。优先级最高，保证创建释放池发生在其他所有回调之前。
第二个 Observer 监视了两个事件： BeforeWaiting 时 释放旧池创建新池；Exit时释放自动释放池。这个 Observer 的 优先级最低，保证其释放池子发生在其他所有回调之后。

#### 循环引用
形成一个圈，分成：自循环，相互循环和多循环。

打破循环引用方法：
1. 避免循环引用
2. 在合适的时机手动断环

另外timer会被runloop强引用，所以可以在NSTimer和vc间增加中间对象，在每次触发定时器时判断如果vc释放，则释放timer。

#### objc使用什么机制管理对象内存？
引用计数，每次runloop运行的时候，都会check retaincount，如果为零，则释放。

#### 如何检查内存问题？原理是什么？
1. Allocation
2. Leak
3. Memory Graph
4. Analyse
5. MLeaksFinder (原理: swizzle navigationController的pop和push方法, 判断weak是否变空)

## Runtime

### 消息传递

![message](./iOS_interview_image/message_send.png)

1. 通过hash函数检查缓存是否命中
2. 对于排序的方法列表，采用二分查找，对于未排序的，采用遍历。
3. 检查父类是否命中，需要检查父类缓存和方法列表

#### 以下代码，输出什么？

```
@interface Phone: Mobile
@end
@implementation Phone
-(id)init {
	self = [super init];
	if(self) {
		NSLog(@"%@", NSStringFromClass([self class]));
		NSLog(@"%@", NSStringFromClass([super class]));
	}
}
```

答案：
Phone 
Phone

原因：
转化成：
```
[self class] -> objc_msgSend(self, @selector(class))
[super class] ->objc_msgSendSuper(super, selector(class))

struct objc_super {
	__unsafe_unretained id receiver;
}
```
super只是从父类开始查，最后在NSObject中找到class方法，因为super的receiver还是self，所以返回phone。

#### 消息转发

![message_forward](./iOS_interview_image/message_forward.png)

1. +resolveInstanceMethod, 动态添加方法，消息会被重新发送。
2. fast转发， 通过forwardingTargeForSelector给其他对象
3. normal转发，先通过methodSignatureForSelector判断函数参数和返回值，然后通过forwardInvocation转发给目标对象。

### Method-Swizzling
使用`exchangeImplementations`交换SEL所对应的IMP。

### 动态添加方法
通过在消息转发resolveInstanceMethod使用`class_addMethod`添加动态方法。

### 动态方法解析
@dynamic： 将函数定义推迟到运行时。

### main之前？

1. 加载dyld（动态链接器），配合ImageLoader将二进制文件加载到内存
2. 开启缓存策略，dyld加载依赖库包括libobjc(objc和runtime)和libSystem(GCD,block,C语言，加密库)，动态链接并初始化
3. 到可执行程序初始化，runtime对所有类初始化调用load，由于lazy bind机制，依赖库多数在使用时才初始化类结构
4. 所有初始化结束，调用main

## Block

### 什么是block？
结构体，同时可以截获变量。

### 截获变量
1. 局部变量：基本数据类型截获值，对象类型连同所有权修饰符截获。
2. 静态局部变量：指针形式截获。
3. 全局或静态全局变量：不截获。

### __block修饰符
__block修饰的变量变成对象, 地址被传入block，局部变量赋值需要__block修饰。

__forwarding作用：
使栈上的__forwarding和堆上的__forwarding都指向堆上的__block变量。

![forwarding](./iOS_interview_image/block_forwarding.png)

### block类型
1. 栈
2. 堆
3. 全局

对block作copy操作：
1. Global，copy什么不做
2. Stack， copy to heap
3. Malloc, copy增加引用计数

### block的循环引用
解决循环引用两个方法：
1. 手动断开
2. weak

## 多线程

### 同步异步串行并行
同步：不开启新的线程
异步：可以开启新的线程，和当前任务同步执行
串行：一个接着一个执行
并行：同时执行

### GCD

dispatch_set_target_queue: 1. 修改优先级 2. 创建层级结构。

<img src="./iOS_interview_image/gcd_set_target_queue.png" alt="GCD" style="zoom:50%;" /> 

dispatch_after: 到时间block被加入queue， 另外timer基于runloop，所以会有runloop没开不运行，mode不对不运行等问题，after则没有这些问题。

dispatch_group: dispatch_group_notify or dispatch_group_wait

dispatch_barrier_async

dispatch_apply：多次添加同一task

dispatch_suspend/dispatch_resume：只阻止尚未开始的task。

dispatch_semaphore: 信号量大于0进入，否则等待

dispatch_once

dispatch I/O

dispatch_source: 可以cancel。

gcd是在系统层对线程管理。



一般用于简单的数据同步或多读单写。

同步/异步 和 串行/并发

performSelector:afterDelay要求当前线程有runloop正在运行，否则不执行。

dispatch_barrier_async

 dispatch_group_async

### NSOperation
基于GCD。
特点：
1. 添加任务依赖
2. 任务执行状态控制：isReady, isExecuting, isFinished, isCancelled
3. 最大并发量

#### 怎样控制状态？
1. 如果只重写main方法，底层控制变更任务执行状态
2. 如果重写了start方法，自行控制任务状态

状态变更会用到KVO,系统通过KVO移除已经完成的op。

### NSThread
![thread](./iOS_interview_image/thread_process.png)

用于实现常驻线程。

[源码][NSThread源码]

### 锁
1. @synchronized: 创建单例对象使用
2. atomic： 修饰属性关键字，保证赋值安全，不保证使用安全
3. OSSpinLock：自旋锁，忙等，用于轻量级访问如int +1/-1
4. NSRecursiveLock：递归锁，可以重复lock
5. NSLock: 互斥锁
6. dispatch_semaphore_t：信号量
   1. dispatch_semaphore_create: 创建信号量
   2. dispatch_semaphore_wait: -1, 如果<0,当前线程主动阻塞
   3. dispatch_semaphore_signal：+1, 如果<=0, 被动唤醒，由释放的线程唤醒阻塞线程

#### 多线程？

#### block?循环引用？

### RunLoop

#### 概念

内部维护事件循环对事件/消息管理的对象。

事件循环： 没有消息休眠（用户态->内核态），有消息立刻唤醒（内核态->用户态）

之所以可以休眠而不是死循环，是由于run会调用系统的mach_msg(), 切换到内核态，

之所以唤醒，是由于mach_msg在一定条件(source1等), 会唤醒runloop，返回用户态

#### 数据结构

1. CFRunLoop
2. CFRunLoopMode
3. souce/timer/observer: source0触摸事件，source1 port事件

#### (1)事件循环机制

![process](./iOS_interview_image/runloop_process.png)

#### RunLoop与NSTimer

苹果mode name:

1. defaultmode
2. uitrackingmode
3. commonmode

将defaultmode和uitrackingmode加入runloop的commonmode（系统预置），timer加入commonmodeitems，timer会自动同步到各个commonmode下，可以使timer在多个mode下运行。

#### RunLoop与多线程

1. 一一对应
2. 主线程自动开启，非主线程手动开启



点击图标，程序启动，运行，到程序被杀死，内部过程？

1. runloop开启：main函数运行，内部会调用UIApplicationMain，在这里面会启动主线程的runloop
2. runloop运行：经过一系列的处理，runloop休眠，如果这时我们点击主屏幕，会产生MachPort，然后转化成source1，把主线程唤醒，处理事件
3. runloop结束：当程序被杀死，进入runloop的退出，observer会接收到通知，runloop关闭后，线程被销毁

如何实现一个常驻线程？

1. 为当前线程开启一个runloop
2. 向runloop中添加一个port/source等维持runloop事件循环
3. 启动runloop

runloop mode的作用？

model主要用来指定事件在运行循环中的优先级。



卡顿检测？

添加observer到runloop的common mode，通过各个阶段的运行时间，判断卡顿位置。

## UI

### UIView vs CALayer
UIView有一个layer成员，同时它继承自UIResponder.
CALayer负责视图显示，UIView只负责事件的传递和响应，体现了单一职责原则。

### 点击事件是如何传递和响应？
点击事件被UIApplication捕获，然后从UIWindow到UIView, 一层一层直到传递到最内层的UIView。通过hitTest和pointinside可以判断点是否在这个视图上，通过修改hidden、alpha、userinteractionisenable可以屏蔽视图传递。
当事件传递到最内层后，通过判断当前视图UIResponse相关的touchbegin、touchmove、touchend是否响应，决定是否把事件交给父视图处理，如果都没有响应，则丢弃这个事件。

### 图像是如何显示？
CPU从UIView中得到位图，通过总线传给GPU进行渲染，然后将结果保存在帧缓冲区。视频控制器根据垂直信号从帧缓冲区提取内容显示到显示器上。

![display](./iOS_interview_image/whole_display.png)

#### CPU和GPU各做了什么？
CPU的工作：
1. layout：布局和文本计算
2. display: 绘制
3. prepare：图片编解码
4. commit：提交位图

GPU的工作：
1. 顶点着色
2. 图元装配
3. 光栅化
4. 片段着色
5. 片段处理

![cpu&gpu](./iOS_interview_image/cpu_gpu_display.png)


#### 为什么会卡顿掉帧？
正常情况下每秒要显示60帧，每16.7ms会产生一个垂直信号，如果信号到来时，CPU和GPU不能将结果存入帧缓冲区，就会掉帧。

#### 什么是离屏渲染？为什么会产生离屏渲染？
GPU在当前屏幕缓冲区以外新开辟一块空间用于渲染就叫离屏渲染。当修改某些图层属性，未合成时不能显示，就会触发离屏渲染。
1. 圆角（和maskToBounds一起使用）
2. 图层蒙版
3. 阴影
4. 光栅化

#### UIView绘制原理
当调用UIView的setneedsdisplay只是打上标记，display会在runloop下一个绘制周期被调用，如果实现了layer的displayLayer会触发异步绘制。

![draw](./iOS_interview_image/draw_principle.png)


系统绘制流程

![system](./iOS_interview_image/system_draw_process.png)

(backing store 相当于bitmap）

异步绘制流程

![async_draw](./iOS_interview_image/async_draw_process.png)

#### 如何提高界面流畅度？ 
1. 优化CPU
   1. 优化对象创建、调整、销毁
      1. 对于不涉及事件响应的控件，使用轻量的CALayer代替UIView
      2. storyboard资源消耗大，在性能敏感的情况不使用storyboard创建对象
      3. 推迟对象的创建，使用缓存池复用对象
      4. layer的属性的修改是通过运行时resolveInstanceMethod为对象临时添加方法，属性值保存在Dictionary中，并且会通知delegate等，应该尽量减少属性修改
      5. 视图层次修改时，会产生很多方法调用，应尽量避免视图层次的调整，添加和修改
      6. 把对象放到后台线程销毁
   2. 优化布局和文本计算
      1. 在后台线程提早计算好布局，一次性调整视图属性
      2. 复杂视图autolayout消耗指数上升，可以通过手动布局
      3. 在后台线程计算文本宽高
   3. 优化文本渲染，图片编解码等
      1. 通过自定义文本控件，异步绘制
      2. 在后台线程把图片会绘制到CGBitmapContext，然后从Bitmap绘制图片
      3. 在异步线程进行图像绘制
2. 优化GPU
   1. 优化纹理渲染
      1. 将多张图片合成一张图片显示
   2. 优化视图混合
      1. 减少视图层级和数量，在不透明的视图中使用opaque避免Alpha合成
   3. 优化图形生成
      1. 为了防止离屏渲染，避免使用遮罩、圆角、阴影等
      2. 把需要显示的图形在后台线程绘制成图片

## 网络

### http协议

request和reponse格式

1. request: 请求行， 消息头， 消息体
2. response：状态行，消息头，消息体

request类型：get, post, head, put, delete, options

get vs post

1. get用于获取资源，post用于处理资源
2. get请求参数在url，post在body
3. get参数有限制，post没有限制
4. get请求不安全，post安全

状态码

![statuscode](./iOS_interview_image/response_statuscode.png)

建立连接

![http_connect](./iOS_interview_image/http_process.png)

http特点

1. 无连接 -> http持久连接，头部字段：（Connection： keep-alive, time: 20(多少时间), max: 10（多少条请求）)
2. 无状态 -> Cookie/Session

### https与网络安全

https = http + tls

#### tls
加密协议层，注意：
1. 会话密钥是通过三个随机数合成。
2. 通过服务器的公钥加密客户端生成的第三个随机数，保证了安全性。
具体流程：
![https_process](./iOS_interview_image/https_process.png)



### tcp/udp

传输层。

udp特点:

1. 无连接
2. 不保证可靠
3. 面向报文：不合并不拆分



复用、分用

差错检测



tcp特点：

1. 面向连接
2. 可靠
3. 面向字节流
4. 流量控制
5. 拥塞控制 

### dns解析

### session/cookie



#### get和post？

1. get获取，post提交
2. get无请求体，post有
3. get暴露请求，post安全
4. get url长度有限，post长度不受限制

如何判断请求结束？

1. Content-length
2. chunked，最后一个package是空的chunked

charles抓包原理？

中间人攻击。

## 性能优化(Pending)

### 电量优化？

1. CPU和GPU优化
2. 定位：降低精确度，及时关闭
3. 网络：一次性下载数据，多用缓存
4. 蓝牙陀螺仪等硬件：按需使用


## 设计模式与架构

#### 原则

1. 单一职责：一个类只负责一件事
2. 开闭：扩展开放修改关闭
3. 依赖倒置：依赖抽象不依赖具体
4. 最少知道：尽量少与其他类交互，高内聚，低耦合
5. 接口隔离：使用 多个专门协议比单协议好
6. 替换：基类出现的地方子类一定可以出现



### 责任链

调用方法时，像链表一样传递下去，每一个节点基类相同只处理自己相关的事务。

```
class A {
	A *next;
	
	Process() {
		block = {
			next->Process();
		}
		handle(block)
	}
	
	handle(returnBlock) {...}
}
```

### 桥接

桥梁作用，用抽象代替具体。

```
class BaseA {
	BaseB *obj;
}
```

### 适配器

转换接口。

```
class A{
	B *b;
	methodA() {
		// 额外操作
		b->methodB();
	}
}
```

### 单例

只有一个实例，懒汉和饿汉。

```
class Single {
	+ (id)sharedInstance {
		static Single *instance = nil;
		static dispatch_once_t onceToken;
		dispatch_once(&onceToken, ^{
			instance = [[super allocWithZone: NULL] init];
		}
		return instance;
	}
	
	+ (id)allocWithZone:(struct _NSZone*)zone {
		return [self sharedInstance];
	}
	
	- (id)copyWithZone:(struct _NSZone*)zone {
		return self;
	}
}
```

### 命令

将请求封装成对象，支持cancel。

### 策略

一个方法有多种实现，对于不同的类型可以用不同的实现。

### 工厂

封装类的实例化。

简单工厂：没有子类，通过参数决定实例化的类。

工厂：子类决定实例化的类。

抽象工厂：包含多个接口，每个接口创建一个实例类。

### 状态

内部状态改变时，切换状态对象。

### 观察者

特点：一对多，松耦合。

描述：注册成为某个主题的观察者，当主题的状态变化，通知所有观察者。

### 代理

委托他人实现具体方法。

### 迭代器

封装聚合顺序访问内部实现。

### 组合

集合对象和个别对象继承同一个父类。

### 外观

提供统一接口。

### 模版

父类创建算法模版，子类通过继承修改某些方法实现。

### 装饰者

通过装饰者动态给原有对象添加新的功能，装饰者和原有对象类型相同。

### 架构

目的

1. 模块化

2. 分层

3. 解耦

4. 降低代码重合度



#### 图片缓存？

![design layer](./iOS_interview_image/image_cache_design_layer.png)

1. 内存读取，有，返回，没有
2. 磁盘读取，有，存到内存，返回，没有
3. 请求网络，存到内存，返回



内存模块需要考虑：

1. size：e.g 10kb * 100, 50kb * 20, 100kb * 10...
2. 淘汰策略
   1. 队列，先进先出
   2. LRU：可以在读写和前后台切换时check存在时间，超过多长时间未使用就删除

磁盘需要考虑：

1. size
2. 淘汰策略
3. 存储方式

网络需要考虑：

1. 最大并发
2. 超时策略
3. 优先级

图片解码需要考虑：可以用策略模式实现对不同种类的图片格式的解码。

解码应该在图片从磁盘取出或网络返回后。



#### 阅读时长统计？

![readtime](./iOS_interview_image/readtime_design.png)

为什么有不同记录器？适配不同场景。

如何处理数据丢失？

1. 定时写磁盘
2. 定量写磁盘

上传？

1. 立刻上传
2. 延时上传
   1. 前后台切换
   2. 从无网到有网变化
3. 定时上传

#### MVVM？

view（view和viewcontroller）只和viewmodel交互，view包含viewmodel强引用成员，viewmodel通过block回调view，model只和viewmodel交互，viewmodel有model强引用成员，model通过block或delegate回调viewmodel。

RN原理？

![RN](./iOS_interview_image/RN_principle.png)

客户端整体架构？

![client](./iOS_interview_image/client_design_layer.png)

中间层用于解耦业务逻辑。

业务间的解耦方式：

1. openURL
2. 依赖注入（业务对其他业务的依赖通过外部注入，实现解耦）



SkyDRM 架构？

client层：UI， DownloadManger, UploadManager, RenderManager

业务层：User(usermanager, user, preference), cloud(cloudmanager, repositorymanager...), token(tokenmanger), log(logmanager)

通用业务层：RestAPI(command: query, response), cache(coredata, keychain, plist manager)

通用层： Alamofire， core



#### 组件化？

目的：解耦，复用，方便管理。

按照基础组件、功能组件、业务组件将项目拆分成多个子项目， 业务组件可以依赖基础组件和功能组件， 业务组件和业务组件之间通过中间层解耦，中间层通过router访问组件最后形成组件对中间层的单向依赖。

## 算法和数据结构
### 数据结构
链表
栈 FILO
队列 FIFO
哈希表 key和value通过哈希函数映射
字典
#### 树
* 二叉搜索树
  二叉树，满足left <= middle <= right
  中序遍历(根在中间)
  先序遍历（根在前）
  后序遍历（根在后）
  操作： 查询，删除，添加
* 红黑树
  是一棵平衡二叉树，满足：
  1. 根节点和叶子结点是黑色
  2. 如果一个结点是红色，则它的子节点是黑色
  3. 对每个结点，到所有后代叶结点的黑色结点相同
* B树

### 排序算法
快排
希尔
桶
冒泡
选择
堆
归并
### 常见算法
字符串反转？

链表反转？

有序数组合并？

Hash算法？

查找两个子视图的共同父视图？

求无序数的中位数？



递归与迭代？

### 加密算法
#### RSA
素数因式分解困难。

#### DH
1. 公开a和p
2. 各自产生公私钥: x, a^x, y, a^y
3. 交换公钥a^x, a^y
4. 得到会话密钥(a^y)^x = (a^x)^y mod p

原理：
`k = (a^x)^y = (a^y)^x mod p`
大数的对数计算很慢。

## 第三方库

### Alamofire?

核心类：SessionManager, SessionDelegate, Request, TaskDelegate

多线程：GCD， OperationQueue

SessionManager的单例default设置SessionDelegate为URLSession的delegate。

当发起一个请求时，SessionManager会创建URLSessionTask， 创建request，设置它的task为URLSessionTask，初始化它的taskdelegate，并且sessiondelegate保存task到request的映射。

当通过链式表达式加入response block会被保存到taskdelegate的queue中，这个queue一开始是挂起的，加入validate会被保存到request的validataions。

当session 回调回来时，判断是否有用户自定义的block回调，没有则通过delegate保存的task到request的映射回调到taskdelegate。

当complete回调返回时，先执行所有的validations，如果失败则保存error到request，当回调到taskdelegate，如果有error则结束，如果没有，不再挂起queue，response将responseData通过block回调。

### SDWebImage?

1. 扩展UIImageView和UIButton
2. 主要类：SDWebImageManage管理dowload和cache，SDWebImageDownloader下载图片和SDWebImageCache管理缓存
3. 下载图片，如果内存有，从内存取，没有从磁盘取，放入内存，没有通过网络下载图片。

如何设计图片缓存参考架构-图片缓存。

### JSBridge?

JS通过Iframe，加载url，触发native webview代理，native调用api后回调JS方法。

### RxSwift(Pending)

### PromiseKit(Pending)

## 跨平台(Pending)

### Flutter

### ReactiveNative

## 参考

[runtime源码]: ./iOS_interview_image/runtime.h
[NSThread源码]: ./iOS_interview_image/NSThread.m
[UI绘制原理1](https://leoliuyt.github.io/2018/05/26/UI绘制原理/)
[UI绘制原理2](http://hchong.net/2019/05/11/iOS开发UI-UI绘制原理/)
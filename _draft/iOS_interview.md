---
layout: post
title: iOS面试题
---



[toc]

先做有答案的题目。



## UI

### UITableView相关

1. 重用机制: 离开屏幕的cell并没有释放掉，而是被加入重用池中，当请求新的cell时从重用池取出。
2. 数据源同步
   1. 并发访问，数据拷贝：记录数据操作，在拷贝数据中也执行一次
   2. 串行访问

### 事件传递&视图响应

传递： UIApplication->UIWindow->UIView, 通过hitTest和pointinside判断，hidden、alpha、userinteractionisenable可以用来屏蔽视图传递。

响应：如果传递到的视图UIResponse相关的touchbegin、touchmove、touchend不响应，则交给父视图，如果都没有响应，则丢弃这个事件。

###图像显示原理

总结：CPU得到位图，通过总线传给GPU渲染，然后保存到帧缓冲区，视频控制器根据信号从帧缓冲区提取内容显示到显示器。

![display](./iOS_interview_image/whole_display.png)

cpu和gpu

![cpu&gpu](./iOS_interview_image/cpu_gpu_display.png)

cpu

![cpu](./iOS_interview_image/cpu_display.png)

layout: 布局和文本计算

display: 绘制

prepare：图片编解码

commit：提交位图

gpu(了解)

![gpu](./iOS_interview_image/gpu_display.png)



###卡顿&掉帧

60fps，即60帧每秒，即每16.7ms会有一个VSync信号

如果在信号到来时，CPU和GPU不能完成完成任务，就会掉帧。

如何优化？

1. CPU
   1. 在子线程对对象创建、修改、删除
   2. 在子线程预排版（布局和文本计算）
   3. 预渲染（文本等异步绘制，图片编解码等）
2. GPU
   1. 纹理渲染方面，减少离屏渲染，即减少layer的圆角，masktobounds，阴影蒙层
   2. 视图混合方面，简化视图层级



### 绘制原理&异步绘制

UIView绘制原理

![draw](./iOS_interview_image/draw_principle.png)

setneedsdisplay只是打上标记，display是在当前runloop将要结束时被调用。

系统绘制流程

![system](./iOS_interview_image/system_draw_process.png)

(backing store 相当于bitmap）

异步绘制流程

1. layer.delegate生成bitmap
2. 将bitmap赋给layer.contents

![async_draw](./iOS_interview_image/async_draw_process.png)



###离屏渲染

在屏渲染：GPU的渲染是在当前显示缓冲区中进行

离屏渲染：GPU在当前屏幕缓冲区以外新开辟一块空间用于渲染

何时出发离屏渲染？

1. 圆角+maskToBounds
2. 图层蒙版
3. 阴影
4. 光栅化 

为什么要避免离屏渲染？增加GPU工作量，可能会使CPU和GPU不能准时完成任务，最后产生掉帧卡顿。

#### UIView和CALayer？

UIView内部有一个layer成员，UIView用于事件的传递和响应，CALayer用于视图显示，这里面体现了单一职责的设计原则。

## OC语言特性

###分类

特点：

1. 运行时添加
2. 可以对系统类添加分类

作用：

1. 声明私有方法
2. 按不同的功能分解类
3. 把framework私有方法公开

可以添加：

1. 实例方法
2. 类方法
3. 属性
4. 协议

易错点：

1. 分类添加的方法可以“覆盖”原类方法
2. 同名分类方法后编译的先生效
3. 名字相同的分类会引起编译报错

关联对象的本质：

关联对象由AssociationsManager管理，对象上所有关联对象会被保存到一个AssociationMap, 然后存入一个全局的AssociationsHashMap.

###关联对象

可以为分类添加实例变量。

主要函数： 

getAssociatedObject

setAssociatedObject

removeAssociatedObjects

### 扩展

特点：

1. 编译时决定
2. 只以声明形式存在，多数情况寄生于宿主类
3. 不能为系统类添加扩展

### 代理

1. 代理模式
2. 一对一
3. 借助协议实现

委托者拥有weak delegate，避免循环引用。

###通知

跨层传递。

如何实现通知机制？

保存一张Map表，key为观察的notificationname，value为一个结构体包含观察的对象和回调的函数，利用观察者模式实现。

### KVO & KVC

KVO：key-value observing, 观察者模式，通过isa-swizzling（动态创建新子类，重写setter方法，修改isa指针）实现。

重写的setter方法：

```
- (void)setValue:(NSString *)value {
	[self willChangeValueForKey: @"value"];
	_value = value;
	[self didChangeValueForKey: @"value"];
}
```

KVC：key-value coding，键值编码。

valueForKey

setValue:forKey

可以通过KVC访问私有方法，会破坏面向对象。

![kvc](./iOS_interview_image/kvc_process.png)

1. 判断getter或setter方法是否存在
2. 如果没有实现accessInstanceVariablesDirectly, 则判断实例变量(key, _key)或相似的实例变量( _isKey, _isKey)是否存在
3. 调用get或set的undefinedkey函数

###属性关键字

1. 读写: readwrite(default), readonly
2. 原子: atomic(default, get, set线程安全，但是对数组的add，remove等不保证线程安全), nonatomic
3. 引用计数: strong/retain, assign/unsafe_unretained, weak, copy

assign vs weak:

1. assign可以用于基本类型， weak只用于OC对象

2. assign和weak不改变引用计数

3. assign可能产生悬挂指针，weak释放自动置为nil

浅拷贝 vs 深拷贝？

1. 浅拷贝增加引用计数，指向同一块地址
2. 深拷贝不会增加引用计数，指向新开辟的内容相同的内存空间

![copy](./iOS_interview_image/copy_mutablecopy.png)



MRC下retain修饰setter方法？

```
@property (nonatomic, retain) id obj;
- (void)setObj:(id)obj {
	if (_obj != obj) {
		[_obj release];
		_obj = [_obj retain];
	}
}
```



id vs instancetype?

1. id在编译时不知道真实类型，instancetype知道，可以让编译器报警告。
2. id还可以定义变量，instancetype只能用做返回值类型。

copy？

* 怎么用copy关键字？

  1. block使用copy

  2. NSString，NSArry， NSDictionary等使用copy

     因为NSString, NSArray, NSDictionary都有对应的可变类型NSMutableString, NSMutableArray, NSMutableDictionary，copy可以生一份不可修改的备份，防止被修改

* 如何让自己的类用copy修饰？如何重写带copy关键字的setter？

## Runtime

### 数据结构

![struct](./iOS_interview_image/runtime_struct.png)

id = objc_object

1. isa_t
2. 关于isa操作相关
3. 弱引用相关
4. 关联对象相关
5. 内存管理相关

Class = objc_class(继承自objc_object)

1. superClass: Class
2. cache: cache_t(方法缓存)
3. bits：class_data_bits_t(类基本信息，包括原有和分类的属性、实例、方法)

isa

1. 指针型isa
2. 非指针型isa（部分代表Class地址）

isa指向：

实例->Class->MetaClass

cache_t（保存SEL和IMP）

1. 用于快速查找方法
2. 是可增量的哈希表（查找更快）
3. 应用局部性原理（只有部分方法被频繁使用）

class_data_bits_t是class_rw_t的封装

class_rw_t（可读可写）

1. class_ro_t
2. protocols(二维数组，分类)
3. properties（二维数组，分类）
4. methods（二维数组，分类 ）

class_ro_t（只读）:

1. name(类名)
2. ivars（实例变量， 一维数组，原有）
3. properties（一维数组， 原有）
4. protocols（一维数组，原有）
5. methodlist（一维数组，原有）

method_t

1. SEL name 方法名称
2. const char* types 方法返回值和参数
3. IMP imp  函数体

const char* types

返回值 + 参数1 + ...(第一，第二个参数固定@：，表示self的id类型和方法的SEL类型)

### 类对象与元类对象

类对象和元类对象类型都是objc_class,

1. 类对象存储实例成员、方法
2. 元类对象存储类成员、方法

![class_metaclass](./iOS_interview_image/class_metaclass.png)

实例方法查找过程：

1. 类对象中寻找
2. 逐级往上，直到nil

类方法查找过程：

1. 原类对象中寻找
2. 逐级往上
3. 在根类对象寻找，如果还没有，返回nil

###消息传递

![message](./iOS_interview_image/message_send.png)

```
[self class] -> objc_msgSend(self, @selector(class))
[super class] ->objc_msgSendSuper(super, selector(class))

struct objc_super {
	__unsafe_unretained id receiver;
}

```



输出什么？

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

原因：super只是从父类开始查，最后还是在NSObject中找到class方法，因为super的receiver还是self，所以返回phone

###方法缓存

1. 通过SEL去cache的哈希表中查找，如果没有
2. 在当前类中查找，如果方法列表是排序好的，用二分法查找，如果未排序，遍历，如果没有
3. 通过superclass，逐级查找，如果没有返回nil

### 消息转发

![message_forward](./iOS_interview_image/message_forward.png)



### method-swizzling

交换方法实现。

exchangeImplementations

### 动态添加方法

performSelector

class_addMethod

###动态方法解析

@dynamic

运行时语言

编译时语言

## Memory

###内存布局

![memory](./iOS_interview_image/memory_layout.png)

stack: 方法调用

heap：alloc等分配的对象

bss：未初始化的全局和静态变量

data：已初始化的全局和静态变量

### 内存管理方案

1. TaggedPointer(number等小对象， value即是实际值+一些信息)
2. NONPOINTER_ISA
3. 散列表（包含引用计数表和弱引用表）



NONPOINTER_ISA

![nonpointer_isa](./iOS_interview_image/nonpointer_isa_address_1.png)

![nonpointer](./iOS_interview_image/nonpointer_isa_address_2.png)

散列表

sidetables：hash表，value为sidetable，sidetable又是一张hash表

分离锁：每张sidetable都有一把锁

### 数据结构

散列表包含：

1. 自旋锁：忙等锁，适合轻量访问
2. 引用计数表：hash表

![reference_table](./iOS_interview_image/reference_hash_map.png)

3. 弱引用表：hash表

   ![weak table](./iOS_interview_image/weak_reference_hashmap.png)

### ARC & MRC

 MRC：手动管理，alloc, dealloc, **retain, release, retainCount, autorelease**

ARC: 自动管理，编译器和runtime协作，不能使用手动管理关键词，增加weak,strong关键词

### 引用计数

retainCount = 存在数据结构中的引用计数+1， 所以尽管alloc不调用retain，retainCount=1.

dealloc内部实现

![dealloc](./iOS_interview_image/dealloc_process.png)

### 弱引用

添加weak变量过程

id __weak a = b -> objc_initWeak(&a, b) ->storeWeak ->weak_register_no_lock

在register函数中，会通过hash查找对象b的弱引用表，如果对象b weak表为空，创建一张新表，使第一个元素为指针a，其余为nil，不为空，直接添加新元素。

清除weak变量，指针置为nil过程

dealloc ->... ->weak_clear_no_lock

在clear中，会通过hash查找当前对象对应的弱引用表，然后遍历，如果找到指针，置为nil。

### 自动释放池

1. 双向链表，每个结点都是一个栈，每次push先判断栈是否满，未满，插入一个nil的哨兵，然后添加对象，满则插入一个新结点栈，然后进行未满操作，每次pop，把到哨兵前的对象release。
2. 和线程一一对应。

### 循环引用

形成一个圈，分成：自循环，相互循环和多循环。

打破循环引用方法：

1. 避免循环引用
2. 在合适的时机手动断开环

因为timer会被runloop强引用，所以可以在NSTimer和vc间增加中间对象，在每次触发定时器时判断如果vc释放，则释放timer。

## Block

### 什么是block

block是将函数及其上下文封装起来的对象。

block调用即函数调用。

### 截获变量

1. 局部变量：基本数据类型截获值，对象类型连同所有权修饰符截获。
2. 静态局部变量：指针形式截获。
3. 全局变量：不截获。
4. 静态全局变量：不截获。

###__block修饰符

1. 对局部变量无论基本类型还是对象类型赋值都需要__block.
2. 对静态局部、全局、静态全局变量赋值都不需要__block。

__block修饰的变量变成了对象。

###block内存管理

类型：

1. Global，copy什么不做
2. Stack， copy to heap
3. Malloc, copy增加引用计数

__forwarding在未copy时指向栈上的block变量，copy后不论栈或是堆的forwarding都指向堆上的block对象。

![forwarding](./iOS_interview_image/block_forwarding.png)

###block的循环引用

将block内使用的成员变量在block外赋给一个weak修饰的局部对象类型变量，因为局部对象类型被截获会连带修饰符，所以可以避免循环引用。

```
__block A *a = self;
_blk = ^int(int num){
	int result = num * a.var;
	// ARC下的修改：a = nil;
	return result;
}
_blk(2);

```

MRC下，没有问题。

ARC下，内存泄漏。



## 多线程

### GCD

一般用于简单的线程同步和多读单写。

同步/异步 和 串行/并发

performSelector:afterDelay要求当前线程有runloop正在运行，否则不执行。

dispatch_barrier_async

 dispatch_group_async

### NSOperation

优势特点：

1. 添加任务依赖
2. 任务执行状态控制：isReady, isExecuting, isFinished, isCancelled
3. 最大并发量

怎样控制状态？

1. 如果只重写main方法，底层控制变更任务执行状态
2. 如果重写了start方法，自行控制任务状态

状态变更会用到KVO。

### NSThread

用于实现常驻线程。

![thread](./iOS_interview_image/thread_process.png)

###多线程与锁

锁：

1. @synchronized: 创建单例对象使用
2. atomic： 修饰属性关键字，保证赋值安全，不保证使用安全
3. OSSpinLock：自旋锁，忙等，用于轻量级访问如int +1/-1
4. NSRecursiveLock：递归锁，可以重复lock
5. NSLock: 互斥锁
6. dispatch_semaphore_t：信号量
   1. dispatch_semaphore_create: 
   2. dispatch_semaphore_wait: -1, 如果<0,当前线程主动阻塞
   3. dispatch_semaphore_signal：+1, 如果<=0, 被动唤醒，由释放的线程唤醒阻塞线程

#### 多线程？

#### block?循环引用？

## RunLoop

### 概念

内部维护事件循环对事件/消息管理的对象。

事件循环： 没有消息休眠（用户态->内核态），有消息立刻唤醒（内核态->用户态）

之所以可以休眠而不是死循环，是由于run会调用系统的mach_msg(), 切换到内核态，

之所以唤醒，是由于mach_msg在一定条件(source1等), 会唤醒runloop，返回用户态

### 数据结构

1. CFRunLoop
2. CFRunLoopMode
3. souce/timer/observer

### 事件循环机制

![process](./iOS_interview_image/runloop_process.png)

### RunLoop与NSTimer

苹果mode name:

1. defaultmode
2. uitrackingmode
3. commonmode

将defaultmode和uitrackingmode加入runloop的commonmode（系统预置），timer加入commonmodeitems，timer会自动同步到各个commonmode下，可以使timer在多个mode下运行。

###RunLoop与多线程

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

##网络

###http协议

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

###https与网络安全

https = http + ssl/tls

https连接流程

![https_process](./iOS_interview_image/https_process.png)



###tcp/udp

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

###dns解析

###session/cookie



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

## 设计模式

责任链

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

桥接

```
class BaseA {
	BaseB *obj;
}
```

适配器

```
class A{
	B *b;
	methodA() {
		// 额外操作
		b->methodB();
	}
}
```

单例

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



命令





#### 原则
1. 单一职责：一个类只负责一件事
2. 开闭：扩展开放修改关闭
3. 依赖倒置：依赖抽象不依赖具体
4. 最少知道：尽量少与其他类交互，高内聚，低耦合
5. 接口隔离：使用 多个专门协议比单协议好
6. 替换：基类出现的地方子类一定可以出现







## 架构

目的

1. 模块化

2. 分层

3. 解耦

4. 降低代码重合度



图片缓存？

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



阅读时长统计？

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

##算法

字符串反转？

链表反转？

有序数组合并？

Hash算法？

查找两个子视图的共同父视图？

求无序数的中位数？



递归与迭代？



## 第三方库

#### 支付宝等sdk集成？

1. 获取AppID
2. 配置密钥？？
3. 集成sdk
4. 调用接口

#### 可能: XMPP？环信



# iOS Core Animation

## Layer
UIView=CALayer+事件响应
CALayer负责图像绘制。
之所以把事件响应和图像绘制分开是为了兼容iOS和Mac，iOS和Mac绘制相同但是响应不同。

### 显示图片
1. 将图片赋给contents.
2. 自定义drawRect方法。

contents属性：
* contentsGravity图片显示方式
* contentScale显示比例
* masksToBounds裁剪
* contentsRect指定图片的一部分
* contentsCenter拉伸方式

drawRect内部实现：
实际上调用CALayerDelegate的drawLayer方法，然后在合适的时机调用display显示。

### 坐标系
frame是通过bounds+position+transform计算而来。
anchorPoint是view的锚点，用于旋转。
通过layer的conver函数，可以转换坐标系。
zPosition可以修改view前后顺序。
containPoint返回点是否在当前layer中，hitTest返回当前点最深的layer。

#### 自动布局
autolayout或autoresizing时，自定义的Layer需要手动布局，所以尽量重用View自带的layer。

### 视觉效果
#### 圆角
通过CornerRadius设置，masksToBounds可以使子视图也产生圆角效果。
#### 边框
borderWidth, borderColor
#### 阴影
shadowOpacity，shadowColor, shadowOffset, shadowRadius，shadowPath
#### 蒙版
设置layer的mask。
#### 设置放大缩小算法
minificationFilter， magnificationFilter
#### 透明度
opacity

### 仿射变换
通过矩阵乘法，使[x, y, z] -> [x', y', z']
2D基本操作：旋转，缩放，平移

### Layer类型
#### CAShapeLayer
通过path属性设置形状，效率优于drawRect.

#### CATextLayer
用于显示文字，优于UILabel.

## 动画

### 隐式动画
每次runloop循环都会开启一个新的事务，所有对属性的修改会加入这个事务中然后被制成动画。

### 显示动画

## 绘制效率
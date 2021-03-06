
### 事件
* ACTION_DOWN 手指初次触摸到屏幕时候触发
* ACTION_MOVE 手指在屏幕上华东的时候触发，会多次触发
* ACTIOM_UP 手指离开屏幕的时候触发
* ACTION_CANCEL 事件被上层拦截的时候触发
* ACTION_OUTSIDE手指不再控件区域的时候触发

##### 方法
* getAction()	获取事件类型。
* getX()	获得触摸点在当前 View 的 X 轴坐标。
* getY()	获得触摸点在当前 View 的 Y 轴坐标。
* getRawX()	获得触摸点在整个屏幕的 X 轴坐标。
* getRawY()	获得触摸点在整个屏幕的 Y 轴坐标。

#### 单点触控一次简单的交互流程
* 手指落下(ACTION_DOWN)------>多次移动(ACTION_MOVE)------>离开(ACTION_UP)
* 如果只是单击事件不会触发ACTION_MOVE
![](http://onf44qqgp.bkt.clouddn.com/17-4-4/53392735-file_1491296114912_10c53.gif)

#### 伪代码
```java
@Override
    public boolean onTouchEvent(MotionEvent event) {
        switch (event.getAction()){
            case MotionEvent.ACTION_DOWN:
                //手指第一次解除屏幕
                break;
            case MotionEvent.ACTION_MOVE:
                //手指移动
                break;
            case MotionEvent.ACTION_UP:
                //手指抬起
                break;
            case MotionEvent.ACTION_CANCEL:
                //事件被拦截
                break;
            case MotionEvent.ACTION_OUTSIDE:
                //超出区域
                break;
        }
        return super.onTouchEvent(event);
    }
```

#### ACTION_CANCEL

上层View回收事件处理权限是后，ChildView会受到一个ACTION_CANCLE事件<br>
eg: <br>
上层 View 是一个 RecyclerView，它收到了一个 ACTION_DOWN 事件，由于这个可能是点击事件，所以它先传递给对应 ItemView，询问 ItemView 是否需要这个事件，然而接下来又传递过来了一个 ACTION_MOVE 事件，且移动的方向和 RecyclerView 的可滑动方向一致，所以 RecyclerView 判断这个事件是滚动事件，于是要收回事件处理权，这时候对应的 ItemView 会收到一个 ACTION_CANCEL ，并且不会再收到后续事件。

### 多点触控
#### 事件

* ACTION_DOWN	第一个手指初次接触到屏幕 时触发。
* ACTION_MOVE	手指在屏幕上滑动时触发，会多次触发。
* ACTION_UP	最后一个 手指 离开屏幕 时触发。
* ACTION_POINTER_DOWN	有非主要的手指按下(即按下之前已经有手指在屏幕上)。
* ACTION_POINTER_UP	有非主要的手指抬起(即抬起之后仍然有手指在屏幕上)。

#### 方法

* getActionMasked() 多点触控必须使用这个方法获取事件类型
* getActionIndex() 获取该事件是哪个手指产生的
* getPointerCount() 获取屏幕上手指的个数
* getPointerId(int pointerId)获取一个指针的位移标识符ID，手指按下和抬起来之间Id始终不变
* findPointerIndex(int pointerId)	d)	通过PointerId获取到当前状态下PointIndex，之后通过PointIndex获取其他内容
* getX(int pointerIndex)	获取某一个指针(手指)的X坐标
* getY(int pointerIndex)	获取某一个指针(手指)的Y坐标

#### getAction() 与 getActionMasked()

当多个手指在屏幕上同时按下时候，会产生大量的事件，如何在获取事件类型的同时区分这些事件就是一个大问题了。
<br>
int类型共32位(0x00000000)，他们用最低8位(0x000000ff)表示事件类型，再往前的8位(0x0000ff00)表示事件编号，以手指按下为例讲解数值是如何合成的:<br>
<br>
ACTION_DOWN 的默认数值为 (0x00000000)<br>
ACTION_POINTER_DOWN 的默认数值为 (0x00000005)<br>

index属性：
* 第1个手指按下	ACTION_DOWN (0x00000000)
* 第2个手指按下	ACTION_POINTER_DOWN (0x00000105)
* 第3个手指按下	ACTION_POINTER_DOWN (0x00000205)
* 第4个手指按下	ACTION_POINTER_DOWN (0x00000305)

#### 注意

* 多点触控时必须使用 getActionMasked() 来获取事件类型。
* 单点触控时由于事件数值不变，使用 getAction() 和 getActionMasked() 两个方法都可以。
* 使用 getActionIndex() 可以获取到这个index数值。不过请注意，getActionIndex() 只在 down 和 up 时有效，move 时是无效的。

#### 郑重声明：追踪事件流，请认准 PointId，这是唯一官方指定标准，不要相信 ActionIndex 那个小婊砸。

PointId 在手指按下时产生，手指抬起或者事件被取消后消失，是一个事件流程中唯一不变的标识，可以在手指按下时 通过 getPointerId(int pointerIndex) 获得。 (参数中的 pointerIndex 就是 actionIndex)

#### 历史数据
由于我们的设备非常灵敏，手指稍微移动一下就会产生一个移动事件，所以移动事件会产生的特别频繁，为了提高效率，系统会将近期的多个移动事件(move)按照事件发生的顺序进行排序打包放在同一个 MotionEvent 中，与之对应的产生了以下方法：

* getHistorySize()	获取历史事件集合大小
* getHistoricalX(int pos)	获取第pos个历史事件x坐标(pos < getHistorySize())
* getHistoricalY(int pos)	获取第pos个历史事件y坐标(pos < getHistorySize())
* getHistoricalX (int pin, int pos)	获取第pin个手指的第pos个历史事件x坐标(pin < getPointerCount(), pos < getHistorySize() )
* getHistoricalY (int pin, int pos)	获取第pin个手指的第pos个历史事件y坐标(pin < getPointerCount(), pos < getHistorySize() )

#### 注意
* pin 全称是 pointerIndex，表示第几个手指，此处为了节省空间使用了缩写。
* 历史数据只有 ACTION_MOVE 事件。
* 历史数据单点触控和多点触控均可以用
#### 伪代码

```java
void printSamples(MotionEvent ev) {
     final int historySize = ev.getHistorySize();
     final int pointerCount = ev.getPointerCount();
     for (int h = 0; h < historySize; h++) {
         System.out.printf("At time %d:", ev.getHistoricalEventTime(h));
         for (int p = 0; p < pointerCount; p++) {
             System.out.printf("  pointer %d: (%f,%f)",
                 ev.getPointerId(p), ev.getHistoricalX(p, h), ev.getHistoricalY(p, h));
         }
     }
     System.out.printf("At time %d:", ev.getEventTime());
     for (int p = 0; p < pointerCount; p++) {
         System.out.printf("  pointer %d: (%f,%f)",
             ev.getPointerId(p), ev.getX(p), ev.getY(p));
     }
}

```
#### 获取事件发生的时间
* getDownTime()	获取手指按下时的时间。
* getEventTime()	获取当前事件发生的时间。
* getHistoricalEventTime(int pos)	获取历史事件发生的时间。

#### 事件分发机制
事件分发机制有dispatchTouchEvent，onInterceptTouchEvent和onTouchEvent三个方法协助完成，一般处理场景是拦截down事件，然后之后的move，up事件就在该拦截的地方停止
![](https://upload-images.jianshu.io/upload_images/944365-aea821bbb613c195.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


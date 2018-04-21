### 四中引用类型
![](https://pic1.zhimg.com/80/65b7abe9bf2fcd249c789024d95bb67a_hd.jpg)
#### 强引用:只要引用存在，对象就永远不会被回收，我们平时用到嘴对的就是强应用，就算内存溢出也不会被回收、Object o = new Object()
#### 软引用:用来描述一些有用但是非必须的对象，在内存溢出之前，会被这些对象列进回收范围内，之后进行回收
```java
        SoftReference softReference = new SoftReference(s);
        if (softReference.get() != null) {
            sN = (String) softReference.get();
        }else {
            s = new String("hello");
            softReference = new SoftReference(s);
        }
```
#### 弱引用：用来描述一些非必须对象，只能存活在下一次垃圾回收之前,当下一次垃圾回收之前无论内存是不是够，都会回收
#### 虚引用：也成为幻影引用，一个对象是否有幻影应用都不会影响他的生存时间，也无法通过一个虚引用来获取一个对象，唯一的作用，就是在垃圾回收的时候获取一个通知虚引用主要用来跟踪对象被垃圾回收器回收的活动。虚引用与软引用和弱引用的一个区别在于：虚引用必须和引用队列 （ReferenceQueue）联合使用。当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在回收对象的内存之前，把这个虚引用加入到与之 关联的引用队列中。
```java
ReferenceQueue queue = new ReferenceQueue ();
PhantomReference pr = new PhantomReference (object, queue); 
```
程序可以通过判断引用队列中是否已经加入了虚引用，来了解被引用的对象是否将要被垃圾回收。如果程序发现某个虚引用已经被加入到引用队列，那么就可以在所引用的对象的内存被回收之前采取必要的行动。
https://blog.csdn.net/coding_or_coded/article/details/6603549

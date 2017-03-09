
## 多线程的安全问题
```java
class Ticket implements Runnable{
 private int ticket =100;
 public void run(){

   while(true){
     if(ticket>0){
       System.out.System.out.println(ticket--);     
     }
   }
 }
   
}
```
#### 问题的原因
当多个线程同时获取获取cpu执行权的时候，都会执行ticket--;多条语句在操作同一个线程的时候，一个线程堆多条语句只执行了一部分，还没有执行完，另一个线程参与进来，导致共享数据
#### 解决办法
对于多条操作共享的数据的时候，只能让一个线程执行完，其他线程不可以参加执行
#### java对于多线程的安全问题提供了解决方案
就是同步代码块
synchronized(对象){
  需要被同步的代码
}
#### 对象(监视器，锁)
判断的条件，当线程获得执行权之后执行同步代码，进入之后讲对象关闭。直到上一个线程执行完同步代码。对象打开，这个时候各个线程又开始争取执行权。eg:厕所的门把手；改之后的代码如下：

```java
class Ticket implements Runnable{
 private int ticket =100;
 Object obj=new Object();
 public void run(){
   while(true){
     synchronized (obj) {
       if(ticket>0){
         System.out.System.out.println(ticket--);     
       }
     }
   }
 }
   
}
```
#### 同步的前提：
* 必须要有两个或者两个以上的线程
* 必须多个线程使用同一个锁
* 必须保证同步中有一个线程在运行

#### 好处 :解决多线程同步问题
#### 弊端 :每次都在判定锁,消耗了资源

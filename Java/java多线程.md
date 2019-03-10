
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
可以重入，一个线程获取了锁之后，在当前类的其他方法中也能获取锁，若果出现异常，锁和会自动释放。同步不具有继承性
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
## 生产者消费者问题
```java
/**
 * 生产者
 */
public class Product{
    private Condition condition;
    private ReentrantLock reentrantLock;

    public Product(ReentrantLock reentrantLock,Condition condition) {
        super();
        this.condition = condition;
        this.reentrantLock = reentrantLock;
    }
    public void setValue(){
            try {
                reentrantLock.lock();
                while (!StringObject.value.equals("")){
                    condition.await();
                }
                String value = System.currentTimeMillis()+""+System.nanoTime();
                System.out.println(Thread.currentThread().getName()+"set value = "+value);
                StringObject.value = value;
                condition.signalAll();

            } catch (InterruptedException e) {
                e.printStackTrace();
            }finally {
                reentrantLock.unlock();
            }
    }
}
/**
 * 消费者
 */
public class Consumer {
    private ReentrantLock reentrantLock;
    private Condition condition;

    public Consumer(ReentrantLock reentrantLock, Condition condition) {
        this.reentrantLock = reentrantLock;
        this.condition = condition;
    }
    public void getValue(){
        try {
            reentrantLock.lock();
            while (StringObject.value.equals("")){
                condition.await();
            }
            System.out.println(Thread.currentThread().getName()+"get value = "+StringObject.value);
            StringObject.value = "";
            condition.signalAll();
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            reentrantLock.unlock();
        }

    }
}
public class ConThread extends Thread {
    private Consumer consumer;

    public ConThread(Consumer consumer) {
        this.consumer = consumer;
    }

    @Override
    public void run() {
        while (true){
            consumer.getValue();
        }
    }
}
public class ProThread extends Thread {
    private Product p;
    public ProThread(Product p){
        super();
        this.p = p;
    }
    @Override
    public void run() {
        while (true){
            p.setValue();
        }
    }
}

public class Client {
    public static void main(String[] args) {
        ReentrantLock reentrantLock = new ReentrantLock();
        Condition condition = reentrantLock.newCondition();

        for (int i = 0; i < 3 ; i++) {
            ConThread conThread = new ConThread(new Consumer(reentrantLock,condition));
            ProThread proThread = new ProThread(new Product(reentrantLock,condition));

            conThread.start();
            proThread.start();
        }
    }
}

```
###### 这里是多生产多消费的代码，注意的地方使用signalAll而不是signa，如果是单生产单消费的话不用注意这个问题，但是多生产的话如果不适用All则会造成假死，由于当前生产线程有好多，在生产完一个之后唤醒了下一个生产者线程接着等待，导致所有的生产者线程都处于等待状态，消费者线程也会有同样的问题，所以造成了假死。使用while的原因是因为如果两个线程在wait之后被唤醒会继续往下执行，会发生越界异常


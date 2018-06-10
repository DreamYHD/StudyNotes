---
title: Java反射
date: 2017-4-23 18:58:27
categories: [Java]
tags: [Reflect,Java]
---
### 基本概念
JAVA反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意方法和属性；这种动态获取信息以及动态调用对象方法的功能称为java语言的反射机制。

### 基本功能
* 在运行时候判断任意对象所属的类
* 在运行时候构造任意一个对象
* 在运行时候判断任意 一个类具有的成员变量和方法
* 在运行时调用任意一个对象的方法
* 生成动态代理

### 基本运用
#### 1.获得class（3种）

* 使用Class类的forName(String clazzName)静态方法，改方法需要传入字符串参数，该字符串的参数的值是某个类的全名（eg：cn.edu.nuc.Test）
* 调用每个类的class属性来获取该类对应的Class对象
* 调用每个对象的getClass()方法，该类是Java.lang.Object的一个方法

```java
     //第一种方式
     Class mClass=Class.forName("cn.edu.nuc.People");
     System.out.println(mClass.getName());

     //第二种方式
     Class mClass2=People.class;
     System.out.println(mClass2.getName());

     //第三种方式
     People mPeople=new People("dreamY",18);
     Class mClass1=mPeople.getClass();
     System.out.println(mClass1.getName());

```
```
D:\java\jdk\bin\java
cn.edu.nuc.People
cn.edu.nuc.People
cn.edu.nuc.People

Process finished with exit code 0

```
#### 2.创建实例（主要两种）
* 使用Class对象的newInstance方法来创建Class对象对应的实例（只可以用来创建无参数的，如果含有参数，不能采取这种方法）
* 通过Class对象获取制定的Constuctor对象，再通过Constructor对象的newInstance()方法来创建实例，这种方法可以用制定的构造器构造类的实例

```java

        //第一种
        Class mClass=Class.forName("cn.edu.nuc.People");
        People mPeople= (People) mClass.newInstance();
        mPeople.setAge(18);
        System.out.println(mPeople.getAge());

        //第二种
        Constructor mConstructor=mClass.getConstructor(String.class,int.class);//获取制定参数类型的构造器
        People mPeople1= (People) mConstructor.newInstance("dreamY",18);
        System.out.println(mPeople1.getAge()+mPeople1.getName());


```
```
D:\java\jdk\bin\java
18
18dreamY

Process finished with exit code 0

```

#### 3.获取方法
以下所有结果以该程序为基础
```java
public class Student {
    public Student() {

    }

    public void eat(int mI){
        System.out.println(mI);

    }
    public void say(){


    }
    private void shengao(){

    }
    protected void tizhong(){


    }
}

```
* getDeclaredMethods()方法返回类或接口声明的所有方法，包括公共、保护、默认（包）访问和私有方法，但不包括继承的方法。

```java
       Class mClass=Student.class;
       Method[]mMethods= mClass.getDeclaredMethods();
       for (Method mMethod:mMethods) {
           System.out.println(mMethod);
       }
```
```
D:\java\jdk\bin\java
public void cn.edu.nuc.Student.eat(int)
public void cn.edu.nuc.Student.say()
private void cn.edu.nuc.Student.shengao()
protected void cn.edu.nuc.Student.tizhong()

Process finished with exit code 0

```

* getMethods()方法返回某个类的所有公用（public）方法，包括其继承类的公用方法。

```java
       Class mClass=Student.class;
       Method[]mMethods= mClass.getMethods();
       for (Method mMethod:mMethods) {
           System.out.println(mMethod.getName());
       }
```
```
D:\java\jdk\bin\java
public void cn.edu.nuc.Student.eat(int)
public void cn.edu.nuc.Student.say()
public final void java.lang.Object.wait() throws java.lang.InterruptedException
public final void java.lang.Object.wait(long,int) throws java.lang.InterruptedException
public final native void java.lang.Object.wait(long) throws java.lang.InterruptedException
public boolean java.lang.Object.equals(java.lang.Object)
public java.lang.String java.lang.Object.toString()
public native int java.lang.Object.hashCode()
public final native java.lang.Class java.lang.Object.getClass()
public final native void java.lang.Object.notify()
public final native void java.lang.Object.notifyAll()

Process finished with exit code 0

```
* getMethod方法返回一个特定的方法，其中第一个参数为方法名称，后面的参数为方法的参数对应Class的对象

```java
       Method mMethod=mClass.getMethod("eat",int.class);
       System.out.println(mMethod.getName());

```
```
D:\java\jdk\bin\java
public void cn.edu.nuc.Student.eat(int)

Process finished with exit code 0
```
#### 4.获取构造器信息
获取类构造器的用法与上述获取方法的用法类似。主要是通过Class类的getConstructor方法得到Constructor类的一个实例，而Constructor类有一个newInstance方法可以创建一个对象实例:

```java
 public T newInstance(Object ... initargs)
```
可以根据传入的参数来调用对应的构造方法
#### 5.获取类的成员变量信息
* getFiled: 访问公有的成员变量
* getDeclaredField：访问类的所有成员变量属性，包括继承的，实现的父类的所有属性
* getFileds: 访问类的所有公有成员变量属性，包括继承的共有属性<br>
<br>
Field提供了两组方法来读取或设置成员变量的值：
getXXX(Object obj):获取obj对象的该成员变量的值。此处的XXX对应8种基本类型。如果该成员变量的类型是引用类型，则取消get后面的XXX。
setXXX(Object obj,XXX val)：将obj对象的该成员变量设置成val值。
参考上面的getMethod和getDeclaredMethods定义

```java
//生成新的对象：用newInstance()方法
Object obj = mClass.newInstance();
//获取age成员变量
Field field = mClass.getField("age");
//将obj对象的age的值设置为10
field.setInt(obj, 10);
//获取obj对象的age的值
field.getInt(obj);
```

#### 6.调用方法
当我们从类中获取了一个方法后，我们就可以用invoke()方法来调用这个方法。invoke方法的原型为:

```java
//第一个参数为实例化对象，第二个为参数
public Object invoke(Object obj, Object... args)
        throws IllegalAccessException, IllegalArgumentException,
           InvocationTargetException
```
```java
        Method mMethod=mClass.getMethod("eat",int.class);
        mMethod.invoke(mStudent,18);
```
```
D:\java\jdk\bin\java
18

Process finished with exit code 0
```
当通过Method的invoke()方法来调用对应的方法时，Java会要求程序必须有调用该方法的权限。如果程序确实需要调用某个对象的private方法，则可以先调用Method对象的如下方法。
setAccessible(boolean flag)：将Method对象的acessible设置为指定的布尔值。值为true，指示该Method在使用时应该取消Java语言的访问权限检查；值为false，指示该Method在使用时要实施Java语言的访问权限检查。

#### 实例化泛型
比如当我们遇到这样的情况
```java
public abstract class Base<P extends BaseP,M extends BaseM > {
}
```
我们想去用一个类继承这个Base，在Base中完成实例化P和M，我们可以借助反射去实现()
```java

protect P p；
protect M m;
p=getT(this,0);
m=getT(this,1);
public static <T>T getT(Object mO,int index){

      try {
          return ((Class<T>)((ParameterizedType)mO.getClass().getGenericSuperclass())
                  .getActualTypeArguments()[index])
                  .newInstance();
      } catch (InstantiationException mE) {
          mE.printStackTrace();
      } catch (IllegalAccessException mE) {
          mE.printStackTrace();
      }

      return null;
  }

```
getGenericSuperclass() 通过反射获取当前类表示的实体（类，接口，基本类型或void）的直接父类的Type，getActualTypeArguments()返回参数数组
### 反射的优缺点
优点：<br>
（1）能够运行时动态获取类的实例，大大提高系统的灵活性和扩展性。<br>
（2）与Java动态编译相结合，可以实现无比强大的功能<br>
缺点：<br>
（1）使用反射的性能较低<br>
（2）使用反射相对来说不安全<br>
（3）破坏了类的封装性，可以通过反射获取这个类的私有方法和属性<br>







### 概念
java提供了一种原程序中的元素关联任何信息和任何元数据的途径和方法

<!-- more -->

### Java中常见的注解
#### jdk注解
* @Override覆盖父类的方法
* @Deprecated过时的方法
* @SuppressWarnings("deprecation")对过时的方法进行描述

### 注解的分类
#### 按照运行机制来分
* 源码注解 注解只存在在源码中，编译成.class文件就不存在了
* 编译时注解 注解在源码和.class都存在的注解（jdk的注解）
* 运行时注解 在运行阶段还起作用，甚至会影响运行逻辑

#### 按照来源来分
* 来自jdk的注解
* 来自第三方的注解
* 自己定义的注解

### 自定义注解
#### 语法要求
```java
//@interface 进行注解的定义
public @interface Description {

 //成员以无参无异常形式声明,这里并不是方法
 String desc();

 String author();
 //给成员定义默认的值
 int age()default 18;

}
```
* 成员类型是受限制的，合法的类型包括原始类型和String，Class，Annotation，Enumeration
* 如果注解只有一个成员，则成员必须取名为value(),在使用时候可以忽略成员名和复制号（=）
* 注解类可以没有成员，没有成员的注解称为标识注解

#### 元注解
#####  @Target()注解的定义域
* ElementType.CONSTRUCTOR 构造方法声明
* ElementType.FIELD 字段声明
* ElementType.LOCAL_VARABLE 局部变量声明
* ElementType.METHOD 方法声明
* ElementType.PACKAGE 包声明
* ElementType.PARAMETER 参数声明
* ElementType.TYPE  类，接口

##### @Retention()
* RetentionPolicy.SOURCE 只在源码显示，编译时会丢弃
* Retention.CLASS 编译时会记录到class中，运行时候忽略
* RetentionPolicy.RUNTIME 运行时候存在，可以通过反射读取

##### @Inherited()
允许子类继承 实现接口继承没有用，只能是class，而且只能继承类名上面的注解

##### @Documented
生成Javadoc时包含注解

#### 使用自定义注解的语法
@<注解名>(<成员名1>=<成员值1>,<成员名2>=<成员值2>,........)<br>
@Description(desc = "hello",author = "world",age = 18)

### java8和注解

* @Repeatable
说明该注解标识的注解可以多次使用到同一个元素的声明上。 看一个使用的例子。首先我们创造一个能容纳重复的注解的容器：

```java
\
/**
 * Container for the {@link CanBeRepeated} Annotation containing a list of values
*/
@Retention( RetentionPolicy.RUNTIME )
@Target( ElementType.TYPE_USE )
public @interface RepeatedValues
{
 CanBeRepeated[] value();
}
```
接着，创建注解本身，然后标记@Repeatable

```java
@Retention( RetentionPolicy.RUNTIME )
@Target( ElementType.TYPE_USE )
@Repeatable( RepeatedValues.class )
public @interface CanBeRepeated
{
 String value();
}
```
最后，我们可以这样重复地使用：

```java
@CanBeRepeated( "the color is green" )
@CanBeRepeated( "the color is red" )
@CanBeRepeated( "the color is blue" )
public class RepeatableAnnotated
{

}
```
* 自Java8开始，我们可以在类型上使用注解。由于我们在任何地方都可以使用类型，包括 new操作符，casting，implements，throw等等。注解可以改善对Java代码的分析并且保证更加健壮的类型检查。这个例子说明了这一点：

```java
@SuppressWarnings( "unused" )
public static void main( String[] args )
{
 // type def
 @TypeAnnotated
 String cannotBeEmpty = null;

 // type
 List<@TypeAnnotated String> myList = new ArrayList<String>();

 // values
 String myString = new @TypeAnnotated String( "this is annotated in java 8" );

}
 // in method params
public void methodAnnotated( @TypeAnnotated int parameter )
{
 System.out.println( "do nothing" );
 ```

 * @FunctionalInterface：

 这个注解表示一个函数式接口元素。函数式接口是一种只有一个抽象方法（非默认）的接口。编译器会检查被注解元素，如果不符，就会产生错误。例子如下：


 ```java

 // implementing its methods
@SuppressWarnings( "unused" )
MyCustomInterface myFuncInterface = new MyCustomInterface()
{

 @Override
 public int doSomething( int param )
 {
 return param * 10;
 }
};

// using lambdas
@SuppressWarnings( "unused" )
 MyCustomInterface myFuncInterfaceLambdas = ( x ) -> ( x * 10 );
}

@FunctionalInterface

interface MyCustomInterface
{
/*
 * more abstract methods will cause the interface not to be a valid functional interface and
 * the compiler will thrown an error:Invalid '@FunctionalInterface' annotation;
 * FunctionalInterfaceAnnotation.MyCustomInterface is not a functional interface
 */
 // boolean isFunctionalInterface();

 int doSomething( int param );
 }


 ```
 这个注解可以被使用到类，接口，枚举和注解本身。它的被JVM保留并在runtime可见，这个是它的声明：

 ```java

 @Documented
  @Retention(value=RUNTIME)
  @Target(value=TYPE)
 public @interface FunctionalInterface
 ```





### 解析注解
```java

@Target({ElementType.TYPE,ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
public @interface Description {

    String value();
}

public interface Person {

    String name();
}


@Description("this name class")
public class Child implements Person {
    @Override
    @Description("this name is method")
    public String name() {
        return null;
    }
}


import java.lang.reflect.Method;

/**
 * Created by Administrator on 2017/4/22.
 */
public class ParseAnn {
    public static void main(String[] args) {
        //使用类加载器加载类
        try {
            Class mClass=Class.forName("cn.edu.nuc.Child");
            //找到类上面的注解
            boolean is=mClass.isAnnotationPresent(Description.class);//检查传入的注解是否存在于当前元素。
            if (is) {
                //拿到注解的实例
                Description mDescription= (Description) mClass.getAnnotation(Description.class);//按照传入的参数获取指定类型的注解。返回null说明当前元素不带有此注解
                System.out.println(mDescription.value());
            }

            //找到方法的注解
            Method[]ms=mClass.getMethods();
            for (Method mMethod:ms) {
                boolean isM=mMethod.isAnnotationPresent(Description.class);
                if (isM){
                    Description mDescription=mMethod.getAnnotation(Description.class);
                    System.out.println(mDescription.value());
                }

            }

            //另一种找到注解的方法
            for (Method mMethod:ms) {
                Annotation []mAnnotations=mMethod.getAnnotations();//返回该元素的所有注解，包括没有显式定义该元素上的注解
                for (Annotation mAnnotation:mAnnotations){
                    if (mAnnotation instanceof  Description){
                        System.out.println(((Description) mAnnotation).value());
                    }
                }
            }

        } catch (ClassNotFoundException mE) {
            mE.printStackTrace();
        }

    }
}

```
结果

```java
D:\java\jdk\bin\java
this name class
this name is method
this name is method


Process finished with exit code 0

```
//注意接口是不能继承的,所以换成class，我们看下方法上的注解能不能继承
```java
public class Child extends Person {
    @Override

    public String name() {
        return null;
    }
}
@Description("this name class")
public class Person {

    @Description("this name is method")
    public String name(){
        return null;
    }
}

```
结果

```java
D:\java\jdk\bin\java
this name class

Process finished with exit code 0
```
很明显不能

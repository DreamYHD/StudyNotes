[string　常用的　ＡＰＩ](http://www.apihome.cn/api/java/String.html)  
[原文链接　ｓｔｒｉｎｇ中常见的１０个问题](http://www.programcreek.com/2013/09/top-10-faqs-of-java-strings/)  

#### 1. 字符串比较,使用 "==" 还是 equals() ?
简单来说, "==" 判断两个引用的是不是同一个内存地址(同一个物理对象).
而 equals 判断两个字符串的值是否相等.
除非你想判断两个string引用是否同一个对象,否则应该总是使用 equals()方法.
如果你了解 字符串的驻留 ( String Interning ) 则会更好地理解这个问题

#### 2. 对于敏感信息,为何使用char[]要比String更好?
String是不可变对象, 意思是一旦创建,那么整个对象就不可改变. 即使新手觉得String引用变了,实际上只是(指针)引用指向了另一个(新的)对象.
而程序员可以明确地对字符数组进行修改,因此敏感信息(如密码)不容易在其他地方暴露(只要你用完后对char[]置0).

#### 3. 在switch语句中使用String作为case条件?
从 JDK7 开始,这是可以的,啰嗦一句,Java 6 及以前的版本都不支持这样做.
```java
// 只在java 7及更高版本有效!  
switch (str.toLowerCase()) {  
      case "a":  
           value = 1;  
           break;  
      case "b":  
           value = 2;  
           break;  
}  
```
#### 4. 转换String为数字
对于非常大的数字请使用Long,代码如下
```java
int age = Integer.parseInt("10");  
long id = Long.parseLong("190"); // 假如值可能很大.  
```
#### 5. 如何通过空白字符拆分字符串
String 的 split()方法接收的字符串会被当做正则表达式解析,
"\s"代表空白字符,如空格" ",tab制表符"\t", 换行"\n",回车"\r".
而编译器在对源代码解析时,也会进行一次字面量转码,所以需要"\\s".
```java
String[] strArray = aString.split("\\s+");  
```

#### 6. substring()  方法内部是如何处理的?
在JDK6中,substring()方法还是共用原来的char[]数组,通过偏移和长度构造了一个"新"的String。
想要substring()取得一个全新创建的对象,使用如下这种方式:
```java
String sub = str.substring(start, end) + "";  

```
 "hamburger".substring(4, 8) returns "urge"<br>
 "smiles".substring(1, 5) returns "mile<br>

当然 Java 7 中,substring()创建了一个新的char[] 数组,而不是共用.
想要了解更多,请参考:  JDK6和JDK7中substring()方法及其差异

#### 7. String vs StringBuilder vs StringBuffer
StringBuilder 是可变的,因此可以在创建以后修改内部的值.
StringBuffer 是同步的,因此是线程安全的,但效率相对更低.


#### 8. 如何重复拼接同一字符串?
方案1: 使用Apache Commons Lang 库的 StringUtils 工具类.
```java
String str = "abcd";  
String repeated = StringUtils.repeat(str,3);//abcdabcdabcd  
```

方案2:
使用 StringBuilder 构造. 更灵活.
```java
String src = "name";  
int len = src.length();  
int repeat = 5;  
StringBuilder builder = new StringBuilder(len * repeat);  
for(int i=0; i<repeat; i++){  
  builder.append(src);  
}  
String dst = builder.toString();  
```

#### 9. 如何将String转换为日期?
```java
SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd");  
String str = "2013-11-07";  
Date date = format.parse(str);  
System.out.println(format.format(date));//2013-11-07  
```

#### 10. 如何统计某个字符出现的次数?
同样使用Apache Commons Lang 库 StringUtils  类:
```java
int n = StringUtils.countMatches("11112222", "1");  
System.out.println(n);  
```

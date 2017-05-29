## Java IO
![](http://onf44qqgp.bkt.clouddn.com/17-5-29/11308849.jpg)

### File的基本操作
![](http://onf44qqgp.bkt.clouddn.com/17-5-29/26487206.jpg)
![](http://onf44qqgp.bkt.clouddn.com/17-5-29/36579275.jpg)
#### 创建一个新的文件
```java
public class Test {
    public static void main(String[] args) {
        File mFile=new File("f://hello");
        if (!mFile.exists()) {
            try {
                mFile.createNewFile();
            } catch (IOException mE) {
                mE.printStackTrace();
            }
        }

    }
}

```

#### File的两个常量
```java
public class Test {
    public static void main(String[] args) {
        System.out.println(File.separator);
        System.out.println(File.pathSeparator);

    }
}
D:\java\jdk\bin\java "-javaagent

\
;

Process finished with exit code 0
```

#### 删除一个文件
```java
public class Test {
    public static void main(String[] args) {
        File mFile=new File("f://hello");
        if (mFile.exists()) {
            mFile.delete();
        }


    }
}


```

#### 创建一个文件夹
```java
public class Test {
    public static void main(String[] args) {
        File mFile=new File("f://hello");
        if (!mFile.isDirectory()) {
            mFile.mkdirs();
        }


    }
}

```

#### 列出指定目录的全部文件（包括隐藏文件）：\
```java
public class Test {
    public static void main(String[] args) {
        File mFile=new File("f://hello");
        if (!mFile.isDirectory()) {
            mFile.mkdirs();
        }
        String[]mStrings=mFile.list();
        for (int i = 0; i <mStrings.length ; i++) {
            System.out.println(mStrings[i]);
        }
        File[] mFiles=mFile.listFiles();
        for (int i = 0; i <mFiles.length ; i++) {
            System.out.println(mFiles[i]);
        }
    }
}

D:\java\jdk\bin\java "-javaagent
hello1
tet2.txt
txt1.txt
f:\hello\hello1
f:\hello\tet2.txt
f:\hello\txt1.txt

Process finished with exit code 0
```

#### 搜索指定目录的全部内容
```java
public class Test {
    public static void main(String[] args) {
        File mFile=new File("f://hello");
        if (!mFile.isDirectory()) {
            mFile.mkdirs();
        }
        printAll(mFile);

    }

    private static void printAll(File mFile) {
        if (mFile == null) {
            return;
        }
        if (mFile.isDirectory()) {
            File[]mFiles=mFile.listFiles();
            if (mFiles!=null&&mFiles.length!=0) {
                for (int i = 0; i < mFiles.length; i++) {
                    printAll(mFiles[i]);
                    System.out.println(mFiles[i]);
                }
            }else {
                return;
            }
        }
    }
}


D:\java\jdk\bin\java "
f:\hello\hello1\hello2\text4.txt
f:\hello\hello1\hello2
f:\hello\hello1\txt3.txt
f:\hello\hello1
f:\hello\tet2.txt
f:\hello\txt1.txt

Process finished with exit code 0
```
#### 输出以txt结尾的文件名
```java
public class Test {
    public static void main(String[] args) {
        File mFile=new File("f://hello");
        if (!mFile.isDirectory()) {
            mFile.mkdirs();
        }
        printAll(mFile);

    }

    private static void printAll(File mFile) {
        if (mFile == null) {
            return;
        }
        if (mFile.isDirectory()) {
            File[]mFiles=mFile.listFiles();
            if (mFiles!=null&&mFiles.length!=0) {
                for (int i = 0; i < mFiles.length; i++) {
                    printAll(mFiles[i]);
                    if (mFiles[i].toString().endsWith(".txt")) {

                    }
                    System.out.println(mFiles[i]);
                }
            }else {
                return;
            }
        }
    }
}
D:\java\jdk\bin\java 
f:\hello\hello1\hello2\text4.txt
f:\hello\hello1\hello2
f:\hello\hello1\txt3.txt
f:\hello\hello1
f:\hello\hello5
f:\hello\tet2.txt
f:\hello\txt1.txt

Process finished with exit code 0
```
### 字节流
#### 向文件中写入字符串
```java
public class Test {
    public static void main(String[] args) {
        File mFile=new File("f://hello//tex1.txt");
        try {
            FileOutputStream mFileOutputStream=new FileOutputStream(mFile);
            String mS="hello";
            byte[]mBytes=mS.getBytes();
            mFileOutputStream.write(mBytes);
            mFileOutputStream.close();
        } catch (FileNotFoundException mE) {
            mE.printStackTrace();
        } catch (IOException mE) {
            mE.printStackTrace();
        }

    }
}

```
#### 一个字节一个字节写
```java
public class Test {
    public static void main(String[] args) {
        File mFile=new File("f://hello//tex1.txt");
        try {
            FileOutputStream mFileOutputStream=new FileOutputStream(mFile);
            String mS="hello";
            byte[]mBytes=mS.getBytes();
            for (int i = 0; i <mBytes.length ; i++) {

                mFileOutputStream.write(mBytes[i]);
            }
            mFileOutputStream.close();
        } catch (FileNotFoundException mE) {
            mE.printStackTrace();
        } catch (IOException mE) {
            mE.printStackTrace();
        }

    }
}

```
#### 向文件中追加新内容
```java
public class Test {
    public static void main(String[] args) {
        File mFile=new File("f://hello//tex1.txt");
        try {
            FileOutputStream mFileOutputStream=new FileOutputStream(mFile,true);
            String mS="world";
            byte[]mBytes=mS.getBytes();
            for (int i = 0; i <mBytes.length ; i++) {

                mFileOutputStream.write(mBytes[i]);
            }
            mFileOutputStream.close();
        } catch (FileNotFoundException mE) {
            mE.printStackTrace();
        } catch (IOException mE) {
            mE.printStackTrace();
        }

    }
}

```
#### 读取文件内容
```java
public class Test {
    public static void main(String[] args) {
        File mFile=new File("f://hello//tex1.txt");

        try {
            FileInputStream mFileInputStream=new FileInputStream(mFile);
            byte[]mBytes=new byte[(int) mFile.length()];
            mFileInputStream.read(mBytes);
            System.out.println(mBytes.length);
            System.out.println(new String(mBytes));

            mFileInputStream.close();
        } catch (FileNotFoundException mE) {
            mE.printStackTrace();
        } catch (IOException mE) {
            mE.printStackTrace();
        }
    }
}
```
#### 一个一个的读取
```JAVA
public class Test {
    public static void main(String[] args) {
        File mFile=new File("f://hello//tex1.txt");

        try {
            FileInputStream mFileInputStream=new FileInputStream(mFile);
            byte[]mBytes=new byte[(int) mFile.length()];
            for (int i = 0; i <mBytes.length ; i++) {
                mBytes[i]= (byte) mFileInputStream.read();
            }

            System.out.println(mBytes.length);
            System.out.println(new String(mBytes));
            mFileInputStream.close();
        } catch (FileNotFoundException mE) {
            mE.printStackTrace();
        } catch (IOException mE) {
            mE.printStackTrace();
        }
    }
}

```
#### 不知道文件大小的情况下读取
```java
public class Test {
    public static void main(String[] args) {
        File mFile=new File("f://hello//tex1.txt");

        try {
            FileInputStream mFileInputStream=new FileInputStream(mFile);
            byte[]mBytes=new byte[1024];
            int len=0;
            int index=0;
            while ((len = mFileInputStream.read())!=-1) {
                mBytes[index++]= (byte) len;
            }
            System.out.println(index--);
            System.out.println(new String(mBytes));
            mFileInputStream.close();
        } catch (FileNotFoundException mE) {
            mE.printStackTrace();
        } catch (IOException mE) {
            mE.printStackTrace();
        }
    }
}

```
### 字符流
#### 向文件中写入数据
```java
public class Test {
    public static void main(String[] args) {
        File mFile=new File("f://hello//tex1.txt");
        try {
            FileWriter mFileWriter=new FileWriter(mFile);
            mFileWriter.write("helloAndroid");
            mFileWriter.close();
        } catch (IOException mE) {
            mE.printStackTrace();
        }
    }
}

```
#### 从文件中读内容
```java
public class Test {
    public static void main(String[] args) {
        File mFile=new File("f://hello//tex1.txt");
        try {
            FileReader mFileReader=new FileReader(mFile);
            char []mChars=new char[1024];
            mFileReader.read(mChars);
            System.out.println(new String(mChars));
        } catch (IOException mE) {
            mE.printStackTrace();
        }
    }
}

```
#### 当然最好采用循环读取的方式
```java
public class Test {
    public static void main(String[] args) {
        File mFile=new File("f://hello//tex1.txt");
        try {
            FileReader mFileReader=new FileReader(mFile);
            char []mChars=new char[1024];
            int len=0;
            int index=0;
            while ((len=mFileReader.read())!=-1){
                mChars[index++]= (char) len;

            }
            System.out.println(index--);
            System.out.println(new String(mChars));
        } catch (IOException mE) {
            mE.printStackTrace();
        }
    }
}

```
### 应用

#### 文件复制(图片之类的复制使用字节流，文本一般是使用字符流)
```java
public class Test {
    public static void main(String[] args) {
        File mFile=new File("f://hello//tex1.txt");
        File mFile1=new File("f://hello//tex1Copy.txt");
        try {
            FileInputStream mFileInputStream=new FileInputStream(mFile);
            FileOutputStream mFileOutputStream=new FileOutputStream(mFile1);
            int len=0;
            while ((len=mFileInputStream.read())!=-1){
                mFileOutputStream.write(len);
            }
            mFileInputStream.close();
            mFileOutputStream.close();
        } catch (FileNotFoundException mE) {
            mE.printStackTrace();
        } catch (IOException mE) {
            mE.printStackTrace();
        }

    }
}

```
#### 将字节输出流转化为字符输出流
```java
public class Test {
    public static void main(String[] args) {

        File file=new File("f://hello//tex1.txt");
        Writer out= null;
        try {
            out =  new OutputStreamWriter(new FileOutputStream(file));
            out.write("hello");
            out.close();
        } catch (FileNotFoundException mE) {
            mE.printStackTrace();
        } catch (IOException mE) {
            mE.printStackTrace();
        }

    }
}
```
#### 将字节输入流变为字符输入流
```JAVA
public class Test {
    public static void main(String[] args) {

        File file=new File("f://hello//tex1.txt");
        Reader read= null;
        try {
            read = new InputStreamReader(new FileInputStream(file));
            char[] b=new char[100];
            int len=0;
            int index=0;
            while ((len=read.read())!=-1){
                b[index++]= (char) len;
            }
            System.out.println(new String(b));
            read.close();
        } catch (FileNotFoundException mE) {
            mE.printStackTrace();
        } catch (IOException mE) {
            mE.printStackTrace();
        }

    }
}
```
### 缓存流
``` java
public class Test {
    public static void main(String[] args) {
        
        try {
            FileReader in = new FileReader("f://hello//tex1.txt");
            FileWriter out = new FileWriter("f://hello//tex1Copy.txt");
            BufferedReader br = new BufferedReader(in);
            BufferedWriter bw = new BufferedWriter(out);
            String str = null;
            while ((str = br.readLine()) != null){
                bw.write(str);
                bw.newLine();
                bw.flush();
            }
            in.close(); 
            out.close();
            br.close();
            bw.close();
        } catch (FileNotFoundException mE) {
            mE.printStackTrace();
        } catch (IOException mE) {
            mE.printStackTrace();
        }
       
    }
}
```















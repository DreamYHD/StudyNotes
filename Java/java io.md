#### 找出d盘中所有以.txt结尾的文件
```java
public class Test {

    public static void selectFile(String fileName){
        File file=new File(fileName);
        File []files=file.listFiles();
        if (files == null||files.length==0) {
            return;
        }
        for (File f:files) {
            if (f.isDirectory()){
                selectFile(f.getPath());
                File[]list=f.listFiles(new MyFilter());
                if (list != null||list.length!=0) {
                    for (File m:list) {
                         System.out.println(m.getPath());
                    }
                }
            }else {
            }
        }
    }
    public static void main(String[] args){
        String path="f:\\";
        selectFile(path);
    }
}
```
```java
public class MyFilter implements FileFilter {
    @Override
    public boolean accept(File pathname) {
        if (pathname.getName().endsWith(".txt")) {
            return  true;
        }
        return false;
    }
}

```

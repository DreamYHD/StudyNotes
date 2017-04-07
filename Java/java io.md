#### 找出f盘中所有以.txt结尾的文件
```java
public class Test {	
	public static void  find(File file) {
		if (file.isDirectory()) {
			File []filesF=file.listFiles(new FileFilter() {
				
				@Override
				public boolean accept(File pathname) {
					if (pathname.getName().endsWith(".bqy")) {
						return true;					
					}
					return false;
				}
			});
			if (filesF!=null&&filesF.length!=0) {
				for (File file2 : filesF) {
					System.out.println(file2);
				}
			}
		
			File []files=file.listFiles();
			if (files!=null&&files.length!=0) {
		        for (File file2 : files) {
				if (file2.isDirectory()) {
					find(file2);
				}else {
					
				}
			   }
				
			}
			
		}
	
	}
	
	
	public static void main(String[] args) {
		
		File file=new File("f:\\");
                find(file);	
      

	}

}


```
#### 文件复制

### Java基础知识--char数组通过StringBuilder转换为字符串问题

**背景**

​		此文章只是记录下本人遗忘的基础知识点。

​		在一次写算法题的时候，声明了一个char类型二维数组，最终通过StringBuilder转换成字符串。在这期间出现了一个基础知识让我想记录下，具体过程如下。

```java
// 测试代码如下

public class TestMain {

	public static void main(String[] args) {
		char [][] cs = new char[10][10];
		StringBuilder sb = new StringBuilder();
		for(int i=0;i<10;i++){
			for(int j=0;j<10;j++){
				sb.append(cs[i][j]);
			}
		}
		sb.append("end");
		for(char a : sb.toString().toCharArray()){
			System.out.print(a);
		}
	}	
}
// 结果如下
//                                                                             end
```

开始很疑惑为什么会出现这么多空白后才出现end字符，直接想到使用sb.toString().replace(" ","")后再输出结果

```java
public class TestMain {

	public static void main(String[] args) {
		char [][] cs = new char[10][10];
		StringBuilder sb = new StringBuilder();
		for(int i=0;i<10;i++){
			for(int j=0;j<10;j++){
				sb.append(cs[i][j]);
			}
		}
		String string = sb.append("end").toString().replace(" ", "");
		System.out.println(string);
		for(char a : string.toCharArray()){
			System.out.print(a);
		}
	}	
}
// 输出结果如下
//                                                                           end
//                                                                           end
```

这下想到去看下char字符的默认值，然后了解到char字符默认值为"u0000"，随后恍然大悟

```java
public class TestMain {

	public static void main(String[] args) {
		char [][] cs = new char[10][10];
		StringBuilder sb = new StringBuilder();
		for(int i=0;i<10;i++){
			for(int j=0;j<10;j++){
				sb.append(cs[i][j]);
			}
		}
		
		for(char a : sb.toString().toCharArray()){
			System.out.print((int)a);
		}
		
		String string = sb.append("end").toString().replace("\0", "");
		System.out.println(string);
		for(char a : string.toCharArray()){
			System.out.print(a);
		}
	}
}
// 输出结果如下
//0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000end
//end
```

**总结** 

​		平时工作中用到的char类型不太多，所以时间长了基础知识有些忘记，今天恰好遇到一个问题，所以随手记录下，希望以后这块能不再出类似尴尬的问题。
## 数组

> 数组是相同类型数据的有序集合

#### 数组的特点：
1. java中数组是定长的，大小不能被改变
2. 数组中的元素必须是相同类型
3. 数组可以是任意类型

```java
	int[] arr01=new int[10];  
	String[] arr02=new String[5];
	int[] a={1,2,3};  //静态初始化
	int[] b=new int[3];	//默认 0，fales，null
	b[0]=1;				//动态初始化
	
```


#### arraycopy()

    public static void main(String[] args){
		System.arraycopy(src,srcPos,dest,destPos,length);
		System.arraycopy(arr, 0, copyArr, 0, arr.length);
	}
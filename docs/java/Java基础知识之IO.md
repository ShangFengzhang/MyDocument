## IO


#### File常用操作
分隔符

		//String path = "D:\\Git\\JavaBasic\\road.jpg";
		//字符串分隔符
        System.out.println(File.separatorChar);
        String path = "D:/Git/JavaBasic/road.jpg";
        System.out.println(path);
		//常量拼接
        path = "D:" + File.separator + "Git" + File.separator + "JavaBasic" + File.separator + "road.jpg";
        System.out.println(path);

File对象

        /* 构建file对象
         * 绝对路径：存在盘符
         * 相对路径：不存在盘符
         * */
        String path = "D:\\Git\\JavaBasic\\road.jpg";
        File src = new File(path);
        System.out.println(src.length());
        src = new File("D:/Git/JavaBasic", "road.jpg");
        System.out.println(src.length());
        //相对于当前项目根目录
        src = new File("a.text");
        System.out.println(src.getAbsolutePath());
基本信息

        File file = new File("D:/Git/JavaBasic/road.jpg");
        System.out.println("名称："+file.getName());
        System.out.println("路径："+file.getPath());   //根据构建时候的路径返回
        System.out.println("绝对路径："+file.getAbsolutePath());
        System.out.println("父路径："+file.getParent());    //返回上一级路径，null

文件状态

        File file = new File("D:/Git/JavaBasic/road.jpg");
        System.out.println("是否存在"+file.exists());
        System.out.println("是否文件"+file.isFile());
        System.out.println("是否文件夹"+file.isDirectory());
        File noFile = new File("D:/Git/JavaBasic/a.jpg");
        System.out.println("是否存在" + noFile.exists());
        System.out.println("是否文件" + noFile.isFile());
        System.out.println("是否文件夹" + noFile.isDirectory());
        File directory = new File("D:/Git/JavaBasic/");
        System.out.println("是否文件夹" + directory.isDirectory());

文件的创建和删除

> 在典型 ext4 文件系统( 大多数人使用的) 上，默认的inode 大小为 256字节，block 大小为 4096字节。
> 目录只是一个特殊文件，其中包含文件名和inode编号的array。 创建目录时，文件系统分配给"文件名"( 实际上是目录名称为)的目录 1. inode指向单个数据 block ( 最低开销)，它是 4096字节。 

        File file = new File("D:/Git/JavaBasic/road.jpg");
        System.out.println("文件长度(文件大小)" + file.length());
        File directory = new File("D:/Git/JavaBasic");
        System.out.println("文件长度" + directory.length());
        File newFile = new File("D:/Git/JavaBasic/a.txt");
        System.out.println("创建文件(已存在不会在创建)：" + newFile.createNewFile());
        System.out.println("删除已存在的文件：" + newFile.delete());

目录

        File dir = new File("D:/Git/JavaBasic/test");
        dir.mkdir();    //确保上级目录存在，不存在会失败
        dir.mkdirs();   //上级目录可以不存在
        String[] dirNames=dir.list();     //列出下一级名称
        File[] files=dir.listFiles();   //列出下一级文件对象
        File.listRoots();           //列出所有盘符

乱码：  
1. 字节数不够  
2. 字符集不统一




### IO流
![IO流](https://gitee.com/zhangshangfeng/MyDocument/raw/master/docs/picture/io.jpg)





#### 文件字节流操作（FileInputStream，FileOutputStream）


		//1、创建源
        File file = new File("abc.txt");
        FileInputStream fileInputStream = null;
        try {
			// 2、选择流
            fileInputStream = new FileInputStream(file);
			//3、操作
            int temp;
            while ((temp = fileInputStream.read()) != -1) {
                System.out.println((char) temp);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //  4、释放资源
            try {
                if (null != fileInputStream) {
                    fileInputStream.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

----------------------

        //1、创建源
        File file = new File("abc.txt");
        FileInputStream fileInputStream = null;
        try {
            //2、选择流
            fileInputStream = new FileInputStream(file);
            byte[] car = new byte[3];   //缓冲容器
            int len = -1; //接收长度
            //3、操作
            while ((len = fileInputStream.read(car)) != -1) {
                String str = new String(car, 0, len);
                System.out.println(str);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4、释放资源
            try {
                if (null != fileInputStream) {
                    fileInputStream.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

-------------

    private void copy(String filePath,String copyFilePath) {
        File file = new File(filePath);
        File copyFile = new File(copyFilePath);
        FileInputStream fileInputStream = null;
        FileOutputStream fileOutputStream = null;
        try {
            try {
                fileInputStream = new FileInputStream(file);
                fileOutputStream = new FileOutputStream(copyFile);
                byte[] flush = new byte[1024];
                int len = -1;
                while ((len = fileInputStream.read(flush)) != -1) {
                    fileOutputStream.write(flush, 0, len);
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        } finally {
            if (null != fileInputStream) {
                try {
                    fileInputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (null != fileOutputStream) {
                try {
                    fileOutputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

#### 文件字符流操作（Reader，Writer）



        //1、创建源
        File file = new File("abc.txt");
        Reader reader = null;
        try {
            //2、选择流
            reader = new FileReader(file);
            char[] flush = new char[1024];   //缓冲容器
            int len = -1; //接收长度
            //3、操作
            while ((len = reader.read(flush)) != -1) {
                String str = new String(flush, 0, len);
                System.out.println(str);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4、释放资源
            try {
                if (null != reader) {
                    reader.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }


----------------

        File file = new File("dest.txt");
        Writer writer = null;
        try {
            writer = new FileWriter(file);
			//写法一
            String msg = "我爱你2";
            char[] data = msg.toCharArray();
            writer.write(data, 0, data.length);
            //写法二
			//writer.write(msg);
            //写法三
			//writer.append("wanan");
            writer.flush();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (null != writer) {
                try {
                    writer.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }



#### 字节数组流（ByteArrayInputStream，ByteArrayOutputStream）


> 文件流访问硬盘数据，java虚拟机需要借助操作系统进行访问，所以需要释放，字节数组流直接访问内存，所以不用释放，将文件转换成字节数组方便网络传输，字节数组不宜过大



        //1、创建源
        byte[] file = "talk is cheap show me the code".getBytes();
        InputStream inputStream = null;
        try {
            //2、选择流
            inputStream = new ByteArrayInputStream(file);
            byte[] flush = new byte[5];   //缓冲容器
            int len = -1; //接收长度
            //3、操作
            while ((len = inputStream.read(flush)) != -1) {
                String str = new String(flush, 0, len);
                System.out.println(str);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4、释放资源
            try {
                if (null != inputStream) {
                    inputStream.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

-------------

        //1、创建源
        byte[] dest = null;
        ByteArrayOutputStream outputStream = null;
        try {
            //2、选择流
            outputStream = new ByteArrayOutputStream();
            String msg = "hello";
            byte[] data = msg.getBytes();   //缓冲容器
            outputStream.write(data,0,data.length);
            outputStream.flush();
            dest=outputStream.toByteArray();
            System.out.println(dest.length+"--->"+new java.lang.String(dest,0,outputStream.size()));
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4、释放资源
            try {
                if (null != outputStream) {
                    outputStream.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }



#### 图片和字节数组的转换
![图片和字节数组](https://gitee.com/zhangshangfeng/MyDocument/raw/master/docs/picture/fileToArray.png)

    public static byte[] fileToByteArray(String filePath) {
        /*
         * 1、读取图片到字节数组
         * 2、图片到程序FileInputStream
         * 3、程序到字节数组ByteArrayOutputStream*/
        File file = new File(filePath);
        InputStream inputStream = null;
        ByteArrayOutputStream outputStream = null;
        try {
            inputStream = new FileInputStream(file);
            outputStream = new ByteArrayOutputStream();
            byte[] flush = new byte[1024 * 1024];
            int len = -1;
            while ((len = inputStream.read(flush)) != -1) {
                outputStream.write(flush, 0, len);
            }
            outputStream.flush();
            return outputStream.toByteArray();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (null != inputStream) {
                try {
                    inputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
        return null;


    }

    public static File byteArrayToFile(byte[] src, String filePath) {
        /*
         * 字节数组写出到图片
         * 1、字节数数组到程序 ByteInputStream
         * 2、程序到文件 FileOutputStream*/
        File file = new File(filePath);
        InputStream inputStream = null;
        OutputStream outputStream = null;
        try {
            inputStream = new ByteArrayInputStream(src);
            outputStream = new FileOutputStream(file);
            byte[] flush = new byte[1024 * 1024];
            int len = -1;
            if ((len = inputStream.read(flush)) != -1) {
                outputStream.write(flush, 0, len);
            }
            return file;
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (null != outputStream) {
                try {
                    outputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
        return null;
    }

#### 文件工具类

	package com.zsf.java.basic.oop.file;

	import java.io.*;

	/**
	 * @program: JavaBasic
 	 * @description: 文件工具类
  	 * @author: Zhang Shangfeng
  	 * @create: 2020-07-21 16:30
	 **/
	public class FileUtils {

    /*  java 9以上使用
     * try...with...resource*/
    public static void copy(InputStream inputStream, OutputStream outputStream) {
        try (inputStream; outputStream) {
            byte[] flush = new byte[1024];
            int len;
            while ((len = inputStream.read(flush)) != -1) {
                outputStream.write(flush, 0, len);
            }
            outputStream.flush();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }


    public static void copy(InputStream inputStream, OutputStream outputStream) {
        try {
            byte[] flush = new byte[1024];
            int len;
            while ((len = inputStream.read(flush)) != -1) {
                outputStream.write(flush, 0, len);
            }
            outputStream.flush();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            close(inputStream, outputStream);
        }
    }
	

    /*释放资源*/
    public static void close(Closeable... ios) {
        for (Closeable io : ios) {
            if (null != io) {
                try {
                    io.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
	}




#### 字节缓冲流（装饰流，BufferedInputStream，BufferedOutputStream）
> io流使用了装饰器模式

1. 缓冲流提高了性能
2. 无论对IO流进行怎样的嵌套处理，最后底层一定是节点流
3. 释放时只需释放最外层的处理流，如果想要手动释放，应从内到外依次释放




        File file = new File("abc.txt");
        InputStream inputStream = null;
        try {
            inputStream = new BufferedInputStream(new FileInputStream(file));
            byte[] flush = new byte[1024];
            int len = -1;
            while ((len = inputStream.read()) != -1) {
                String s = new String(flush, 0, len);
                System.out.println(s);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (null != inputStream) {
                try {
                    inputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

----------
  
        File file = new File("dest.txt");
        OutputStream outputStream = null;
        try {
            outputStream = new BufferedOutputStream(new FileOutputStream(file));
            String s = "java is the best language";
            byte[] datas = s.getBytes();
            outputStream.write(datas, 0, datas.length);
            outputStream.flush();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (null != outputStream) {
                try {
                    outputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

#### 字符缓冲流

        File file=new File("abc,txt");
        BufferedReader reader=null;
        try {
            reader=new BufferedReader(new FileReader(file));
            String line=null;
            while ((line=reader.readLine())!=null){
                System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

------
        File file = new File("abc,txt");
        BufferedWriter writer = null;
        try {
            writer = new BufferedWriter(new FileWriter(file));
            writer.append("java is not easy");
            writer.newLine();
            writer.append("java is not easy");
            writer.flush();
        } catch (IOException e) {
            e.printStackTrace();
        }

#### 转换流（操作纯文本，指定字符集）
从字节流转换到字符流  
InputStreamReader   
OutputStreamWriter   

        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new OutputStreamWriter(System.out));
        try {
            String msg="";
            while (!msg.equals("exit")){
                msg=bufferedReader.readLine();
                bufferedWriter.write(msg);
                bufferedWriter.newLine();
                bufferedWriter.flush();
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {

            try {
                bufferedReader.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                bufferedWriter.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

#### 数据流（DataInputStream,DataOutputStream）
将数据写入字节流，**按顺序读写**

        ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
        DataOutputStream dataOutputStream=new DataOutputStream(new BufferedOutputStream(outputStream));

        dataOutputStream.writeUTF("代码使我快乐");
        dataOutputStream.writeInt(20);
        dataOutputStream.writeBoolean(false);
        dataOutputStream.writeChar('a');
        dataOutputStream.flush();
        byte[] data=outputStream.toByteArray();

        DataInputStream dataInputStream = new DataInputStream(new BufferedInputStream(new ByteArrayInputStream(data)));

        String msg=dataInputStream.readUTF();
        int age=dataInputStream.readInt();
        boolean flag=dataInputStream.readBoolean();
        char ch=dataInputStream.readChar();
        System.out.println(msg);
        System.out.println(age);
        System.out.println(flag);
        System.out.println(ch);
#### 对象流
**序列化**  
只有支持Serializable接口的对象才能写入流中  
obj需要手动进行转化  
不需要序列化的数据 transient

        ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
        ObjectOutputStream objectOutputStream = new ObjectOutputStream(new BufferedOutputStream(outputStream));

        objectOutputStream.writeUTF("代码使我快乐");
        objectOutputStream.writeInt(20);
        objectOutputStream.writeBoolean(false);
        objectOutputStream.writeChar('a');
        objectOutputStream.writeObject("不要996");
        objectOutputStream.writeObject(new Person("java", "18"));


        objectOutputStream.flush();
        byte[] data = outputStream.toByteArray();


        ObjectInputStream objectInputStream = new ObjectInputStream(new BufferedInputStream(new ByteArrayInputStream(data)));

        String msg = objectInputStream.readUTF();
        int age = objectInputStream.readInt();
        boolean flag = objectInputStream.readBoolean();
        char ch = objectInputStream.readChar();
        Object str = objectInputStream.readObject();
        Object person = objectInputStream.readObject();
        System.out.println(msg);
        System.out.println(age);
        System.out.println(flag);
        System.out.println(ch);
        if (str instanceof String) {
            String strObj = (String) str;
            System.out.println(strObj);
        }
        if (person instanceof Person) {
            Person personObj = (Person) person;
            System.out.println(personObj.toString());
        }


#### 打印流（PrintStream、PrintWriter）

        PrintStream printStream=System.out;
        printStream.println("打印流");
        printStream.println(true);

        printStream=new PrintStream(new BufferedOutputStream(new FileOutputStream("print.txt")),true);
        printStream.println("java");
        printStream.println("hello world");
        printStream.close();

        //重定向输出端
        System.setOut(printStream);
        System.out.println("change");
        // 重定向到控制台
        System.out.println(new PrintStream(new BufferedOutputStream(new FileOutputStream(FileDescriptor.out)),true));


#### 文件分割

#####1、面向过程
按起始位置和结束位置

        RandomAccessFile randomAccessFile = new RandomAccessFile(new File("abc.txt"), "r");
        //起始位置
        int beginPos = 2;
        int actualSize = 1026;
        randomAccessFile.seek(beginPos);

        byte[] flush = new byte[1024];
        int len;
        while ((len = randomAccessFile.read(flush)) != -1) {
            if (actualSize > len) {
                //获取所有内容
                System.out.println(new String(flush, 0, len));
                actualSize -= len;
            } else {
                //获取剩余内容
                System.out.println(new String(flush, 0, actualSize));
                break;
            }
        }
        randomAccessFile.close();

	
分块

    public static void main(String[] args) throws IOException {

        File file = new File("abc.txt");
        //总长度
        long len = file.length();
        //块长
        int blockSize = 1024;
        //块数
        int size = (int) Math.ceil(len * 1.0 / blockSize);
        System.out.println(size);
        int actualSize = blockSize > len ? (int) len : blockSize;
        for (int i = 0; i < size; i++) {
            if (i == size - 1) {
                actualSize = (int) len;
            } else {
                actualSize = blockSize;
                len -= blockSize;
            }
        }
        split(1, actualSize,file);

    }

    private static void split(int beginPos, int actualSize,File file) throws IOException {
        RandomAccessFile randomAccessFile = new RandomAccessFile(file, "r");
        randomAccessFile.seek(beginPos);
        byte[] flush = new byte[1024];
        int len;
        while ((len = randomAccessFile.read(flush)) != -1) {
            if (actualSize > len) {
                //获取所有内容
                System.out.println(new String(flush, 0, len));
                actualSize -= len;
            } else {
                //获取剩余内容
                System.out.println(new String(flush, 0, actualSize));
                break;
            }
        }
        randomAccessFile.close();
    }

#### CommonIO工具集
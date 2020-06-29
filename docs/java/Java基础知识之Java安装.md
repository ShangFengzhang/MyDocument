## Java安装
1、下载  
下载地址：[https://www.oracle.com/java/technologies/javase-downloads.html](https://www.oracle.com/java/technologies/javase-downloads.html)  
![下载界面](https://gitee.com/zhangshangfeng/MyDocument/raw/master/docs/picture/jdkDownload1.png)  
![下载界面](https://gitee.com/zhangshangfeng/MyDocument/raw/master/docs/picture/jdkDownload2.png)

2、安装  
打开安装程序默认安装即可

3、配置环境变量   
右键我的电脑->属性  
![环境变量](https://gitee.com/zhangshangfeng/MyDocument/raw/master/docs/picture/jdkDownload3.png)

(1)新建->变量名"JAVA_HOME"，变量值C:\Java\jdk1.8.0_05（即JDK的安装路径）  
(2)编辑->变量名"Path"，在原变量值的最后面加上 %JAVA_HOME%\bin
之后一路点击确定

4、验证是否成功安装  
打开命令行工具，输入java  
![安装成功](https://gitee.com/zhangshangfeng/MyDocument/raw/master/docs/picture/jdkDownload4.png)  
输入java -version  
![安装成功](https://gitee.com/zhangshangfeng/MyDocument/raw/master/docs/picture/jdkDownload5.png)


5、多个版本的jdk配置
改变JAVA_HOME的路径
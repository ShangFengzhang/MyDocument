## Java安装

<h3>1.下载</h3>  

下载地址：[https://www.oracle.com/java/technologies/javase-downloads.html](https://www.oracle.com/java/technologies/javase-downloads.html)      
![下载界面](https://gitee.com/zhangshangfeng/MyDocument/raw/master/docs/picture/jdkDownload1.png)  
![下载界面](https://gitee.com/zhangshangfeng/MyDocument/raw/master/docs/picture/jdkDownload2.png)

<h3>2.安装</h3>  
打开安装程序默认安装即可

<h3>3.配置环境变量</h3>    
右键我的电脑->属性    
![环境变量](https://gitee.com/zhangshangfeng/MyDocument/raw/master/docs/picture/jdkDownload3.png)

(1)新建->变量名"JAVA_HOME"，变量值C:\Java\jdk1.8.0_05（即JDK的安装路径）  
(2)编辑->变量名"Path"，在原变量值的最后面加上 %JAVA_HOME%\bin
之后一路点击确定

<h3>4.验证是否成功安装  </h3>  

打开命令行工具，输入java    
![安装成功](https://gitee.com/zhangshangfeng/MyDocument/raw/master/docs/picture/jdkDownload4.png)  
输入java -version    
![安装成功](https://gitee.com/zhangshangfeng/MyDocument/raw/master/docs/picture/jdkDownload5.png)


<h3>5.多个版本的jdk配置</h3>  
改变JAVA_HOME的路径
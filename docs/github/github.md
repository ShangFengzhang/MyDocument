## Github
### 1.官网
[https://github.com/](https://github.com/)
### 2.git的安装与使用

[https://git-scm.com/](https://git-scm.com/)  

#### 什么是git？  
Git是一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理

+ 在Linux上安装git  
通过系统自带的软件包管理工具安装  
使用如下命令：  
```
sudo apt install git-all
```

+ 在windows上安装git   
直接下载Git for Windows安装包  
![windos git安装 ](https://gitee.com/zhangshangfeng/MyDocument/raw/master/docs/picture/gitWindows.png)  
安装完成后  
桌面上鼠标右键会有Git Bash Here菜单，单击出现如下界面则安装成功  
![git命令行](https://gitee.com/zhangshangfeng/MyDocument/raw/master/docs/picture/GitBashHere.png)


在线演示学习工具  
[Learn Git Branching](https://oschina.gitee.io/learn-git-branching/) 



### 3.小问题

修改文件名无法提交：Git忽略大小写，所以修改文件名大小写是无法提交的。

* git config core.ignorecase 查看是否开启忽略大小写
* git config core.ignorecase
* 提交
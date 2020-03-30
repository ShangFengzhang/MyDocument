## docsify

本网站使用docsify框架搭建，Github Pages部署
#### 1 在哪里可以找到呢？
docsify:[https://docsify.js.org](https://docsify.js.org/#/)  
Github:[Github](github/github.md)
#### 2.准备工作
1. 准备一个github账号  
  [安装git环境](github/github.md)及注册github账号（很简单）

2. 安装node.js(运行在服务器端的JavaScript)环境    
下载地址：[https://nodejs.org/en/download/](https://nodejs.org/en/download/)   
历史版本：[https://nodejs.org/dist/](https://nodejs.org/dist/)    
![node下载](https://raw.githubusercontent.com/ShangFengzhang/MyDocument/master/docs/picture/NodeDownloads.png)  
安装过程：    
![node安装1](https://raw.githubusercontent.com/ShangFengzhang/MyDocument/master/docs/picture/node1.png)  
![node安装2](https://raw.githubusercontent.com/ShangFengzhang/MyDocument/master/docs/picture/node2.png)  
![node安装3](https://raw.githubusercontent.com/ShangFengzhang/MyDocument/master/docs/picture/node3.png)  
![node安装4](https://raw.githubusercontent.com/ShangFengzhang/MyDocument/master/docs/picture/node4.png)  
![node安装5](https://raw.githubusercontent.com/ShangFengzhang/MyDocument/master/docs/picture/node5.png)  
![node安装6](https://raw.githubusercontent.com/ShangFengzhang/MyDocument/master/docs/picture/node6.png)  
![node安装7](https://raw.githubusercontent.com/ShangFengzhang/MyDocument/master/docs/picture/node7.png)  
![node安装8](https://raw.githubusercontent.com/ShangFengzhang/MyDocument/master/docs/picture/node8.png)  	
配置环境变量：  
```
C:\Program Files\nodejs\;  
```    
查看版本：  
![node版本](https://raw.githubusercontent.com/ShangFengzhang/MyDocument/master/docs/picture/NodeVersion.png)

#### 3.本地工程创建  

1. 安装docsify -cli工具   
```
npm i docsify-cli -g
```
2. 初始化一个项目  
选择一个目录存放，在该目录下打开命令行工具，执行命令：  
```
docsify init ./docs
```  
![docsify初始化](https://raw.githubusercontent.com/ShangFengzhang/MyDocument/master/docs/picture/CreateDocsify.png)  
成功后会生成一个docs文件夹，其中会生成几个文件：  
 + index.html 网站入口
 + README.md 主页内容渲染
 + .nojekyll 用于阻止GitHub Pages会忽略掉下划线开头的文件  
 
3. 预览项目  
输入命令：  
```
docsify serve docs
```  
  ![docsify启动](https://raw.githubusercontent.com/ShangFengzhang/MyDocument/master/docs/picture/DocsifyStart.png)  

在浏览器输入： [http://localhost:3000](http://localhost:3000)  
4. 配置（参考docsify文档和Markdown语法）  
5. 部署到github  
创建一个仓库  
![创建仓库](https://raw.githubusercontent.com/ShangFengzhang/MyDocument/master/docs/picture/CreateRepository.png)   
进入docs的上一级目录，在该路径下打开Git Bash Here，将项目提交到GitHub上：  
```
git init //初始化一个仓库    
git add -A //添加所有文件到暂存区  
git commit -m "first commit" //提交到git仓库，引号里是注释  
git remote add origin https:github.com/... //github仓库中给出的路径
git push -u origin master //推送到仓库中  
```

6.建站  
在仓库下选择```Settings```选项，直到```Github Pages```标签，在Source下面选择```master branch/docs folder```选项，根据给出的地址访问网站即可。
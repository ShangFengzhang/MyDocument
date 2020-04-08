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
![node下载](http://188.131.181.191/images/NodeDownloads.png)  
安装过程：    
![node安装1](http://188.131.181.191/images/node1.png)  
![node安装2](http://188.131.181.191/images//node2.png)  
![node安装3](http://188.131.181.191/images/node3.png)  
![node安装4](http://188.131.181.191/images/node4.png)  
![node安装5](http://188.131.181.191/images/node5.png)  
![node安装6](http://188.131.181.191/images/node6.png)  
![node安装7](http://188.131.181.191/images/node7.png)  
![node安装8](http://188.131.181.191/images/node8.png)  	
配置环境变量：  
```
C:\Program Files\nodejs\;  
```    
查看版本：  
![node版本](http://188.131.181.191/images/NodeVersion.png)

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
![docsify初始化](http://188.131.181.191/images/CreateDocsify.png)  
成功后会生成一个docs文件夹，其中会生成几个文件：  
 + index.html 网站入口
 + README.md 主页内容渲染
 + .nojekyll 用于阻止GitHub Pages会忽略掉下划线开头的文件  
 
3. 预览项目  
输入命令：  
```
docsify serve docs
```  
  ![docsify启动](http://188.131.181.191/images/DocsifyStart.png)  

在浏览器输入： [http://localhost:3000](http://localhost:3000)  
4. 配置（参考docsify文档和Markdown语法）  
5. 部署到github  
创建一个仓库  
![创建仓库](http://188.131.181.191/images/CreateRepository.png)   
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
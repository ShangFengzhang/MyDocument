## Sublime Text 3

首先到官网下载sublime text 3 build 3211

软件安装前修改host文件  
位置一般在C:\Windows\System32\drivers\etc  
127.0.0.1    license.sublimehq.com 


安装时需要勾选**Add to explorer context menu**


将注册机放在安装目录下  
右键以管理员身份运行  
复制注册码


打开sublime  
help->Enter License  
粘贴注册码


1. 直接安装  
下载安装包解压缩到Packages目录（菜单->preferences->packages）  
2. 使用Package Control组件安装  
（1）首先安装Package Control：Package control是必装插件，所有其他的插件和主题都可以通过它来安装  
  
  　　1）按Ctrl+`调出console    
  　　2）粘贴以下代码到底部命令行并回车： 
 
``` 
import urllib.request,os; 
pf = 'Package Control.sublime-package'; 
ipp = sublime.installed_packages_path();
 urllib.request.install_opener( urllib.request.bui<br>ld_opener( urllib.request.ProxyHandler()) );
 open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ',<br>'%20')).read())
```  
  
  　　3）重启Sublime Text 3  
  　　4）如果在Perferences->package settings下看到package control项，则安装成功  
 （2）用Package Control安装插件  
  　 1）按下Ctrl+Shift+P调出命令面板  
  　  2）输入install 调出 Install Package 选项并回车  
   　 3）输入插件名，然后在列表中选中要安装的插件  


常用插件：

 1. ChineseLocalizations 汉化  
 2. AutoFileName  自动补全路径  
 3. SublimeCodeIntel 代码自动补全和提示插件  
 4. CSS Format css格式化  
 5. HTML5  HTML5标签拓展  
 6. ColorHighlighter  展示你所选择的颜色
 7. LESS LESS高亮插件  
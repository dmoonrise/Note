# 前置
vm虚拟机 ubuntu-server-20  
# 第一部
安装Nodejs  
参考网站[https://github.com/nodesource/distributions/blob/master/README.md](curl)  
```
# Using Ubuntu
curl -fsSL https://deb.nodesource.com/setup_17.x | sudo -E bash -
sudo apt-get install -y nodejs
```
安装cnpm  
下面是淘宝的cnmp安装命令  
```
npm install -g cnpm --registry=http://registry.npm.taobao.org
```
查看nodejs和cnmp的版本`node -v` `cnmp -v`  
安装hexo的框架
```
cnmp install -g hexo-cli
```
查看hexo框架的版本`hexo -v`
# 第二部分
创建个人blog的本地目录
```
#我的在/home目录下
cd /home
mkdir /blog
chmod 777 /blog
#blog可以是任意的英文名字，我的以blog为基础
```
在目录下搭建hexo博客  
```
cd /home/blog
sudo hexo init
```
测试一下本地博客`hexo s`  
进入本地网站，会有提示`http://localhost:4000`
至此，hexo的基本就搭建完成，下面进行hexo博客的美化  
# 第三部分
美化hexo，进入hexo官网找到主题[https://hexo.io/themes/](curl)  
找到喜欢的主题，进入会直接转到github，里面有教程  
这里我以lete的主题作为参考  
```
```
# 以下未编辑，因为我还没有部署好
```
hexo n 我的第一篇文章 #创建新的文章
#返回blog目录
hexo clean #清理
hexo g #生成
#Github创建一个新的仓库 YourGithubName.github.io
cnpm install --save hexo-deployer-git #在blog目录下安装git部署插件
----
#配置_config.yml
-----
	# Deployment
	## Docs: https://hexo.io/docs/deployment.html
	deploy:
  		type: git
 		repo: https://github.com/YourGithubName/YourGithubName.github.io.git
  		branch: master
-----
hexo d	#部署到Github仓库里
https://YourGithubName.github.io/  #访问这个地址可以查看博客
初始化博客
 hexo init 文件夹名称
新建文章
 hexo new 文章名称
(或者)
 hexo n 文章名称
在线预览
 hexo server
(或者)
 hexo s
一键部署
 hexo deploy
(或者)
 hexo d
```
```
git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia  #下载yilia主题到本地
会有报错，可以将https改成git就行了
```

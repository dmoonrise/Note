# linux-ubuntu-server-20 安装 h5ai
## 第一步
安装nginx
采用nginx官网的命令
参考链接[Nginx官网](http://nginx.org/en/linux_packages.html#Ubuntu)
导入官方的nginx签名密钥

```
curl https://nginx.org/keys/nginx_signing.key | gpg --dearmor \
    | sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null
```
为稳定的nginx软件包设置apt存储库
```
echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \
http://nginx.org/packages/debian `lsb_release -cs` nginx" \
    | sudo tee /etc/apt/sources.list.d/nginx.list
```
设置存储库固定，以优先选择我们的包而不是分发提供的包
```
echo -e "Package: *\nPin: origin nginx.org\nPin: release o=nginx\nPin-Priority: 900\n" \
    | sudo tee /etc/apt/preferences.d/99nginx
```
最后运行
```
sudo apt update
sudo apt install nginx
```


问题：运行 `sudo apt update` 出现以下错误

```
Hit:1 https://mirrors.tuna.tsinghua.edu.cn/ubuntu focal InRelease
Hit:2 https://mirrors.tuna.tsinghua.edu.cn/ubuntu focal-updates InRelease
Ign:3 http://nginx.org/packages/debian focal InRelease
Err:4 http://nginx.org/packages/debian focal Release
  404  Not Found [IP: 52.58.199.22 80]
Hit:5 https://mirrors.tuna.tsinghua.edu.cn/ubuntu focal-backports InRelease
Hit:6 https://mirrors.tuna.tsinghua.edu.cn/ubuntu focal-security InRelease
Reading package lists... Done
E: The repository 'http://nginx.org/packages/debian focal Release' does not have a Release file.
N: Updating from such a repository can't be done securely, and is therefore disabled by default.
N: See apt-secure(8) manpage for repository creation and user configuration details.
```

解决方案：

```
更改 为稳定的nginx软件包设置apt存储库
```

命令如下

如果你想使用主线nginx包，请改为运行以下命令：

```
echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \
http://nginx.org/packages/mainline/ubuntu `lsb_release -cs` nginx" \
    | sudo tee /etc/apt/sources.list.d/nginx.list
```



通过以下命令查看nginx的版本

```
nginx -v
```
给出的是
```
nginx version: nginx/1.20.2
```
安装完成nginx
以下是Nginx的服务器代码

```
要停止 Nginx 服务，请运行：
sudo systemctl stop nginx
要启动 Nginx 服务 ，键入：
sudo systemctl start nginx
要重启 Nginx 服务，键入：
sudo systemctl restart nginx
```
## 第二步
安装php
ps:ununtu20是php7.4 ubuntu18是php7.2
```
sudo apt install php7.4-fpm php7.4-json php7.4-gd
```
以下是php-fpm的服务器代码
```
php7.4和php7.2的代码不一样，把php7.4改成php7.2就行了
要停止 php-fpm 服务，请运行：
sudo systemctl stop php7.4-fpm.service
要启动 php-fpm 服务 ，键入：
sudo systemctl start php7.4-fpm.service
要重启 php-fpm 服务，键入：
sudo systemctl restart php7.4-fpm.service
```
## 第三步
### 3.1修改php-fpm配置
php-fpm配置参考位置`/etc/php/7.4/fpm/pool.d`
记得备份配置文件`sudo cp www.conf /etc/php/7.4/fpm/pool.d/www.conf.beifen`
找到`[www]`修改`listen`里面的内容,改成下面的形式

```
listen = 127.0.0.1:9000
listen.allowed_clients = 127.0.0.1
```
部分配置代码如下
```
; Start a new pool named 'www'.
; the variable $pool can be used in any directive and will be replaced by the
; pool name ('www' here)
[www]

; Per pool prefix
; It only applies on the following directives:
; - 'access.log'
; - 'slowlog'
; - 'listen' (unixsocket)
; - 'chroot'
; - 'chdir'
; - 'php_values'
; - 'php_admin_values'
; When not set, the global prefix (or /usr) applies instead.
; Note: This directive can also be relative to the global prefix.
; Default Value: none
;prefix = /path/to/pools/$pool

; Unix user/group of processes
; Note: The user is mandatory. If the group is not set, the default user's group
;       will be used.
user = www-data
group = www-data

; The address on which to accept FastCGI requests.
; Valid syntaxes are:
;   'ip.add.re.ss:port'    - to listen on a TCP socket to a specific IPv4 address on
;                            a specific port;
;   '[ip:6:addr:ess]:port' - to listen on a TCP socket to a specific IPv6 address on
;                            a specific port;
;   'port'                 - to listen on a TCP socket to all addresses
;                            (IPv6 and IPv4-mapped) on a specific port;
;   '/path/to/unix/socket' - to listen on a unix socket.
; Note: This value is mandatory.
listen = 127.0.0.1:9000
listen.allowed_clients = 127.0.0.1
```
最后面的就是修改的
### 3.2修改nginx的配置
nginx配置参考位置`/etc/nginx/conf.d`
记得备份配置文件`sudo cp default.conf /etc/nginx/conf.d/default.conf.bf`

```
    #location ~ \.php$ {
    #    root           你的网站位置和上面的root一样;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    #    include        fastcgi_params;
    #}
```
这段话的所有的#去掉
提示

```
#    fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
这段话会不一样，改成我这样就行了
```
整体的配置文件如下
```
server {
    listen       80;
    server_name  localhost;

    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /home/www;#这是我修改之后的默认网站位置,教程在下面
	index  index.html  index.php  /_h5ai/public/index.php;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    location ~ \.php$ {
    #    root           html;
	      root	/home/www;#和上面的root一样
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```
## 第四步
安装h5ai
[h5ai的官网](https://larsjung.de/h5ai/)
首先修改nginx的默认网站位置将 root 的后面更改为`/home/www;`
位置在`/etc/nginx/conf.d`
修改`default.conf`
进行下面的代码

```
cd /home
sudo mkdir www
chmod 777 www/
wget https://release.larsjung.de/h5ai/h5ai-0.30.0.zip
sudo atp install zip
unzip h5ai-0.30.0.zip
rm -r h5ai-0.30.0.zip
```
这里面的`_h5ai`文件夹就是网站整体
下面是h5ai的路径和分享文件夹的路径

```
home/
└── www
    ├── _h5ai
    └── 你分享的文件夹
```
访问
检查h5ai是否可访问，此页面显示了有关服务器功能的一些提示
http://你的ip地址/_h5ai/public/index.php

提示找不到文件记得给文件777权限

```
cd /home/www/_h5ai
chmod 777 private/
chmod 777 public/
```

就可以了

PS:h5ai的几个no

```
apt install ffmpeg
apt install imagemagick
apt install zip#这个前面安装过了
chmod 666 private/cache
chmod 666 public/cache
```
# 总结
每次修改配置之后都需要重启pfp-fpm和nginx服务，代码在上面有
以上教程全部都是手打，如需引用请注明来源

# H5ai网页配置参考网站
[https://zhuanlan.zhihu.com/p/103907645](https://zhuanlan.zhihu.com/p/103907645)

# 前置
服务器:ubuntu-server-18
# 参考资料
acme.sh官方文档[https://github.com/acmesh-official/acme.sh/wiki/说明](url)
b站安装acme.sh教程[https://www.bilibili.com/video/BV1SE411j7Jt](url)

## 过程
[http:www.kkphub.top](url)
下载 acme.sh

```
curl  https://get.acme.sh | sh -s email=my@example.com
```
`my@example.com`是用于注册Let's Encrypt帐户的电子邮件，您将在此处收到续订通知电子邮件。

以本系统为例

提示有错误建议重启服务器或者重新ssh连接
输入`acme.sh`测试有没有成功安装
生成证书

```
acme.sh --issue -d 网站的域名 -w  网站根目录位置
例如我的h5ai的网站
acme.sh --issue -d www.kkphub.top -w  /hmoe/www
```
安装证书 我的是nginx的web服务器
```
acme.sh --install-cert -d 网站的域名 \
--key-file       /网站根目录/网站域名.key  \
--fullchain-file /网站根目录/网站域名.pem \
--reloadcmd     "service nginx force-reload"
例如
acme.sh --install-cert -d www.kkphub.top  \
--key-file /home/www/www.kkphub.top.key \
--fullchain-file /home/www/www.kkphub.top.pem \
--reloadcmd     "service nginx force-reload"
```
修改nginx配置文件
修改如下
```
server {
#监听 443 ssl端口
    listen       443 ssl;
    server_name  www.kkphub.top;
#添加ssl证书位置
ssl_certificate /网站根目录/网站域名.pem;
ssl_certificate_key /网站根目录/网站域名.pem;
```
输入`systemctl restart nginx`重启nginx服务器
## 更新 acme.sh
升级 acme.sh 到最新版 :
```
acme.sh --upgrade
```
如果你不想手动升级, 可以开启自动升级:
```
acme.sh  --upgrade  --auto-upgrade
```
之后, acme.sh 就会自动保持更新了.
你也可以随时关闭自动更新:

```
acme.sh --upgrade  --auto-upgrade  0
```

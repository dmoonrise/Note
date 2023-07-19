# 零基础搭建博客并部署到GitHub

## 环境搭建

- 以下内容由mac下完成，且仅适用于mac

1. ```
   sudo -i  #开启root权限
   Password: #输入密码授权
   npm install hexo-cli -g 
   ```

   - 如果不这样运行会出现红色错误！（官网未见解释）
   - 运行完成后，**重新**打开终端

2. 重新打开终端运行代码

   ```
   hexo init blog
   cd blog
   npm install
   hexo server
   ```

   - hexo本地化环境搭建已经完成
   - 出现下图即正常![](https://cdn.jsdelivr.net/gh/Less-star/image-host/ime/20210428111621.png)
   - 这时候你可以访问http://localhost:4000本地预览

## 安装主题

当默认安装后，我们的界面一般是这样的![image-20210428112705604](/Users/lijinxing/Library/Application Support/typora-user-images/image-20210428112705604.png)

如果不喜欢怎么办呢？那么我们就要**修改/更换**主题

- 修改（自我研究，看你心情）
- 更换主题看下面！

1. 访问：[Themes | Hexo](https://hexo.io/themes/)查找喜欢的主题源文件
   - 接下来的内容以“ayer”为例

2. 通过作者文档查询安装命令

   ```
   npm i hexo-theme-ayer -S
   ```

   ![image-20210428113406021](/Users/lijinxing/Library/Application Support/typora-user-images/image-20210428113406021.png)

3. 运行安装

   - 手动进入根目录/blog文件夹运行终端输入

     ```
     npm i hexo-theme-ayer -S
     ```

   - 或者使用终端进入文件夹操作

     ```
     cd blog
     npm i hexo-theme-ayer -S
     ```

4. 参考作者推荐设置
   - 将博客根目录下的 `_config.yml` 里的 `theme` 值修改成 `ayer`
     - 这里包含所有博客相关信息
   - **自定义**博客根目录生成的`_config.ayer.yml`主题配置文件
     - 这里面包含了所有主题自定义相关信息

5. 设置完成后**博客根目录**运行以下命令预览成果

   ```
    hexo server
   ```

6. 安装已经完成，请自行调整博客与主题内容
   - 这里的信息最有效的参考，
     - 主题作者的建议
     - 你的聪明才智

## 安装插件

### [hexo-generator-searchdb](https://github.com/theme-next/hexo-generator-searchdb) 用于搜索

```
COPYnpm install hexo-generator-searchdb --save
```

然后将以下配置复制到你博客根目录下的 `_config.yml` 里（注意不是 主题包 目录下的）:

```
COPY# hexo-generator-searchdb
search:
  path: search.xml
  field: post
  format: html
```

### [hexo-generator-feed](https://github.com/hexojs/hexo-generator-feed) 用于生成 RSS 订阅

```
COPYnpm install hexo-generator-feed --save
```

然后将以下配置复制到你博客根目录下的 `_config.yml` 里（注意不是 主题 目录下的）:

```
COPYfeed:
  type: atom
  path: atom.xml
  limit: 20
  hub:
  content:
  content_limit: 140
  content_limit_delim: " "
  order_by: -date
```

一件部署npm install `--`save hexo-deployer-git
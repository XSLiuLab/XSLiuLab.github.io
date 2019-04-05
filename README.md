# Homepage of Xue-Song Liu lab

网站由Hugo驱动，更新此仓库时Netlify会自动构建新版本并发布于<https://liuxslab.netlify.com/>。
下面的内容主要说明在学校CentOS虚拟主机上的配置和维护。

## 配置

如果是第一次/重新部署到CentOS上，需要进行配置，否则只需要进行维护。

### 安装Hugo

```
wget -c https://github.com/gohugoio/hugo/releases/download/v0.54.0/hugo_0.54.0_Linux-64bit.tar.gz # 可以下载其他版本
tar -zxvf *tar.gz
#cp ./hugo /usr/local/bin/ # 可以放到专门的程序目录中，也可以后面直接通过路径运行
```

### 拉取apache容器

安装docker-ce，然后`sudo docker pull httpd`。

接着将配置文件从容器中拷贝出来

专门创建一个`apache`目录放置建站需要修改或提供的文件。

在里面新建2个文件夹：`conf`和`logs`。

以交互模式运行一个`httpd`容器，然后从外面拷贝配置文件到`conf`目录下。

```
sudo docker cp <容器名>:/usr/local/apache2/conf/httpd.conf /public/data/apache/conf/
```

克隆本仓库，在仓库根目录下运行`hugo`。

将`public`目录链接为`apache`下的`www`目录。

```
 ln -s XSLiuLab.github.io/public/ www
```


然后使用`sudo`运行下面的代码。

```
 sudo ./run_website.sh liulab_website
```

文件名：run_website.sh

```
docker run --name $1 -p 80:80 \
  -v /public/data/apache/www/:/usr/local/apache2/htdocs/  \
  --mount type=bind,source=/public/data/apache/conf/httpd.conf,target=/usr/local/apache2/conf/httpd.conf  \
  -v /public/data/apache/logs/:/usr/local/apache2/logs/  \
  -d httpd

```

此时就可以访问了。

下面展示下`apache`目录结构：

```
.
├── conf
│   └── httpd.conf
├── logs
│   └── httpd.pid
├── www -> XSLiuLab.github.io/public/
└── XSLiuLab.github.io
    ├── archetypes
    ├── config.toml
    ├── content
    ├── public
    ├── README.md
    ├── _redirects
    ├── resources
    ├── static
    └── themes
```


## 维护


- 停止`liulab_website`容器。
- 拉取和更新仓库。
- 使用`hugo`重新生成页面。
- 重新运行`liulab_website`容器

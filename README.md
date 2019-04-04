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

安装docker-ce，然后`sudo docker pull htppd`。

接着将配置文件从容器中拷贝出来

专门创建一个`apache`目录放置建站需要修改或提供的文件。

```
sudo docker cp <容器名>:/usr/local/apache2/conf/httpd.conf /public/data/apache/conf/
```


## 维护

将本仓库克隆，然后使用hugo构建，将public目录下文件全部链接到www目录，然后修改一些参数？

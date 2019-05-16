### nignx简介与安装

##### 1.1nignx简介

*Nginx* (engine x) 是一个高性能的[HTTP](https://baike.baidu.com/item/HTTP)和[反向代理](https://baike.baidu.com/item/%E5%8F%8D%E5%90%91%E4%BB%A3%E7%90%86/7793488)web服务器，同时也提供了IMAP/POP3/SMTP服务。Nginx是由伊戈尔·赛索耶夫为[俄罗斯](https://baike.baidu.com/item/%E4%BF%84%E7%BD%97%E6%96%AF/125568)访问量第二的Rambler.ru站点（俄文：Рамблер）开发的，第一个公开版本0.1.0发布于2004年10月4日。

其将[源代码](https://baike.baidu.com/item/%E6%BA%90%E4%BB%A3%E7%A0%81)以类BSD许可证的形式发布，因它的稳定性、丰富的功能集、示例配置文件和低系统资源的消耗而[闻名](https://baike.baidu.com/item/%E9%97%BB%E5%90%8D/2303308)。2011年6月1日，nginx 1.0.4发布。

Nginx是一款[轻量级](https://baike.baidu.com/item/%E8%BD%BB%E9%87%8F%E7%BA%A7/10002835)的[Web](https://baike.baidu.com/item/Web/150564) 服务器/[反向代理](https://baike.baidu.com/item/%E5%8F%8D%E5%90%91%E4%BB%A3%E7%90%86/7793488)服务器及[电子邮件](https://baike.baidu.com/item/%E7%94%B5%E5%AD%90%E9%82%AE%E4%BB%B6/111106)（IMAP/POP3）代理服务器，在BSD-like 协议下发行。其特点是占有内存少，[并发](https://baike.baidu.com/item/%E5%B9%B6%E5%8F%91/11024806)能力强，事实上nginx的并发能力确实在同类型的网页服务器中表现较好，中国大陆使用nginx网站用户有：百度、[京东](https://baike.baidu.com/item/%E4%BA%AC%E4%B8%9C/210931)、[新浪](https://baike.baidu.com/item/%E6%96%B0%E6%B5%AA/125692)、[网易](https://baike.baidu.com/item/%E7%BD%91%E6%98%93/185754)、[腾讯](https://baike.baidu.com/item/%E8%85%BE%E8%AE%AF/112204)、[淘宝](https://baike.baidu.com/item/%E6%B7%98%E5%AE%9D/145661)等。

##### 1.2安装fastdfs-nginx-module 

1. 解压缩 nginx-1.8.1.tar.gz

2. 解压缩 fastdfs-nginx-module-master.zip

3. 进入nginx-1.8.1目录中

4. 执行

   ```shell
   sudo ./configure  --prefix=/usr/local/nginx/ --add-module=fastdfs-nginx-module-master的解压后的目录的绝对路径/src
   ```

   注意：**这时候会报一个错，说没有PCRE库**

下载缺少的库

```shell
sudo apt-get install libpcre3 libpcre3-dev 
```

- 首先你需要去更换源，因为ubuntu自带的源没有这个库

- 更换下载源为阿里的源

- 先把原来的源文件备份

  ```shell
  sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
  ```

- 编辑源文件

  ```shell
  sudo vim /etc/apt/sources.list
  ```

  把原来的内容全部删掉，粘贴一下内容：

  ```shell
  deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
  deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
   
  deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
  deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
   
  deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
  deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
   
  deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
  deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
   
  deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
  deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
  ```

  更换完源之后执行  

  ```shell
  sudo apt-get  update
  sudo apt-get install libpcre3 libpcre3-dev
  ```

  然后进入nginx-1.8.1目录中，再次执行：

  ```shell
  sudo ./configure  --prefix=/usr/local/nginx/ --add-module=fastdfs-nginx-module-master解压后的目录的绝对路径/src
  ```

  然后编译：

  ```shell
  sudo make
  ```

  这时候还会报一个错（错误还真多），错误原因是因为nginx编译的时候把警告当错误处理，事实上这个警告并不影响（程序员忽略警告）

  解决方法：

  找到objs目录下的Makefile

  vim Makefile

  删掉里面的-Werror(**如果没有修改权限，修改一下这个文件的权限,`chmod 777 Makefile`**)

  然后回到nginx-1.8.1目录中

  执行完成后继续执行**sudo make**

  执行**sudo make install** 

  5.sudo cp fastdfs-nginx-module-master解压后的目录中src下mod_fastdfs.conf   /etc/fdfs/mod_fastdfs.conf

  6.sudo vim /etc/fdfs/mod_fastdfs.conf

  修改内容：

  connect_timeout=10
  tracker_server=自己ubuntu虚拟机的ip地址:22122
  url_have_group_name=true
  store_path0=/home/itcast/fastdfs/storage

  7.sudo cp 解压缩的fastdfs-master目录中的conf中的http.conf  /etc/fdfs/http.conf

  8.sudo cp 解压缩的fastdfs-master目录中conf的mime.types /etc/fdfs/mime.types

  9.sudo vim /usr/local/nginx/conf/nginx.conf

  在http部分中添加配置信息如下：!!!替换原来的server{。。。}

  server {
              listen       8888;
              server_name  localhost;
              location ~/group[0-9]/ {
                  ngx_fastdfs_module;
              }
              error_page   500 502 503 504  /50x.html;
              location = /50x.html {
              root   html;
              }
          }

  10. 启动nginx

    	sudo  /usr/local/nginx/sbin/nginx

  
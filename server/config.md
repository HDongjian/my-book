
# 关于apt

## 介绍

高级包装工具（英语：Advanced Packaging Tools,简称：APT）是Debian及其衍生发行版（如：ubuntu）的软件包管理器。APT可以自动下载，配置，安装二进制或者源代码格式的软 件包，因此简化了 Unix系统上管理软件的过程,apt-get命令一般需要root权限执行，所以一般跟着sudo命令。

## 常用命令

*   `apt-get update` 更新软件列表信息（注意只是更新列表，并未更新程序，后接apt-get upgrade）

*   `apt-get upgrade` 更新程序

*   `apt-get dist`-upgrade 版本升级

*   `apt-get install` packagename（安装程序包）

*   `apt-get remove` packagename (卸载程序）

*   `apt-cache search` packagename（搜索程序包）

*   `apt-get clean` 删除所有已下载的包文件

*   `apt-get autoclean` 删除已下载的旧包文件

*   `apt-get autoremove` 卸载所有自动安装且不再使用的软件包

## 常见问题

### `0% [Waiting for headers]`

`apt-get clean`
`apt-get update`

### apt 命令不可用

`apt-get update`

# 安装mysql

参考链接：[https://www.cnblogs.com/duaimili/p/9955608.html](https://www.cnblogs.com/duaimili/p/9955608.html)


## 安装步骤分为以下

依次输入这三条命令;
```shell
  sudo apt-get install mysql-server
  sudo apt install mysql-client
  sudo apt install libmysqlclient-dev
```

## 检验是否安装mysql成功
```shll
sudo netstat -tap | grep mysql
```
出现下图表示安装成功
![](index_files/12b9de8b-6b8c-4b92-af73-cc6ccbeb7580.jpg)

## 打开一个文件

```shell
sudo vim /etc/mysql/debian.cnf
```
在这个文件里面有着MySQL默认的用户名和用户密码， 
用户名默认是debian-sys-maint，如下所示

![](index_files/4377c227-6153-4b42-a878-676812618263.jpg)

需要先使用这个用户名和密码登录进去

然后终端会提示你输入密码

这是输入文件中的密码即可成功登陆。

![](index_files/ab4c230f-edfa-4f90-b2e2-0dcf0a1c2e4b.jpg)

根据给定的账号密码进行修改密码，将密码设置为root，用户名还是 debian-sys-maint

版本是5.7，所以password字段已经被删除，取而代之的是authentication_string字段，所以要更改密码：

```shell
update mysql.user set authentication_string=password('root') where user='debian-sys-maint'and Host = 'localhost';
```

重新登录检测成功（只是知道了用户名 和修改为自己的密码了）

在mysql环境下执行授权命令(授权给远程任何电脑登录数据库)：

```shell
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'root' WITH GRANT OPTION;
```

重启/打开/关闭MySQL的方法是

```shell
sudo service mysql restart/start/stop
```
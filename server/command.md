# linux

*   `cd /` 是跳转到根目录
    *   根目录是所有用户共享的目录
*   `cd ~` 是跳转到当前用户的家目录
    *   如果是root用户，cd ~ 相当于 cd /root
    *   如果是普通用户，cd ~ 相当于cd /home/当前用户名
*   `cd /home` 相当于查看有多少普通用户的家目录
    *   因为所有的普通用户的父目录都是home目录
*   `pwd` 查看路径
*   `ll` 查看所有文件
*   `wget` 下载
*   `sudo passwd root` 重置密码
*   `su` 管理员
*   `tar -zxvf apache-tomcat-9.0.22.tar.gz` 解压tar文件
*   `mkdir` 创建文件夹
*   `scp wenjianming root@192.168.0.11:/home`文件传输
*   `sudo service ssh restart` 重启ssh服务
*   `sudo bash` 切换管理员权限

# vim

*   `vim server.xml` 打开文件
*   按o进行编辑
*   ESC键跳到命令模式
*   :w保存文件但不退出vi 编辑
*   :w! 强制保存，不退出vi 编辑
*   :w file将修改另存到file中，不退出vi 编辑
*   :wq保存文件并退出vi 编辑
*   :wq!强制保存文件并退出vi 编辑
*   q:不保存文件并退出vi 编辑
*   :q!不保存文件并强制退出vi 编辑
*   :e!放弃所有修改，从上次保存文件开始在编辑

# nginx

*   `./nginx -c /usr/local/nginx/conf/nginx.conf` 启动nginx
*   `ps -ef|grep nginx` 查看启动状态
*   `nginx -s reload|reopen|stop|quit` 重新加载配置|重启|停止|退出 nginx

# node

*   `node server.js` 查看服务

# pm2

*   `pm2 start app.js` # 启动app.js应用程序
*   `pm2 start app.js -i 4` # cluster mode 模式启动4个app.js的应用实例 # 4个应用程序会自动进行负载均衡
*   `pm2 start app.js --name="api"` # 启动应用程序并命名为 “api”
*   `pm2 start app.js --watch` # 当文件变化时自动重启应用
*   `pm2 start script.sh` # 启动 bash 脚本
*   `pm2 list` # 列表 PM2 启动的所有的应用程序
*   `pm2 monit` # 显示每个应用程序的CPU和内存占用情况
*   `pm2 show [app-name]` # 显示应用程序的所有信息
*   `pm2 logs` # 显示所有应用程序的日志
*   `pm2 logs [app-name]` # 显示指定应用程序的日志
*   `pm2 flush`
*   `pm2 stop all` # 停止所有的应用程序
*   `pm2 stop 0` # 停止 id为 0的指定应用程序
*   `pm2 restart all` # 重启所有应用
*   `pm2 reload all` # 重启 cluster mode下的所有应用
*   `pm2 gracefulReload all` # Graceful reload all apps in cluster mode
*   `pm2 delete all` # 关闭并删除所有应用
*   `pm2 delete 0` # 删除指定应用 id 0
*   `pm2 scale api 10` # 把名字叫api的应用扩展到10个实例
*   `pm2 reset [app-name]` # 重置重启数量
*   `pm2 startup` # 创建开机自启动命令
*   `pm2 save` # 保存当前应用列表
*   `pm2 resurrect` # 重新加载保存的应用列表

# mysql

## mysql命令模式

*   `mysql -u root -p` 进入mysql编辑模式

*   `desc table_name;` 查表的字段信息（不包含字段内容）

*   `show columns from table_name;` 同上

*   `show create table table_name;` 查表字段信息和字符集信息

*   `select * from table_name;` 查表所有内容

*   `select * from table_name where id=？;` 查指定行

*   `select field_name from table_name;` 查指定列，field意为字段

*   `select * from table_name where field_name like "%???%";` 根据字段内容的近似值查找指定行
*   `select field_name1,field_name2 from table_name;` 查指定字段的多个列

*   `update table_name set field_name="abc" where id=?;` 修改指定字段的内容

*   `show databases;` 查看有哪些数据库

*   `use database_name;` 进入数据库

*   `create database_name;` 创建数据库

*   `show tables;` 查看数据库内有哪些表

*   `flush privileges;` 刷新

*   `exit` 退出

## linux下查看mysql服务的两种方式：

`ps -ef|grep mysql`

`netstat -nlp`

## linux下启动mysql服务的两种方式：

`cd /usr/bin`
`./mysqld_safe &`
`service mysql start`
`service mysql restart`

## linux下关闭mysql服务的两种方式：

命令行方式：
`mysqladmin -u root shutdown`

服务方式：
`service mysql stop`

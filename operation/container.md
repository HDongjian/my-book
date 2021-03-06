# 容器化运维操作

日趋复杂的运维开发环境，对虚拟服务器及应用服务的要求更加的多元化。我们需要更加容易扩展、性能优越、方便监控的管理服务，容器化应用、容器化运维应运而生。



**【知己知彼】你将在学习本课程中学习到：**

1. 理解/安装docker容器技术
2. 秒级快速部署mysql、nginx、tomcat等服务
3. 使用容器技术发布nodejs应用
4. …

**【工欲善其事，必先利其器】你需要准备的环境（三选一）：**

- Linux环境(Centos 7以上/Debian 8以上/Ubuntu 16.04LTS以上版本)
- Windows 64位专业版/企业版/教育版(Build 15063以上)
- macOS Sierra 10.12以上的版本

> 如果，你有了上面的环境，就可以很快速的跟着我们的内容快速学习。



## 容器和Docker

### 什么是容器化？

容器化是将应用程序或服务、其依赖项及其配置（抽象化为部署清单文件）一起打包为容器映像的一种软件开发方法。

![docker-containerized-appliction-blue-border_2](./container/docker-containerized-appliction-blue-border_2.png)

软件容器充当软件部署的标准单元，其中可以包含不同的代码和依赖项。 按照这种方式容器化软件，开发人员和 IT 专业人员只需进行极少修改或不修改，即可将其部署到不同的环境。

容器化应用程序在容器主机上运行，而容器主机在 OS（Linux 或 Windows）上运行。因此，容器的占用比虚拟机 (VM) 映像小得多。

容器化的特点：

- 一致的运行环境
- 可伸缩性
- 更方便的移植
- 隔离性

### 什么是Docker?

[Docker](https://github.com/moby/moby)是用GO语言开发的应用容器引擎，基于容器化，沙箱机制的应用部署技术。可适用于自动化测试、打包，持续集成和发布应用程序等场景，包括阿里云，亚马逊在内的云计算服务商都采用了docker来打造serverless服务平台。它不仅仅可以部署项目，还可以用于数据库搭建，nginx服务搭建，nodejs、php等编程语言环境搭建。

> PS: docker现已改名为moby



Docker中的三个**重要概念**：

- 镜像(image)：分片的（只读）文件系统，由Dockerfile创建

  独立、易扩展、更效率

- 容器(container)：由Docker进程创建和管理的：文件系统 +  系统资源 + 网络配置 + 日志管理

  docker是docker镜像的运行环境，所以容器的概念就比较好理解了

- 仓库(registry)：用来远端存储docker镜像

  版本控制、变更管理、为持续集成与快速部署提供便利

### Docker vs 虚拟机

![docker-containerized-and-vm-transparent-bg](./container/docker-containerized-and-vm-transparent-bg.png)

1. 容器是应用层的抽象，它将代码和依赖关系打包在一起。 多个容器可以在同一台机器上运行，并与其他容器共享操作系统内核，每个容器在用户空间中作为独立进程运行。 容器占用的空间比VM少（容器映像的大小通常为几十MB），可以处理更多的应用程序，并且需要更少的VM和操作系统。
2. 虚拟机（VM）是物理硬件的抽象，将一台服务器转变为多台服务器。 管理程序允许多台VM在单台机器上运行。 每个VM都包含操作系统的完整副本，应用程序，必要的二进制文件和库 - 占用数十GB。 虚拟机也可能很慢启动。

**总结一下：**

| 特性          | 容器                     | 虚拟机      |
| :------------ | :----------------------- | :---------- |
| 启动          | 秒级                     | 分钟级      |
| 硬盘使用      | 一般为 `MB`              | 一般为 `GB` |
| 性能          | 接近原生                 | 弱于        |
| 系统支持量    | 单机支持上千个容器       | 一般几十个  |
| 开发/环境定制 | 方便(命令行、面向对象式) | 进入虚拟机  |

**相同点：**

1. 文件隔离/文件共享（沙箱）
2. 资源隔离
3. 网络隔离
4. 支持多种宿主环境（扩展）
5. 快照/镜像（版本控制/变更管理）

**不同点：**

1. 不同的资源管理/依赖//释放（虚拟机占用更多的系统资源）
2. 不同的应用运行环境
3. Docker是写时复制
4. 不同的日志方式(Docker收集日志，而虚拟机需要在虚拟系统里面看日志)
5. 不同的交互方式(Docker偏shell，虚拟机偏GUI)

### Docker的工作原理(重点)

![work](./container/work.png)

Docker是容器化部署技术，它主要作用在于通过运行容器来实现应用部署，而容器基于镜像运行。

简单地说，就是将你的项目和依赖包(基础镜像)打成一个带有启动指令的项目镜像，然后在服务器创建一个容器，让镜像在容器内运行，从而实现项目的部署。

服务器就是容器的宿主机，docker容器与宿主机之间是相互隔离的。

Docker 的基础是Linux容器（LXC：Linux Containers）等技术。

**一般情况下：**

Linux(服务器) -> tomcat安装 -> Java依赖 -> maven依赖 ->放置在/usr/lib目录 -> 配置tomcat端口/目录 -> 运行tomcat中的startup.sh脚本 -> 配置网络、防火墙等(后续) -> 服务启动

**Docker：**

使用基于java的tomcat镜像 -> docker run -> 指定端口/挂载webapp目录 -> 服务启动

这其中，发生了什么？

1. Docker会自己拉取镜像，若本地已经存在该镜像，则不用到网上去拉取

2. 创建新的容器

3. 分配文件系统并且挂着一个可读写的层，任何修改容器的操作都会被记录在这个读写层上，你可以保存这些修改成新的镜像，也可以选择不保存，那么下次运行改镜像的时候所有修改操作都会被消除

4. 分配网络\桥接接口，创建一个允许容器与本地主机通信的网络接口

5. 设置ip地址，从池中寻找一个可用的ip地址附加到容器上，换句话说，localhost并不能访问到容器

6. 运行你指定的程序

7. 捕获并且提供应用输出，包括输入、输出、报错信息



Docker的价值：

从应用架构角度：统一复杂的构建环境；

从应用部署角度：解决依赖不同、构建麻烦的问题，结合自动化工具（如jenkins）提高效率。

从集群管理角度：规范的服务**调度**，**服务发现**，**负载均衡**



## 常见的应用场景介绍

Docker提供了轻量级的虚拟化，相比于虚拟机，可以在同一台机器上创建更多数量的容器。它常见的应用场景：1. 快速部署；2. 隔离应用； 3. 提高开发效率； 4. 版本控制； 5. 简化配置，整合资源； 

### 快速部署

我们尝试着来部署一个mysql：

```bash
docker run -d --name mysql-test -e MYSQL_ROOT_PASSWORD=123456 mysql
```

![3](./container/3.gif)

同样的道理，我们来部署一个nginx:

![4](./container/4.gif)

注意：这里没有映射服务端口，所以在`80`端口是看不到`index.html`中的内容的。需要加入`-p`参数来映射端口！！

```bash
docker run -d --name web -p 8000:80 -v ${your_dir}:/usr/share/nginx/html nginx
```



### 隔离应用

我们可以同时跑两个mysql，两个nginx，指定不同的端口进行映射：

把`mysql-test1`映射到`8001`端口，把`mysql-test2`映射到`8002`端口。

```bash
docker run -d --name mysql-test1 -p 8001:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
docker run -d --name mysql-test2 -p 8002:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
```

把web1映射到8100端口，把web2映射到8200端口。

```bash
docker run -d --name web1 -p 8100:80 -v ${your_dir}:/usr/share/nginx/html nginx
docker run -d --name web2 -p 8200:80 -v ${your_dir}:/usr/share/nginx/html nginx
```



### 提高开发效率

1. 一致的运行环境

   由于 Docker 确保了执行环境的一致性，使得应用的迁移更加容易。Docker 可以在很多平台上运行，无论是物理机、虚拟机、公有云、私有云，甚至是笔记本，其运行结果是一致的。因此用户可以很轻易的将在一个平台上运行的应用，迁移到另一个平台上，而不用担心运行环境的变化导致应用无法正常运行的情况。

2.  更快速的启动时间

   传统的虚拟机技术启动应用服务往往需要数分钟，而 Docker 容器应用，由于直接运行于宿主内核，无需启动完整的操作系统，因此可以做到秒级、甚至毫秒级的启动时间。大大的节约了开发、测试、部署的时间。

3. 更高效的复用系统资源

   由于容器不需要进行硬件虚拟以及运行完整操作系统等额外开销，Docker 对系统资源的利用率更高。无论是应用执行速度、内存损耗或者文件存储速度，都要比传统虚拟机技术更高效。因此，相比虚拟机技术，一个相同配置的主机，往往可以运行更多数量的应用。

4. 仓库/镜像机制

   使用仓库可以方便的在任何有docker进程的虚拟机/服务器/主机上运行docker应用，环境的统一，让它们的部署变的非常的简单。

### 版本控制

Docker容器还可以像git仓库一样，可以让你提交变更到Docker镜像中并通过不同的版本来管理它们，来看看下面的例子：

我们之前创建了一个`mysql`，现在，我们使用`commit`命令就可以给它做一个快照，打上一个`tag`。

![5](./container/5.gif)

在后面的课程中，我们会详细的介绍[Docker的常见命令](#Docker常见命令(重点))

### DevOps流程

对开发和运维（[DevOps](https://zh.wikipedia.org/wiki/DevOps)）人员来说，最希望的就是一次创建或配置，可以在任意地方正常运行。

使用 Docker 可以通过定制应用镜像来实现持续集成、持续交付、部署。开发人员可以通过 [Dockerfile](https://yeasy.gitbooks.io/docker_practice/image/dockerfile) 来进行镜像构建，并结合 [持续集成(Continuous Integration)](https://en.wikipedia.org/wiki/Continuous_integration) 系统进行集成测试，而运维人员则可以直接在生产环境中快速部署该镜像，甚至结合 [持续部署(Continuous Delivery/Deployment)](https://en.wikipedia.org/wiki/Continuous_delivery) 系统进行自动部署。

而且使用 `Dockerfile` 使镜像构建透明化，不仅仅开发团队可以理解应用运行环境，也方便运维团队理解应用运行所需条件，帮助更好的生产环境中部署该镜像。

## 使用Docker

### Docker如何进行安装

#### 关于系统需求(重点)

1. 平台类

| 操作系统                                                     |                            x86_64                            |
| :----------------------------------------------------------- | :----------------------------------------------------------: |
| [Docker Desktop for Mac (macOS)](https://docs.docker.com/docker-for-mac/install/) | ![yes](https://docs.docker.com/install/images/green-check.svg) |
| [Docker Desktop for Windows (Microsoft Windows 10)](https://docs.docker.com/docker-for-windows/install/) | ![yes](https://docs.docker.com/install/images/green-check.svg) |

2. 服务器

| 平台                                                         | x86_64 / amd64                                               | ARM                                                          | ARM64 / AARCH64                                              | IBM Power (ppc64le)                                          | IBM Z (s390x)                                                |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| [CentOS](https://docs.docker.com/install/linux/docker-ce/centos/) | [![yes](https://docs.docker.com/install/images/green-check.svg)](https://docs.docker.com/install/linux/docker-ce/centos/) |                                                              | [![yes](https://docs.docker.com/install/images/green-check.svg)](https://docs.docker.com/install/linux/docker-ce/centos/) |                                                              |                                                              |
| [Debian](https://docs.docker.com/install/linux/docker-ce/debian/) | [![yes](https://docs.docker.com/install/images/green-check.svg)](https://docs.docker.com/install/linux/docker-ce/debian/) | [![yes](https://docs.docker.com/install/images/green-check.svg)](https://docs.docker.com/install/linux/docker-ce/debian/) | [![yes](https://docs.docker.com/install/images/green-check.svg)](https://docs.docker.com/install/linux/docker-ce/debian/) |                                                              |                                                              |
| [Fedora](https://docs.docker.com/install/linux/docker-ce/fedora/) | [![yes](https://docs.docker.com/install/images/green-check.svg)](https://docs.docker.com/install/linux/docker-ce/fedora/) |                                                              | [![yes](https://docs.docker.com/install/images/green-check.svg)](https://docs.docker.com/install/linux/docker-ce/fedora/) |                                                              |                                                              |
| [Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/) | [![yes](https://docs.docker.com/install/images/green-check.svg)](https://docs.docker.com/install/linux/docker-ce/ubuntu/) | [![yes](https://docs.docker.com/install/images/green-check.svg)](https://docs.docker.com/install/linux/docker-ce/ubuntu/) | [![yes](https://docs.docker.com/install/images/green-check.svg)](https://docs.docker.com/install/linux/docker-ce/ubuntu/) | [![yes](https://docs.docker.com/install/images/green-check.svg)](https://docs.docker.com/install/linux/docker-ce/ubuntu/) | [![yes](https://docs.docker.com/install/images/green-check.svg)](https://docs.docker.com/install/linux/docker-ce/ubuntu/) |

#### Linux(重点)

对于中国用户：

```shell
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

对于国外vps：

```
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```

> 脚本介绍：https://github.com/docker/docker-install



#### Windows

安装Docker的系统需求：

- 系统版本：Windows 10 64bit: Pro, Enterprise or Education (Build 15063 or later).
- 在BIOS中开启了虚拟化（virtualization），正常情况下会默认开启，去系统管理中看看有没有Hyper-v被安装了。
- CPU特性：SLAT-capable.
- 4GB以上的内存

> 如果不满足上面的需求，则需要使用[Docker Toolbox](https://docs.docker.com/toolbox/overview/)，相当于是运行了一个virtualbox的虚拟镜像。
>



#### MacOS

**方法一官方`dmg`：**

官方下载地址：

[Docker Descktop for Mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac)

对系统的要求：

> Requires Apple Mac OS Sierra 10.12 or above. Download [Docker Toolbox](https://docs.docker.com/toolbox/overview/) for previous OS versions.

只要系统是 Mac OS Sierra 10.12以上即可。

下载完`Docker.dmg`安装包之后，双击即可以安装，可能需要系统管理员权限，输入密码即可。

运行过后，小图标：

![2](./container/2.gif)

安装完之后，在终端工具中，使用`docker version`来查看Docker版本。

![docker-version](./container/docker-version.jpg)



**方法二brew cask：**

```shell
brew update 

brew cask install docker

# 删除的方法, 还需要手动删除Docker.app
brew cask uninstall docker
```

上面的命令将会把Docker安装在`Applications`目录下。



### Docker国内加速

1. Docker的Toolbox镜像站(推荐)，主要是针对低版本的windows与mac用户：

[http://mirrors.aliyun.com/docker-toolbox/](http://mirrors.aliyun.com/docker-toolbox/)

2. Docker镜像加速，主要是对`docker pull`拉取镜像操作进行网络加速优化：

需要注册阿里云的账号，登录[容器Hub服务](https://cr.console.aliyun.com/)，在左侧的加速器帮助页面就会显示为你独立分配的加速地址。

![image-20190524172015448](./container/image-20190524172015448.png)

### 第一个Docker应用hello world

1. 查看docker版本

2. 运行第一个Docker应用

   使用`docker run`命令

3. 查看容器运行状态

   使用`docker ps`命令来查看正在运行的容器的状态，`-a`参数来查看所有的已经运行的容器(无论是否停止)

![1](./container/1.gif)

### Docker常见命令(重点)

- **创建`run`**，`-p` 映射宿主机的端口给镜像服务使用，`-v` 挂载宿主机的文件目录到镜像里面去，-it是提供交互式的终端工具，`-d` 是让镜像容器在后台去持续运行,`--name` 指定容器的名称，exec可以进入到容器里面去：

  ```
  docker exec -it <container_name> /bin/bash
  
  docker exec -it <container_name> /bin/sh
  ```

- **启动`start`/停止`stop`/重启`restart`/删除已停止容器`rm`**

- **登录仓库`login`/拉取镜像`pull`/推送`push`/提交镜像`commit`/给指定容器标签`tag`**

  docker hub上去注册我们的账号，才能使用login

  使用docker commit来给运行中的容器打tag:

  ```
  docker commit <container_id> <namespace>/<image_name>:tag
  ```

  ```
  docker commit e12b80defe8c itcast/mysql
  ```

  使用`pull`/`push`命令来推送本地的镜像到远程仓库

  ```
  docker pull/push <namespace>/<image_name>:tag
  ```

- 查看所有本地镜像`images`/删除本地镜像`rmi`

  删除镜像之前，要停止`stop`并且删除`rm`运行中的容器。

- 查看容器服务打印的日志`logs`/检阅容器`inspect`(更详细的容器信息，硬件/网络/版本等)/进入容器`exec`

- 查看版本`version`/docker进程信息`info`

-  查看命令帮助：

  ```
  docker <run/pull> --help
  ```

  

## 扩展知识

### 制作Docker镜像(重点)

Dockerfile 是一个由一堆命令+参数构成的脚本，使用 `docker build` 即可执行脚本构建镜像，自动的去做一些事，主要用于进行持续集成。

一般，Dockerfile 共包括四部分：

- 基础镜像信息
- 维护者信息
- 镜像操作指令
- 容器启动时执行指令



当Node.js遇见Docker，下面介绍Docker在前端中的应用：

一个简单的Koa应用：

```javascript
const Koa = require('koa');
const app = new Koa();

// response
app.use(ctx => {
  ctx.body = 'Hello Koa!!';
});

app.listen(3000);
```



Dockerfile示例：

```dockerfile
FROM node:10

LABEL maintainer=itheima@itcast.cn

# 创建 app 目录
WORKDIR /app

# 把 package.json，package-lock.json(npm@5+) 或 yarn.lock 复制到工作目录(相对路径)
COPY ["package.json","*.lock","./"]

# 打包 app 源码
# 特别注意：要指定工作目录中的文件名
COPY src ./src

# 使用.dockerignore文件，上面两个COPY合并成一个
# COPY . .

# 使用Yarn安装 app 依赖
# 如果你需要构建生产环境下的代码，请使用：
# --prod参数
RUN yarn --prod --registry=https://registry.npm.taobao.org

# 对外暴露端口 -p 4000:3000
EXPOSE 3000

CMD [ "node", "src/index.js" ]
```



使用`docker build`打包：

```shell
docker build -t ${your_name}/${image_name}:${tag} .
```

这里的`your_name`代表的是远程仓库中的用户名，或者仓库地址; `image_name`为镜像名称，`tag`是给镜像打的标签，用于版本控制。例如：

```shell
docker build -t itheima/node-demo:1.0 .
```



打包过程：

```bash
$ docker build -t itheima/node-demo:1.0 .
Sending build context to Docker daemon  17.92kB
Step 1/8 : FROM node:10
 ---> 5a401340b79f
Step 2/8 : LABEL maintainer=itheima@itcast.cn
 ---> Using cache
 ---> dd01419f30d5
Step 3/8 : WORKDIR /app
 ---> Using cache
 ---> bb9a44851ec9
Step 4/8 : COPY . .
 ---> Using cache
 ---> b08ff37c5456
Step 5/8 : RUN ls -la /app
 ---> Using cache
 ---> 4e0e57e807a1
Step 6/8 : RUN yarn --prod --registry=https://registry.npm.taobao.org
 ---> Using cache
 ---> 96ab842b4766
Step 7/8 : EXPOSE 3000
 ---> Using cache
 ---> 505dda4e680f
Step 8/8 : CMD [ "node", "src/index.js" ]
 ---> Using cache
 ---> f60e25a577de
Successfully built f60e25a577de
Successfully tagged itheima/node-demo:1.0
```

**回顾前面的知识：**

如何使用？

还记得`docker run`命令吗？

```bash
docker run -d --name nodedemo -p 3000:3000 itheima/node-demo:1.0
```

然后使用`docker ps`来看运行状态

```bash
$ docker run -d --name nodedemo -p 3000:3000 itheima/node-demo:1.0
c863da9afea1558593843233aec08989184d8dafbb0f8443830d1e523104ab00
$ docker ps
CONTAINER ID        IMAGE                   COMMAND               CREATED             STATUS              PORTS                    NAMES
c863da9afea1        itheima/node-demo:1.0   "node src/index.js"   2 seconds ago       Up 1 second         0.0.0.0:3000->3000/tcp   nodedemo
```



### Docker-compose介绍

通过 Docker-Compose 用户可以很容易地用一个配置文件定义一个多容器的应用，然后使用一条指令安装这个应用的所有依赖，完成构建。Docker-Compose 解决了容器与容器之间如何管理编排的问题。

![docker-compose](./container/docker-compose.png)

Compose 中有两个重要的概念：

- 服务 (service) ：一个应用的容器，实际上可以包括若干运行相同镜像的容器实例。
- 项目 (project) ：由一组关联的应用容器组成的一个完整业务单元，在 docker-compose.yml 文件中定义。

Docker Compose 是 Docker 的独立产品，因此需要安装 Docker 之后在单独安装 Docker Compose .

**安装方法：**

```bash
#下载
sudo curl -L https://github.com/docker/compose/releases/download/1.20.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
#安装
chmod +x /usr/local/bin/docker-compose
#查看版本
docker-compose --version
```



**常见使用：**

前面的例子，使用`docker-compose`改写：

```yaml
version: '3'
services:
  mysql:
    image: mysql
    container_name: test-mysql
    ports:
    - "8000:3306"
    environment:
    - MYSQL_ROOT_PASSWORD=123456
```

在此文件的当前目录下，使用`docker-compose up -d`来执行。

生命周期管理：

创建：`run`/`up`

启动/停止/删除/重启：`start/stop/rm/restart`

检视/日志：`logs`/`ps`



看两个个复杂的应用：

搭建本地mongo + mongo-express服务

```yaml
version: '3.1'
services:
  mongo:
    image: mongo
    restart: always
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: 123456

  mongo-express:
    image: mongo-express
    restart: always
    ports:
      - 8081:8081
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: root
      ME_CONFIG_MONGODB_ADMINPASSWORD: 123456
```

给公司搭建一个git服务器吧？！

项目地址: https://github.com/sameersbn/docker-gitlab

推荐服务器配置：适用于10人的小团队~~~~

2core

2GB+2GB(swap) 

```yaml
version: '2'

services:
  redis:
    restart: always
    image: sameersbn/redis:4.0.9-1
    command:
    - --loglevel warning
    volumes:
    - /srv/docker/gitlab/redis:/var/lib/redis:Z

  postgresql:
    restart: always
    image: sameersbn/postgresql:10
    volumes:
    - /srv/docker/gitlab/postgresql:/var/lib/postgresql:Z
    environment:
    - DB_USER=gitlab
    - DB_PASS=password
    - DB_NAME=gitlabhq_production
    - DB_EXTENSION=pg_trgm

  gitlab:
    restart: always
    image: sameersbn/gitlab:11.11.0
    depends_on:
    - redis
    - postgresql
    ports:
    - "10080:80"
    - "10022:22"
    volumes:
    - /srv/docker/gitlab/gitlab:/home/git/data:Z
    environment:
    - DEBUG=false

    - DB_ADAPTER=postgresql
    - DB_HOST=postgresql
    - DB_PORT=5432
    - DB_USER=gitlab
    - DB_PASS=password
    - DB_NAME=gitlabhq_production

    - REDIS_HOST=redis
    - REDIS_PORT=6379

    - TZ=Asia/Kolkata
    - GITLAB_TIMEZONE=Kolkata

    - GITLAB_HTTPS=false
    - SSL_SELF_SIGNED=false

    - GITLAB_HOST=localhost
    - GITLAB_PORT=10080
    - GITLAB_SSH_PORT=10022
    - GITLAB_RELATIVE_URL_ROOT=
    - GITLAB_SECRETS_DB_KEY_BASE=long-and-random-alphanumeric-string
    - GITLAB_SECRETS_SECRET_KEY_BASE=long-and-random-alphanumeric-string
    - GITLAB_SECRETS_OTP_KEY_BASE=long-and-random-alphanumeric-string

    - GITLAB_ROOT_PASSWORD=123456
    - GITLAB_ROOT_EMAIL=itheima@itcast.cn
...
```



docker-compose在前端里面的使用：

Nodejs + mongodb + koa + vue的应用组合：

特别注意：

视频中，我们使用vue是在局部安装的，所以使用npx命令在进行创建。

vue官网推荐大家进行全局安装:

```bash
npm i -g @vue/cli

# 然后使用
vue create myproject
```



 docker-compose.yml

```yaml
version: '3'
services:
  web:
    image: web:1.0
    ports:
    - "8080:80"

  server:
    image: server:1.0
    ports:
    - "3000:3000"
    depends_on:
    - mongodb
    links:
    - mongodb:db

  mongodb:
    image: mongo
    restart: always
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: 123456
```

`depends_on` 决定了容器加载的先后顺序，这里`mongdb`、`web`先加载，`mongdb`创建完成之后，再来创建`server`。



### 自学补充知识

1. 什么是Kubernetes?

Kubernetes, 中文意思是舵手或导航员，它是一个容器集群管理系统，主要职责是容器编排（Container Orchestration）——启动容器，自动化部署、扩展和管理容器应用，还有回收容器。

文档: https://kubernetes.io/zh/

集群相关：[Mesos](http://mesos.apache.org/)，[Docker Swarm](https://github.com/docker/swarm)

2. 简单的DevOps流程介绍：

![devOps](./container/devOps-8692746.jpeg)

参考链接：[从一张图看Devops全流程](https://cloud.tencent.com/developer/article/1070465)

3.  Docker中的文件系统，深入理解原理：

![一图看尽Docker容器文件系统](./container/一图看尽Docker容器文件系统.png)

4. Docker的一般开发流程介绍：

   - 寻找基础镜像
   - 基于基础镜像编写Dockerfile脚本
   - 根据Dockerfile脚本创建项目镜像
   - 将创建的镜像推送到docker仓库 (根据自身需要，可做可不做)
   - 基于项目镜像创建并运行docker容器 (实现最终部署)

   这里面如果一扩展，就变成了自动化开发流程，比如：加入版本控制git -> 使用webhook -> jenkins自动打包  -> docker自动构建 -> 推送镜像 -> 生产环境部署



## 课程资源

Docker for Mac【系统：10.12以上】: https://download.docker.com/mac/stable/Docker.dmg

Docker for Windows【系统：专业版及企业版】: https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe

Docker ToolBox for Mac【系统：10.10.3以上】: https://download.docker.com/mac/stable/DockerToolbox.pkg 

Docker ToolBox for Windows【系统：windows7以上】：https://download.docker.com/win/stable/DockerToolbox.exe
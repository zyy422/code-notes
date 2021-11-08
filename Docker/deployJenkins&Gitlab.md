# Docker 部署Jenkins和Gitlab

## Docker核心思想
Build, Ship and Run Any App, Anywhere

一次封装，到处运行。传统虚拟化方式是在宿主机操作系统上，构建一个虚拟机管理软件，在这个管理软件上，运行了多个虚拟机操作系统。而对于Docker技术，属于操作系统级的虚拟化，内核通过创建多个虚拟的操作系统实例（内核和库）来隔离不同的进程。

## 一些概念
- Docker镜像： 它是一个只读的模版，里面包含了基本的操作系统环境和制定的应用程序。
- Docker容器： 它是一个从镜像创建的应用运行实例，而Docker正式用容器来隔离这些不同的应用。
- Docker仓库： 集中存放Docker镜像的场所
- Docker引擎： 支持不同操作系统和平台，为企业和用户提供简单安全弹性的容器集群编排和管理
  
## 一些操作
```shell
# 下载一个ubuntu镜像（默认最新）
docker pull ubuntu 

# 查看所有已下载镜像
docker images

# 查看镜像的详细信息
docker inspect ubuntu

# 删除镜像
docker rm ubuntu

# 创建一个容器,创建后容器并不会直接运行，而是处于created状态
docker create ubuntu

# 启动容器
docker start ubuntu

# 根据镜像直接创建并且启动容器
# 宿主机的80端口映射到容器的81端口
# 宿主机的/home/gitlab目录映射到容器的/home/gitlab-container
# 以后台模式运行
docker run -p 80:81 -d -v /home/gitlab:/home/gitlab-container nginx

# 进入docker容器中
docker exec -it [dockerid] bash
```

## Jenkins部署
- 先拉取一个Jenkins镜像
    ```
    jenkins/jenkins:lts
    ```
- 创建一个Jenkins的工作目录，用于进行映射
- 安装和启动镜像
    ```
    docker run -d --name jenkins -p 8081:8080 -v /data/jenkins_home:/var/jenkins_home jenkins/jenkins:lts;
    ```
- 对Jenkins进行一些必要的配置，比如登录时的密码，这些配置信息都在进行映射的目录中

## Gitlab部署
- 拉取一个Gitlab镜像
  ```
  docker pull gitlab/gitlab-ce
  ```
- 创建并启动一个Gitlab容器
    ```
    $ GITLAB_HOME = /home/docker/gitlab     # 建立gitlab本地目录
    $ docker run -d \
    --hostname gitlab.example.com\          # 指定容器域名,创建镜像仓库用
    -p 8443:443 \                           # 容器443端口映射到主机8443端口(https)
    -p 8080:80 \                            # 容器80端口映射到主机8080端口(http)
    -p 2222:22 \                            # 容器22端口映射到主机2222端口(ssh)
    --name gitlab \                         # 容器名称
    --restart always \                      # 容器退出后自动重启
    -v $GITLAB_HOME/config:/etc/gitlab \    # 挂载本地目录到容器配置目录
    -v $GITLAB_HOME/logs:/var/log/gitlab \  # 挂载本地目录到容器日志目录
    -v $GITLAB_HOME/data:/var/opt/gitlab \  # 挂载本地目录到容器数据目录
    gitlab/gitlab-ce:latest                 # 使用的镜像:版本
    ```
- 一些默认配置文件,都在```/home/docker/gitlab/```中
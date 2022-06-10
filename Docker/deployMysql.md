# Dockerfile 构建MySQL基础镜像环境

## 选择合适的mysql基础镜像
```docker
# 由于m1芯片，需要指定平台
docker pull --platform linxu/amd64 mysql:5.7
```

## 编写Dockerfile文件
```docker
# 指定基础镜像版本
FROM --platform=linux/x86_64 mysql:5.7

MAINTAINER Stephen Joey
# 指定MySQL数据库root密码
ENV MYSQL_ROOT_PASSWORD=123456

# 增加几个我自己需要导入的shell脚本和配置文件
ADD mysql.cnf /etc/mysql/mysql.conf.d/my.cnf
ADD admin.sql /admin.sql
ADD archiveMenu.sql /archiveMenu.sql
ADD init.sh /init.sh

# 提前给初始化的shell增加执行权限
CMD ["chmod","777","/init.sh"]

# 对外暴露3306端口
EXPOSE 3306
```

## 根据Dockerfile构建镜像
```
docker build -t my-builder/mysql:1.0 ./
```

## 启动镜像并初始化数据库
```docker
docker run -d  -t -i -p 3306:3306 [image id] /bin/bash 

# 如果不加-d,无法后台运行，自身的terminal一旦退出，则容器停止
```
- -d：以Daemonized执行 
- -t: 分配一个伪终端
- -i:  保持打开容器的标准输入
- -p: 端口映射

## 进入到容器中

```dockerfile
docker exec -it [container ID] /bin/bash
```

- 如果仅制定-i,那么容器接受标准输入，但是没有分配伪终端
- 如果仅制定-t，那么无法接受标准输入，因此所有命令无效。

## 启动一个私人docker仓库 [Server]

```
docker run -d -p 5000:5000 --restart=always --name registry registry
```

## 允许不安全registry [Client]
```yaml
{
  "features": {
    "buildkit": true
  },
  "insecure-registries": [
    "159.75.247.231:5000"
  ],
  "experimental": false,
  "builder": {
    "gc": {
      "defaultKeepStorage": "20GB",
      "enabled": true
    }
  }
}
```

## 镜像打tag后才能上传 [Client]
```docker
docker tag [container Id] [remote uri]:[tag name]
```

## 镜像上传私有仓库 [Client]

```docker
docker push [remote uri]:[tag name]
```

## 私有仓库拉取镜像 [Server]
```docker
docker pull [127.0.0.1]:[tag name]
```

# 常用命令

yun install docker 安装docker

systemctl start docker  启动docker

systemctl enable docker 设置开机启动docker

systemctl stop docker 关闭docker

 

docker search name  在网站上检索name的镜像

docker pull  name:tag  下载指定tag的镜像

docker images 查看本地镜像

docker rmi id 删除指定id的镜像

 

docker run --name mytomcat -d -p 8080:8080 tomcat:latest   根据镜像绑定端口创建并启动容器

docker ps  -a 查看所有的容器

docker stop id 停止指定id的容器

docker start id 启动指定id的容器

docker rm id  删除指定id的容器

 

docker logs id   查看容器的日志

docker exec -it 62349aa31687 /bin/bash 进入指定容器的指定目录





# 创建一个新容器来部署springboot项目

首先生成springboot项目的jar包

然后编写一个Dockerfile文件

```dockerfile
From java:8
VOLUME /tmp
ADD testdocker.jar /app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

[Dockerfile文件如何编写](http://xuewei.world:8000/2019/12/18/dockerfile-%e4%bb%8b%e7%bb%8d/)

然后把jar包和dockerfile上传到linux系统中

进入jar包和dockerfile的文件夹中运行



```
docker build -t 镜像名 .
```

注意后面的点，代表当前目录，也就是jar包的目录



然后就正常操作即可


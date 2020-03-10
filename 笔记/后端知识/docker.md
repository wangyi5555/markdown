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
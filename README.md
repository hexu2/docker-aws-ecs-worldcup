<h1 align="center" style="margin: 30px 0 30px; font-weight: bold;">World CUP 2022 v1.0.0</h1>
<h4 align="center">Docker (ngnix,static htmls) + AWS ECS</h4>
<p align="center">
	<a href="https://github.com/hexu2/docker-aws-ecs-worldcup"><img src="https://img.shields.io/badge/worldcup-v1.0.0-brightgreen.svg"></a>
</p>

* Docker hub as image repositories
https://hub.docker.com/repository/docker/2212925883/worldcup 
* Tech stack（[Docker](https://www.docker.com/) | [AWS](https://www.docker.com/) )

## function URL
http://worldcupalb-1548022113.us-east-1.elb.amazonaws.com/index.html


## Source codes
~~~
worldcup     
├── conf.d       // ngnix config,Port [80]
├── css/*
├── fancybox/*
├── fonts/*             
├── images/*             
├── js/*             
├── index.html   // entry page
├── Dockerfile   // Dockerfile for build image

~~~

## build docker image
``````````
1. Create Dockerfile
#FROM nginx:latest
FROM nginxdemos/hello:latest
COPY ./css  /usr/share/nginx/html 
COPY ./fancybox /usr/share/nginx/html 
COPY ./fonts /usr/share/nginx/html 
COPY ./js /usr/share/nginx/html 
COPY ./images /usr/share/nginx/html 
COPY . /usr/share/nginx/

2. Build image

hexudeMacBook-Air:worldcup hexu$ sudo docker build -t worldcup:4.0 .


hexudeMacBook-Air:worldcup hexu$ docker images
REPOSITORY                        TAG       IMAGE ID       CREATED              SIZE
worldcup                          1.0       a619e414d6a7   About a minute ago   144MB
nginx                             latest    ac8efec875ce   36 hours ago         142MB
ubuntu                            latest    a8780b506fa4   4 weeks ago          77.8MB
drone/drone-ci-docker-extension   0.2.0     9ac8b4ce3f28   5 weeks ago          177MB

3. Local testing : Run image
hexudeMacBook-Air:worldcup hexu$ docker run -p 8080:80 -d worldcup:1.0
4e88b1d0e65f9420dede31e4753cdbdfe1241d39bc4ff922d636e8527ffa6517
hexudeMacBook-Air:worldcup hexu$ 


4: push to hub
1).若是首次构建镜像，使用这个命令:
docker build -t <hub-user>/<repo-name>[:<tag>]
docker build -t 2212925883/worldcup:1.0 

2).若已存在镜像，且不用修改内容，通过重新命名现有的本地镜像:

docker tag <existing-image> <hub-user>/<repo-name>[:<tag>]

hexudeMacBook-Air:worldcup hexu$ docker tag worldcup:1.0 2212925883/worldcup:1.0
hexudeMacBook-Air:worldcup hexu$ 

docker tag worldcup  https://hub.docker.com/repository/docker/2212925883/worldcup

3).若镜像内容有修改，通过更改本地镜像:

docker commit <existing-container> <hub-user>/<repo-name>[:<tag>]

docker commit worldcup 2212925883/worldcup:1.0 


通过上面的任何一种方法可以将本地镜像命名为docker hub标准的镜像，最后可以将此镜像推送到Docker Hub

docker push <hub-user>/<repo-name>:<tag>


hexudeMacBook-Air:worldcup hexu$ docker push 2212925883/worldcup:1.0
The push refers to repository [docker.io/2212925883/worldcup]
825b38f8dbe6: Pushed 
7b72d5d921cb: Pushed 
aa3739f310f5: Pushed 
6906edffc609: Pushed 
f88642d922a1: Pushed 
2842e5d66803: Pushed 
b5ebffba54d3: Pushed 
1.0: digest: sha256:53b9c67855f34a4055c5fa9cbd6c8273ccdf738a120854cdb2d71221b4d47a7f size: 1781
hexudeMacBook-Air:worldcup hexu$ 


4) 查看当前运行镜像

hexudeMacBook-Air:worldcup hexu$ docker ps
CONTAINER ID   IMAGE                        COMMAND                  CREATED          STATUS          PORTS                  NAMES
3dac3c0e6d29   worldcup:2.1                 "/docker-entrypoint.…"   2 minutes ago    Up 2 minutes    0.0.0.0:8085->80/tcp   awesome_wozniak
16278db57e4a   2212925883/worldcup:1.0      "/docker-entrypoint.…"   6 minutes ago    Up 6 minutes    0.0.0.0:8084->80/tcp   youthful_gould
145d3d4abc58   2212925883/worldcup:latest   "/docker-entrypoint.…"   8 minutes ago    Up 8 minutes    0.0.0.0:8082->80/tcp   busy_mcclintock
823ca9ef7109   2212925883/worldcup:latest   "/docker-entrypoint.…"   22 minutes ago   Up 22 minutes   0.0.0.0:8080->80/tcp   pensive_elbakyan
91dea9de7059   worldcup:2.0                 "/docker-entrypoint.…"   49 minutes ago   Up 49 minutes   0.0.0.0:8081->80/tcp   flamboyant_satoshi
hexudeMacBook-Air:worldcup hexu$ 
hexudeMacBook-Air:worldcup hexu$ 

``````````

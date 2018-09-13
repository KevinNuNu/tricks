# docker基础命令操作

docker images:查看存在的镜像
```
sudo docker images 
```

docker ps:查看正在运行的容器
```
sudo docker ps -a
# 参数'-a'：查看所有存在的容器(包括已经被stop掉的)
```

docker pull:从dockerhub远端拉取镜像
```
sudo docker pull kevinnunu/caffe2-py2-cuda9-cudnn7:latest
# kevinnunu(仓库名)/caffe2-py2-cuda9-cudnn7(镜像名):latest(标签)
```

docker tag:标记某一镜像(可以理解为给镜像重命名)，并将其归入某一仓库
```
sudo docker tag ubuntu:16.04 kevinnunu/nunu_ubuntu:16.04
# ubuntu:16.04(需要修改的镜像)
# kevinnunu(新的仓库名，后续push时候有讲究)/nunu_ubuntu(新的镜像名):16.04(新的标签)
```

docker commit:基于做了修改后的容器，重新生成一个新的镜像，语法类似git的commit，这也是除了用Dockerfile生成镜像之外另一种常用的方式
```
sudo docker commit -a "Author: Wang Nu" -m "first commit" a404c6c174a2 kevinnunu/nunu_ubuntu:16.04
# 参数-a:添加作者信息
# 参数-m:添加此处commit的解释信息
# a404c6c174a2(容器ID)
# kevinnunu(新的仓库名)/nunu_ubuntu(新的镜像名):16.04(新的标签)
```

docker push:将本地的镜像上传到dockerhub，要先登陆到dockerhub
```
sudo docker push kevinnunu/keypoints:firstpush
# 提交的镜像的仓库名(kevinnunu)必须是dockerhub上登陆的用户名，不然push不上dockerhub
# 遇到仓库名和用户名不匹配导致无法push的问题，可以用docker tag修改仓库名或者更换dockerhub账户解决
```

docker build:使用Dockerfile创建镜像
```
sudo docker build --no-cache -t kevinnunu/new_image:usr_Dockerfile_build .
sudo docker build -f /path/to/a/Dockerfile --no-cache -t kevinnunu/new_image:usr_Dockerfile_build .
# 注意！！！ 末尾有一个“空格”和一个“点”，必须要加！(第一次的时候被坑了好久...)
# 第一种是使用当前目录下的Dockerfile来创建镜像，所以必须先cd到Dockerfile所在文件目录下
# 第二种使用参数-f，可以指定Dockerfile目录来生成，此时就不需要先cd到Dockerfile所在文件目录下
# 参数-t:给生成的镜像上标签 kevinnunu(仓库名)/new_image(镜像名):usr_Dockerfile_build(标签)
# 参数--no-cache:创建镜像的过程不使用缓存，可以缩小创建镜像的大小
```

docker run:创建一个新的容器
```
sudo docker run --link=myMongoDB:mongodb -v /wn:/data -p 9999:8008 --name keypoints -it kevinnunu/keypoints:latest /bin/bash
# 参数--link:添加链接到另一个容器myMongoDB
# 参数--name:为容器指定个名字keypoints
# 参数-v:进行文件映射，将 “外部的/wn文件夹中的内容” 映射到 “容器内部的/data文件夹”
# 参数-p:进行端口映射，将 “外部的9999端口” 映射到 “容器内部的8008端口”
# 参数-it: -i以交互模式运行容器，-t为容器重新分配一个伪输入终端，通常-i-t同时使用
# 参数-d:后台运行容器，并返回容器ID
# 参数-c:可以在后面跟你进入容器后想要执行的操作，用“”包起来
# /bin/bash:表示载入容器后运行bash，docker中必须要保持一个进程的运行，要不然整个容器就会退出
```

docker stop/kill:终止一个运行的容器
```
sudo docker stop/kill keypoints
# keypoints(容器名 or 容器ID)
```

docker start + docker attach:重新启动一个被终止的容器，并加载到容器中
```
sudo docker start keypoints
sudo docker attach keypoints
# keypoints(容器名 or 容器ID)
# 先用docker start唤醒容器，但并没有进到容器中
# 再用docker attach加载到容器中
```

docker exec:进入到一个唤醒状态的容器中，和docker run的命令很像
```
sudo docker exec -it keypoints /bin/bash
# 参数-d:后台运行
# 参数-it: -i以交互模式运行容器，-t为容器重新分配一个伪输入终端，通常-i-t同时使用
```

docker rm + docker rmi:删除容器 / 删除镜像
```
sudo docker rm keypoints
sudo docker rmi kevinnunu/keypoints:latest
# keypoints(容器名 or 容器ID)
# kevinnunu/keypoints:latest(镜像名 or 镜像ID)
```

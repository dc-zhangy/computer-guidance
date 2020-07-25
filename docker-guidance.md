# 1.镜像

## 下载镜像
- ```docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]```
- 例如：``` docker pull ubuntu:18.04 ```


## 列出镜像列表
- ``` docker image ls```
## 删除镜像

- ```docker image rm [选项] <镜像1> [<镜像2> ...]```
- 例如 ```docker image rm ubuntu:18.04```  

# 2.容器
## 查看所有容器
- ```docker ps -a```
## 启动容器
启动容器有两种方式，一种是基于镜像新建一个容器并启动，另外一个是将在终止状态（stopped）的容器重新启动。
### 新建启动 
```docker run```

下面的命令输出一个Hello World，之后终止容器<p>
```docker run ubuntu:18.04 /bin/echo Hello world```

下面的命令则启动一个bash终端，允许用户进行交互，并给容器取名ubuntu-test <p> 
```docker run -it --name ubuntu-test ubuntu:18.04 /bin/bash```<p>
要退出终端，直接输入 ```exit```，退出后容器停止运行。

### 启动已停止运行的容器
```docker start ```<p>
```docker start ubuntu-test```启动容器ubuntu-test，但此时还没有进入此容器
## 后台运行
在大部分的场景下，我们希望 docker 的服务是在后台运行的，我们可以过 ```-d ```指定容器的运行模式<p>
```docker run -itd --name ubuntu-test ubuntu /bin/bash```<p>
**注**：加了 -d 参数默认不会进入容器，想要进入容器需要使用指令```docker exec```
## 停止容器
- ```docker stop <容器 ID>```<p>
- 停止的容器可以通过 ```docker restart <容器 ID>``` 重启
## 进入容器
- ```docker exec```
- ```docker exec -it ubuntu-test  /bin/bash```
## 删除容器
- ```docker rm```
- ```docker rm -f ubuntu-test```,```-f```删除一个运行中的容器
- ``` docker container prune```清理掉所有处于终止状态的容器

# 3.端口映射
容器中可以运行一些网络应用，要让外部也可以访问这些应用，可以通过 -P 或 -p 参数来指定端口映射
## 映射所有接口地址
使用 ```hostPort:containerPort``` 格式本地的 5000 端口映射到容器的 5000 端口<p>
``` docker run -d -p 5000:5000 training/webapp python app.py```

## 映射到指定地址的指定端口
可以使用 ```ip:hostPort:containerPort``` 格式指定映射使用一个特定地址<p>
```docker run -d -p 127.0.0.1:5000:5000 training/webapp python app.py```
## 映射到指定地址的任意端口
使用 ```ip::containerPort``` 绑定 localhost 的任意端口到容器的 5000 端口，本地主机会自动分配一个端口。<p>
```docker run -d -p 127.0.0.1::5000 training/webapp python app.py```

# 4.宿主机与容器互相拷贝传递文件
## 从容器拷贝文件到宿主机
```docker cp mycontainer：/opt/testnew/file.txt /opt/test/```<p>
将容器mycontainer中路径/opt/testnew/下的文件file.txt拷贝到宿主机opt/test/路径下
## 从宿主机拷贝文件到容器
```docker cp /opt/test/file.txt mycontainer：/opt/testnew/```
将宿主机中路径/opt/test/下的文件file.txt拷贝到容器mycontainer的/opt/testnew/路径下

# 5. 挂载主机目录到镜像
```docker run -it -v /home/dock/Downloads:/usr/Downloads ubuntu64 /bin/bash```<p>
通过```-v```参数，冒号前为宿主机目录，必须为绝对路径，冒号后为镜像内挂载的路径<p>
**高级方式挂载数据卷：**[参考这里](https://yeasy.gitbooks.io/docker_practice/content/data_management/volume.html)
# 6. 使用dockerfile定制镜像
[参考这里](https://yeasy.gitbooks.io/docker_practice/content/image/build.html)


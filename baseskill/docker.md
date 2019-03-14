# docker

## 1、基本概念

Docker 主要包含三个基本概念，分别是镜像、容器和仓库，理解了这三个概念，就理解了 Docker 的整个生命周期。

- **镜像**：Docker 镜像是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。镜像不包含任何动态数据，其内容在构建之后也不会被改变。
- **容器**：容器的实质是进程，但与直接在宿主执行的进程不同，容器进程运行于属于自己的独立的命名空间容器可以被。创建、启动、停止、删除和暂停等等，说到镜像与容器之间的关系，可以类比面向对象程序设计中的类和实例。
- **仓库**：镜像构建完成后，可以很容易的在当前宿主机上运行，但是，如果需要在其它服务器上使用这个镜像，我们就需要一个集中的存储、分发镜像的服务，Docker  Registry 就是这样的服务。一个 Docker Registry  中可以包含多个仓库；每个仓库可以包含多个标签；每个标签对应一个镜像，其中标签可以理解为镜像的版本号。

## 2、Docker 三剑客

**docker-compose**：Docker 镜像在创建之后，往往需要自己手动 pull 来获取镜像，然后执行 run 命令来运行。当服务需要用到多种容器，容器之间又产生了各种依赖和连接的时候，部署一个服务的手动操作是令人感到十分厌烦的。

dcoker-compose 技术，就是通过一个 `.yml` 配置文件，将所有的容器的部署方法、文件映射、容器连接等等一系列的配置写在一个配置文件里，最后只需要执行 `docker-compose up` 命令就会像执行脚本一样的去一个个安装容器并自动部署他们，极大的便利了复杂服务的部署。

**docker-machine**：Docker 技术是基于 Linux 内核的 cgroup 技术实现的，那么问题来了，在非 Linux 平台上是否就不能使用 docker 技术了呢？答案是可以的，不过显然需要借助虚拟机去模拟出 Linux 环境来。

docker-machine 就是 docker 公司官方提出的，用于在各种平台上快速创建具有 docker 服务的虚拟机的技术，甚至可以通过指定 driver 来定制虚拟机的实现原理（一般是 virtualbox）。

**docker-swarm**：swarm 是基于 docker 平台实现的集群技术，他可以通过几条简单的指令快速的创建一个 docker 集群，接着在集群的共享网络上部署应用，最终实现分布式的服务。

## 3、镜像的获取开启与基础使用

```
docker pull ubuntu:16.04
```

1、Docker 镜像仓库地址若不写则默认为 Docker Hub，而仓库名是两段式名称，即 `<用户名>/<软件名>`。对于 Docker Hub，如果不给出用户名，则默认为 library，也就是官方镜像。

2、如果你不指定一个镜像的版本标签，例如你只使用 ubuntu，docker 将默认使用 ubuntu:latest 镜像。

`docker run --name webserver -d -p 4000:80 nginx`

- --name:命名开启的docker;
- -d:后台启动；
- -p:制定映射的端口

如果开启的ngnix有tag标签，需要使用NGING:TAG来表示，默认表示latest。

`docker run -d -p 127.0.0.1:5000:5000/udp training/webapp python app.py`

制定udp端口以及ip地址，直接运行容器中的命令脚本。

`docker run -it nginx bash`

在容器没有开启的情况下，可以使用此命令直接进入容器。

- -it:交互式界面；

`docker exec -it webserver bash`

在容器已经开启的情况下，可以用此命令进入容器。

`docker images`

查看已有的镜像文件。

`docker ps`

查看已经运行的容器。

`docker start webserver && docker stop webserver`

启动和关闭docker ，关闭启动docker不会影响容器内修改的数据。

`docker rm webserver`   

删除docker容器，注意删除后容器内修改的数据全部会消失。

`docker rmi 9bf5a174ba16`   

删除docker镜像，注意删除镜像需要先删除关联的容器和子镜像。

`docker search oracle`

查询镜像库镜像文件。

`docker logs b58e26807a6eb7605f3b`

查询容器的日志，只能在容器运行时候查看。

## 4、容器更新

场景：现在启动一个nginx，首页显示为welcome to nginx。现在需要修改首页显示为hello docker,并且重新打包为一个新的镜像。

两种方法：1、从已有创建的容器中更新镜像，并且提交这个镜像；2、使用dockerfile指令来创建一个新的镜像。

```
root@sullivan-KVM:~# docker exec -it webserver bash
echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
```

修改nginx中的主页。

`docker diff webserver`

查看容器中修改的内容

`docker commit -m="has update" -a="zhuliang" webserver ngnix:v2`

- -m:修改的注释说明信息；
- -a:修改作者信息；

Docker 提供了一个 docker commit 命令，可以将容器的存储层保存下来成为镜像。换句话说，就是在原有镜像的基础上，再叠加上容器的存储层，并构成新的镜像。

`docker container ls -a`  查看所有已经生成的容器

`docker inspect webserver1` 查看容器的具体属性

**至于dockerfile的方式需要使用的时候再说吧**

## 5、数据持久

场景：当对容器进行docker rm的时候，存储的数据就会随着容器一起消失，如何保持数据持久？

`docker volume create my-column-2`  会在/var/lib/docker/volumes/生成一个my-column-2的目录

`docker run -d -p 4000:80 --name leadsino -v my-column-2:/root nginx` 挂载目录到root目录下

至此挂在结束，之后容器内/root的文件改变时会同步到/var/lib/docker/volumes/my-column-2/_data目录下，同时外部目录的文件有改变时也会同步到容器内的root目录中。

```
docker run -it -v my-volume:/mydata nginx
```

一句话表示形式，如果没有my-volume那么会直接生成。

```
ocker run -it -v /mydata alpine sh
```

如果不制定目录，那么会在/var/lib/docker/volumes/自动生成一个随机数目录。










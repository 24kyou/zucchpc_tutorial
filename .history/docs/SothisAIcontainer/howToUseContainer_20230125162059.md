# notebook服务和容器服务的区别。  

关于容器和notebook的使用方式请见：[平台使用教程](../SothisAI/UsePlatform.md)

容器和notebook本质上是一样，底层都是用docker实现，选择notebook则是可以在网页端


## 使用容器的好处

1. 可以自定义资源配置，不会局限于notebook中提供的固定的资源配置

2. 在拜托了notebook的束缚之后，可以通过本地编程软件原创连接到容器上（虽然notebook也可以远程连接）。详细方式见[ssh连接](ssh.md)



# 为何推荐使用ssh作为镜像而不是jupyter镜像

## 1.缺少必要脚本配置

自己配置jupyter的镜像除了要安装jupyterlab，notebook等环境后，还需要配置与前端web服务兼容的脚本。

缺少脚本会导致容器/作业无法运行，目前厂家那边没有给我们能够完美自己搭建jupyter镜像的方案，目前已有方案都会导致jupyter镜像失败。

而配置ssh镜像只需要有sshd、sudo环境即可。

## 2.不适应web界面的notebook

自己已经搭建了一套工作环境，想通过远程连接到容器进行工作的。就不需要配置一个有前端web页面的jupyter镜像了，一个ssh镜像就好。远程连接方法见[ssh连接](ssh.md)



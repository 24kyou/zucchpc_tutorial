# 平台与镜像
> 本文档主要是介绍与平台相适应的镜像构建教程
## 镜像为何会部署时间很长
用户提交的镜像是存储在[Harbor(用于存储docker images的服务)]()上的,使用时会从Harbor上下载到计算节点上,下载所需时间依据镜像大小而定

## 为何自己上传的镜像无法使用
这往往是是因为本地环境下运行的环境缺少必要包,导致执行脚本无法将镜像与平台进行对接从而关闭了镜像

### 必要环境
> 以下环境对具体版本要求不大
1. **sudo**  

在ubuntu/centos的镜像中正常下载一些环境所必需的。

2. **ssh**

使用 sudo yum/apt/apt-get install openssh-server 进行下载,且端口仅能为22,即默认端口。只有配置了ssh服务，才能作为base进行镜像提交且能正常使用。

3. **conda/miniconda**

在容器中推荐使用 conda install 而不是pip install。
> <font style ="color:red">此方法可有可无，conda是与jupyter相配套，由于jupyter不再需要所以conda也可有可无</font >

1. **jupyterlab jupyterhub notebook**

想要在web端中使用notebook功能，则要求容器内已经存在jupyterlab包环境（其他环境可选）且安装目录在/opt/conda/下面（即conda install的默认环境）。在上传时勾选 **jupyter**。

> <font style ="color:red">此方法可有可无，废弃原因详情见[使用ssh而不是notebook作为镜像源](howToUseContainer.md)</font >

### 关于docker/nvidia-docker 版本要求
存在一些镜像需要要求使用特定版本的docker/nvidia-docker来进行启动（<font style="color:red">说的就是nvidia官方的docker</font>），目前服务器上的docker版本为**18.09** 。

### cuda版本问题
部分镜像存在 /usr/lcoal/的路径下不存在cuda，主要是因为 cuda安装有runtime和devel，因此对应的镜像也有两个版本，devel版本存在/usr/local/cuda,runtime版本不存在。下载镜像时请注意这点。

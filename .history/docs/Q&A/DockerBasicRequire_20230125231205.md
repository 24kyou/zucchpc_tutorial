### 惊醒运行的必要环境
> 以下环境对具体版本要求不大
1. **sudo**  

在ubuntu/centos的镜像中正常下载一些环境所必需的。

2. **ssh**

使用 sudo yum/apt/apt-get install openssh-server 进行下载,且端口仅能为22,即默认端口。只有配置了ssh服务，才能作为base进行镜像提交且能正常使用。

3. **conda/miniconda**

在容器中推荐使用 conda  而不是pip,且使用 conda 作为包管理器，方便了后续的ssh操作识别当前环境。


1. **jupyterlab/jupyterhub notebook**

想要在web端中使用notebook功能，则要求容器内已经存在jupyterlab包环境（其他环境可选）且安装目录在/opt/conda/下面（即conda install的默认环境）。

并且在上传镜像时需要勾选 **jupyter**。


### 关于docker/nvidia-docker 版本要求
存在一些镜像需要要求使用特定版本的docker/nvidia-docker来进行启动（<font style="color:red">说的就是nvidia官方的docker</font>），目前服务器上的docker版本为**18.09** 。

### cuda版本问题
部分镜像存在 /usr/lcoal/的路径下不存在cuda，主要是因为 cuda安装有runtime和devel，因此对应的镜像也有两个版本，devel版本存在/usr/local/cuda,runtime版本不存在。下载镜像时请注意这点。
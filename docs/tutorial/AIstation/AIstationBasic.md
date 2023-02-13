# AI station 使用手册技术说明
具体使用手册见附件，此处仅就一些技术区别、关键信息进行说明。

## AI station docker镜像环境配置要求
集群上的docker engine参数信息： 
```sh
NVIDIA Docker: 2.5.0
Client: Docker Engine - Community
 Version:           20.10.7
 API version:       1.41
 Go version:        go1.13.15
 #Git commit:       
 #Built:            
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.7
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.13.15
  #Git commit:       
  #Built:          
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.6
  #GitCommit:        
 nvidia:
  Version:          1.0.0-rc95
  #GitCommit:       
 docker-init:
  Version:          0.19.0
  #GitCommit:        
```
## dockerfile 文件模版
具体镜像使用细节见附件User.pdf中dockerfile相关内容，主要由**openSSH**、**openSSl**、**python3**、**jupyterlab**、**tini**组成，其余组件由使用人员自行配置
```sh
# base image
FROM ubuntu:18.04

MAINTAINER {xxx}

# install openssh and openssl 
RUN set -ex \
	&& apt-get -o Acquire::Check-Valid-Until=false  -o Acquire::Check-Date=false   update \
	&& apt-get install -y --no-install-recommends openssh-client openssh-server \
	&& mkdir -p /var/run/sshd \
	&& /usr/bin/ssh-keygen -A

# Allow OpenSSH to talk to containers without asking for confirmation
RUN set -ex \
	&& cat /etc/ssh/ssh_config | grep -v StrictHostKeyChecking > /etc/ssh/ssh_config.new \
	&& echo "    StrictHostKeyChecking no" >> /etc/ssh/ssh_config.new \
	&& cat /etc/ssh/sshd_config | grep -v  PermitRootLogin> /etc/ssh/sshd_config.new \
	&& echo "PermitRootLogin yes" >> /etc/ssh/sshd_config.new \
	&& mv /etc/ssh/ssh_config.new /etc/ssh/ssh_config \
	&& mv /etc/ssh/sshd_config.new /etc/ssh/sshd_config

# start install python3
## python version
ARG python=3.6.11
ENV PYTHON_VERSION=${python}

# start install python3
## python version
ARG python=3.6.11
ENV PYTHON_VERSION=${python} 

RUN sed -ex \
	&& apt-get -o Acquire::Check-Valid-Until=false  -o Acquire::Check-Date=false   update \
	&& apt-get install -y --allow-downgrades --allow-change-held-packages --no-install-recommends \
	&& apt-get install -y wget build-essential libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev zlib1g-dev libsqlite3-dev \
	&& cd /home \
	&& wget https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tgz \
	&& tar -zxvf Python-${PYTHON_VERSION}.tgz \
	&& cd Python-${PYTHON_VERSION} \
	&& ./configure \
	&& make \
	&& make install \
	&& make clean \
	&& rm -rf Python-${PYTHON_VERSION} 	 

## install python3-pip
RUN set -ex \
	&& apt-get install -y python3-pip \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/* 

## set python3
RUN set -ex \
	&& if [ -e /usr/bin/python ]; then mv /usr/bin/python /usr/bin/python27; fi \
    && if [ -e /usr/bin/pip ]; then mv /usr/bin/pip /usr/bin/pip-python27; fi \
    && ln -s /usr/local/bin/python3 /usr/bin/python \
    && ln -s /usr/local/bin/pip3 /usr/bin/pip

# end install python3

# install jupyterlab                                                                                                                                       
RUN set -ex \
	&& pip install --upgrade pip \
	&& pip --no-cache-dir install jupyter jupyterlab \
	&& rm -rf /root/.cache/pip/http/*

## configure jupyterlab                                                                                                                                             
RUN set -ex \
	&& mkdir /etc/jupyter/ \
	&& wget -P /etc/jupyter/  https://raw.githubusercontent.com/Winowang/jupyter_gpu/master/jupyter_notebook_config.py \
	&& wget -P /etc/jupyter/ https://raw.githubusercontent.com/Winowang/jupyter_gpu/master/custom.js 

## tini
ENV TINI_VERSION v0.19.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini-amd64 /tini
##github 上对应下载链接改变，此处注意
RUN chmod +x /tini
```

## MIG技术

MIG技术是nvidia公司随A100系列、H100系列、A30系列高性能GPU一同推出的技术，通过物理硬件上的隔离来实现将1块GPU显卡分成多张GPU卡提供给多个用户同时使用。具体实现原理、参数细节和配置实践见[MIG-GPU简介与A100-MIG实践详解](https://zhuanlan.zhihu.com/p/558046644)





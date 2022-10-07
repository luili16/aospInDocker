---
title: docker环境下编译android源码
---



### 1 安装docker

+ 快速备忘

  + ubuntu环境下安装docker [Install Docker Engine]( https://docs.docker.com/engine/install/ubuntu/)

  + [non-root user](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user)

     ```sh
      $ sudo groupadd docker
      $ sudo usermod -aG docker $USER
     ```

   记得要退出当前用户，重新登录

### 2 目录结构

	|-- aospInDocker
	
	       |-- aosp    
	
	       |-- docker 
	
	       |-- AospMake 

	+ aosp 安卓源码存放的位置
	+ docker Dockerfile相关的文件
	+ AospMake 工具脚本

### 3 编译DockerFile

```shell
$ cd aospInDocker
$ chmod u+x AospMake
$ ./AospMake docker 
# 等待docker镜像创建成功...

```



### 4 下载和编译安卓源码

```
$ ./AospMake aosp 
# 进入docker交互环境
# 以下所有都在docker环境中执行
# 下载过程可参考 https://mirrors.tuna.tsinghua.edu.cn/help/AOSP/ 
user@liulixin:~/aosp$ repo init -u https://mirrors.tuna.tsinghua.edu.cn/git/AOSP/platform/manifest
user@liulixin:~/aosp$ repo sync -j4
# 漫长的同步过程...
user@liulixin:~/aosp$ source build/envsetup.sh
user@liulixin:~/aosp$ lunch
```



### 5 FQ

+ python源码安装从官网下载[Python-3.7.10.tgz](https://www.python.org/ftp/python/3.7.10/Python-3.7.10.tgz)执行会报错_ssl not imported，回退到[Python-3.6.10.tgz](https://www.python.org/ftp/python/3.6.10/Python-3.6.10.tgz)解决
+ 在docker环境里curl https://mirrors.tuna.tsinghua.edu.cn/help/AOSP/ 提示证书验证错误，但是在本地环境没有问题，这个没有定位到准确的原因，但是可以通过设置环境变量 PYTHONHTTPSVERIFY=0来解决
+ repo sync -j4 结束会报错误 在[这里](https://blog.csdn.net/tkwxty/article/details/121333540)找到了解决方案

​	    <img src="docker环境下编译android/1.png" alt="docker环境下编译android" style="zoom:50%;" />






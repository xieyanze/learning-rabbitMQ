docker 安装
===
> 操作系统建议centos7 或 redhat7 以上，linux内核版本大于2.6

### 参考文档

使用docker官方仓库安装，最新版本方法，请参考[官方文档](https://docs.docker.com/engine/installation/)

### CentOS

```bash
	sudo yum install docker-ce
```

### Ubuntu

```bash
	sudo apt-get install docker-ce
```

### 无网络安装

> 官方文档：[https://docs.docker.com/engine/installation/binaries/](https://docs.docker.com/engine/installation/binaries/)

1. 下载二进制docker包

下载地址：[https://github.com/docker/docker/releases](https://github.com/docker/docker/releases),选择合适的版本下载tar文件。
2.  解压移动文件

```bash
	$ tar xzvf /path/to/<FILE>.tar.gz
	$ sudo cp docker/* /usr/local/bin/
```
3.  启动dockerd

```bash
	$sudo dockerd &
```

### F&Q

#### 非root用户安装docker后，无法启动直接使用docker命令。

安装完成docker以后，运行docker命令时无法使用，只能使用sudo docker 命令。具体原因和解决方案如下：

Docker守候进程绑定的是一个unix socket，而不是TCP端口。这个套接字默认的属主是root，其他是用户可以使用sudo命令来访问这个套接字文件。因为这个原因，docker服务进程都是以root帐号的身份运行的。
为了避免每次运行docker命令的时候都需要输入sudo，可以创建一个docker用户组，并把相应的用户添加到这个分组里面。当docker进程启动的时候，会设置该套接字可以被docker这个分组的用户读写。这样只要是在docker这个组里面的用户就可以直接执行docker命令了。

>警告：该dockergroup等同于root帐号。

#### 操作步骤：
1. 使用有sudo权限的帐号登录系统。创建docker分组，并将相应的用户添加到这个分组里面。
```bash
	sudo usermod -aG docker your_username
```
2. 退出，然后重新登录，以便让权限生效。确认你可以直接运行docker命令。
```bash
	$ docker run hello-world
```


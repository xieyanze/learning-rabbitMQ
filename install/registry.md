@(learning docker)[docker]

# 镜像仓库

当前版本（2.6）的镜像仓库，官方没有提供历史镜像文件删除方案，当多次push 同一个镜像（tag相同）的时候，会产生大量垃圾文件，暂时没找到好用的解决方案。github上面有一些脚本文件，有空可以去试一下。

[TOC]

## 安装
先pull镜像文件，然后启动容器即可，启动时注意API端口映射。
> 注意：将仓库存储路径挂载到宿主机硬盘，建议使用容量较大数据盘，仓库中的历史镜像删除非常麻烦。

```bash
	$ docker run -d -p 5000:5000 -v /data1/registry:/var/lib/registry --restart=always --name registry registry
```

## 推送镜像
1. 制作镜像

```
	$ docker build
```

2. 制作tag标签，添加namespace
> 备注：如果是本地仓库，namespace = localhost:5000

```bash
	$ docker tag <namespace>/<image_name>:<version> 		
```

3. 推送镜像

```bash
	$ docker push <namespace>/<image_name>
```

>提示：如果push失败并提示:Error getting v2 registry: Get https://xxx:5000/v2/: http: server gave HTTP response to HTTPS client 。
>1.在docker客户端机器创建或修改 /etc/docker/daemon.json
>```bash
>{ "insecure-registries":["myregistry.example.com:5000"] }
>```
>2.重启docker服务

## 删除仓库中的镜像

registry:2.5.0版本的镜像，将镜像默认存放在了/var/lib/registry目录下
/var/lib/registry/docker/registry/v2/repositories/ 目录下会有几个文件夹，命名是已经上传了的镜像的名称。 
如果需要删除已经上传的镜像，现有两种方法

### **官方推荐版**
1. 更改registry容器内/etc/docker/registry/config.yml文件
```
storage:
  delete:
    enabled: true	
```
2. 找出你想要的镜像名称的tag

```bash
$ curl -X GET <protocol>://<registry_host>/v2/<image_name>/tags/list
```

3. 拿到digest_hash参数
>  **注意** 2.4以上版本 如果不加header头，也可以获取到digest_hash，但值是错误的 无法删除。
```bash
$ curl  --header "Accept: application/vnd.docker.distribution.manifest.v2+json" -I -X GET <protocol>://<registry_host>/v2/image_name>/manifests/<tag>
```


4. 复制digest_hash

```
Docker-Content-Digest: <digest_hash>
```

5. 删除清单

```bash
curl -I -X DELETE <protocol>://<registry_host>/v2/<repo_name>/manifests/<digest_hash>
```

6. 执行垃圾回收操作，删除文件系统内的镜像文件，注意2.4版本以上的registry才有此功能

```bash
	$ docker exec -it <registry_container_id> bin/registry garbage-collect <path_to_registry_config
```

### 简易版
1. 打开镜像的存储目录，如有-V操作打开挂载目录也可以，删除镜像文件夹

```bash
	$ docker exec <registry_container_id> rm -rf /var/lib/registry/docker/registry/v2/repositories/<image_name>
```

2. 执行垃圾回收操作。

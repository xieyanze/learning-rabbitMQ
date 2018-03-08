@(learning docker)

镜像导入导出
===

在有些生产环境中，docker宿主机无法访问外网，就需要从外部导入镜像文件。以registry为例，具体步骤如下：

1.  保存镜像
在可以上网的机器上面运行运行
```bash
	docker pull registry
	docker save > /tmp/registry.tar
```
2.  压缩镜像文件
 压缩后，体积大概为原来的三分之一，建议压缩后再传输。
```bash
	tar zcvf /tmp/registry.tar.gz /tmp/registry.tar 
```
3. 加载镜像
将镜像文件复制到docker宿主机后，使用下面命令：
```bash
	tar xvf registry.tar.gz
	docker load < registry.tar
```

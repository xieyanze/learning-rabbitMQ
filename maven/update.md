# 部署

### maven 命令

```bash
mvn clean package docker:build docker:push
```

### 添加service

在docker swarm 中添加service

### 更新

在开发阶段，如果为latest版本，在构建了新的镜像后，因为版本号没改变，可以使用以下方法强制更新

```bash
docker service update --force --image <reigstry host>/<image name>:<version> <service name>	
```

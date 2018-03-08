添加aliyun镜像方法
===
1. 创建docker配置文件目录
```shell
sudo mkdir -p /etc/systemd/system/docker.service.d
```
2. 添加配置文件
```shell
sudo vi /etc/systemd/system/docker.service.d/mirror.conf
```
3. 修改配置添加
```shell
[Service]
ExecStart=
ExecStart=/usr/bin/docker daemon -H fd:// --registry-mirror=https://xxxx
```
>注意:https后面填写aliyun提供的镜像地址
4. 重新加载
```shell
sudo systemctl daemon-reload
```
5. 重启docker服务
```shell
 sudo systemctl restart docker
```

### 自动添加脚本:
```shell
sudo mkdir -p /etc/systemd/system/docker.service.d sudo tee /etc/systemd/system/docker.service.d/mirror.conf <<-'EOF' [Service] ExecStart= ExecStart=/usr/bin/docker daemon -H fd:// --registry-mirror=https://vpilhtt9.mirror.aliyuncs.com EOF sudo systemctl daemon-reload sudo systemctl restart docker
```
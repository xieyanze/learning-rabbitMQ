@(learning docker)[docker]

#Docker Maven

[TOC]

**官方文档**
[https://dmp.fabric8.io/](https://dmp.fabric8.io/)

## 添加插件

```xml
<plugin>
  <groupId>io.fabric8</groupId>
  <artifactId>docker-maven-plugin</artifactId>
  <version>0.20.0</version>

  <configuration>
     <verbose>true</verbose>
     <images>
        <image>
           <name>%g/%a:%l</name>
           <build>
             <from></from>
             <entryPoint>
               <exec>
                 <arg>java</arg>
                 <arg>-jar</arg>
                 <arg>/%{project.artifactId}-${project.version}.jar</arg>
               </exec>
             <entryPoint>
             <assembly>
               <targetDir>/</targetDir>
               <descriptorRef>artifact</descriptorRef>
             </assembly>
           </build>
        </image>
     </images>
  </configuration>
</plugin>
```

## 镜像名称

%g 获取project.groupId 最后一个 . 之后的部分

%a 获取project.artifactId

s%l 获取project.version，如果version以-SNAPSHOT结尾，version为：latest，否则为version值。

例如：

```xml
<groupId>com.service.test</groupId>
<artifactId>test-service</artifactId>
<version>0.1.1-SNAPSHOT</version>
```
生成imageName为：test/test-service:latest

## 关键配置

```xml
<properties>
  <docker.registry>myregistry:5000<docker.registry>
  <docker.host>http://myhost:2375</docker.host>
  <server.port>8080</server.port>
</properties>
```

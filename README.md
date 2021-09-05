# devimages

## 使用方法

1. 构建镜像 & 启动容器

```
DISTRO=ubuntu_focal
docker-compose build --force-rm --no-cache --pull $DISTRO
docker-compose up -d $DISTRO
```

2. 查看容器的IP地址

```
docker inspect --format='{{.Name}} - {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -aq)
```

3. 登录到容器

```
ssh root@172.18.0.2
```

## 已支持的发行版

| 发行版       | 服务名       |
| ------------ | ------------ |
| Ubuntu 20.04 | ubuntu_focal |

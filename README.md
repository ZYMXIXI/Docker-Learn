## Docker学习笔记

|      镜像      |                  注释                  |
| :------------: | :------------------------------------: |
|  hello-docker  |          打印 Hello Docker！           |
|  nginx-custom  | 自定义 Nginx 首页，打印 Docker is run! |
|     ghost      |   Nginx + Ghost + MySQL 开源博客程序   |
| CentOS7.6.1810 |   可用 Systemd 服务的 CentOS7.6.1810   |

### CentOS7.6.1810

> Dockerfile

```dockerfile
FROM centos:7.6.1810
ENV container docker
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;
VOLUME [ "/sys/fs/cgroup" ]
CMD ["/usr/sbin/init"]
```

> Build Command

```bash
docker build --rm -t canvas1996/centos7.6.1810 .
```

> Example Apache Systemd Service To Run

```bash
docker run -it -d --privileged -v /sys/fs/cgroup:/sys/fs/cgroup --name centos7 -p 80:80 centos7.6.1810:latest /usr/sbin/init
docker exec -it centos /bin/bash
```


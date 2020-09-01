# Docker

## Container

### Background Mode

Check all the docker container state
```sh
docker ps
```

Stop certain container 

```sh
docker stop XXX
```

start/restart a stopped container

```sh
docker ps -a
docker start b750bbbcfd88 # b750bbbcfd88 is certain container ID
docker restart b750bbbcfd88 # b750bbbcfd88 is certain container ID
```

Run in background(**-d**)

```sh
docker run -itd XXXX
```

Check log in background mode

```sh
docker logs 2b1b7a428627
docker logs amazing_cori
```



Enter container from background mode.

```sh
docker exec -it 243c32535da7 /bin/bash  
docker attach 1e560fca3906  # Exit in this mode may result in container stop
```

Import/Export container

```sh
docker export 1e560fca3906 > ubuntu.tar
cat docker/ubuntu.tar | docker import - test/ubuntu:v1
docker import http://example.com/exampleimage.tgz example/imagerepo # from URL or Path
```

Delete container

```sh
docker rm -f 1e560fca3906
```

### Interactive Container

```sh
docker run -i -t XXXXXX
```

* -i 允许你对容器内的标准输入 (STDIN) 进行交互

* -t: 在新容器内指定一个伪终端或终端

**Exit container ** *Ctrl+D* or ```exit```

## Image

List all docker images of local 

```sh
docker images
```

Delete docker image

```sh
deocker rmi hello-world
```

Create image

1, modify a exist image, submit the new one.

```sh
docker commit -m="has update" -a="runoob" e218edb10161 runoob/ubuntu:v2
```

* ```-m``` msg
* ```-a``` author
* ```e218edb10161``` 容器 ID
* ```runoob/ubuntu:v2``` image name

2, construct new image 

```sh
$ cat Dockerfile 
FROM    centos:6.7
MAINTAINER      Fisher "fisher@sudops.com"

RUN     /bin/echo 'root:123456' |chpasswd
RUN     useradd runoob
RUN     /bin/echo 'runoob:123456' |chpasswd
RUN     /bin/echo -e "LANG=\"en_US.UTF-8\"" >/etc/default/local
EXPOSE  22
EXPOSE  80
CMD     /usr/sbin/sshd -D
```



```sh
$ docker build -t runoob/centos:6.7 .
Sending build context to Docker daemon 17.92 kB
Step 1 : FROM centos:6.7
 ---&gt; d95b5ca17cc3
Step 2 : MAINTAINER Fisher "fisher@sudops.com"
 ---&gt; Using cache
 ---&gt; 0c92299c6f03
Step 3 : RUN /bin/echo 'root:123456' |chpasswd
 ---&gt; Using cache
 ---&gt; 0397ce2fbd0a
Step 4 : RUN useradd runoob
......
```

- **-t** ：指定要创建的目标镜像名
- **.** ：Dockerfile 文件所在目录，可以指定Dockerfile 的绝对路径

## Ref

[runoob](https://www.runoob.com/docker/docker-tutorial.html)
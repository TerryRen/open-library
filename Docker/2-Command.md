### Info

```bash
sudo docker info
```

### Version

```bash
sudo docker version
```

### Search Images

```bash
sudo docker images
sudo docker images | grep "ubun"
```

### Pull/Push images

```bash
sudo docker pull ubuntu:14.04
docker push docker-hub-test.net/base/ubuntu:16.04
```

### Tag

```bash
sudo docker tag ubuntu:latest ubuntu:16.04
```

### Run Container

```bash
sudo docker run -t -i ubuntu:14.04 /bin/bash
sudo docker run -d -P --name App-1.0 -v /home/temp/user:/host ubuntu:14.04 /bin/bash
sudo docker run --name ffcross --restart=always -d -p 4096:4096 docker-hub-test.net:80/group/ffcross:1.0.8995
```

### Start/Stop/IN Container

```bash
sudo docker start -i App-1.0
sudo docker stop 0f6c09a2d5d8 3e6c09a2d5d8
sudo docker exec -it App-1.0 /bin/bash
```

### Commit Container as Image

```bash
sudo docker commit -m "Init ENV" -a "Terry" 1b2616b0e5a8 group/app:v1.0.0
```

### Container TCP Port

```bash
sudo docker port 1b2616b0e5a8
```

### Docker conf (by Systemd bootstrap)

```bash
more /usr/lib/systemd/system/docker.service.d/http-proxy.conf
```

### Docker conf (by INIT.d bootstrap)

```bash
more /etc/sysconfig/docker
```

### Docker Container PID

```bash
sudo docker inspect -f '{{.State.Pid}}' b3f010b9bb31
```

### Tcp of PID

```bash
nsenter -t 3097 -n netstat -np | grep tcp
nsenter -t 3188 -n netstat -n | awk '/^tcp/ {++state[$NF]} END {for(key in state) print key,"\t",state[key]}'
nsenter -t 3188 -n netstat -np | grep TIME_WAIT | wc -l
```

### Docker registry-mirror

#### 方法一
```bash
docker --registry-mirror=https://registry.docker-cn.com daemon
```

#### 方法二
永久修改
```txt
修改文件 /etc/systemd/system/docker.service
ExecStart=/usr/bin/dockerd --registry-mirror=https://registry.docker-cn.com
```

```bash
systemctl restart docker
```

### Zbox

```bash
mkdir -p /srv/zbox && docker run -d -p 8111:80 \
        --restart=always \
        -e USER="root" -e PASSWD="123456" \
        -e BIND_ADDRESS="false" \
        -e SMTP_HOST="163.177.90.125 smtp.exmail.qq.com" \
        -v /srv/zbox/:/opt/zbox/ \
        --name zentao-server \
        idoop/zentao:latest
```

### Gitlab

https://docs.gitlab.com/omnibus/docker/

```bash
sudo docker run --detach \
    --publish 9443:443 --publish 9080:80 --publish 9022:22 \
    --name gitlab \
    --restart always \
    --volume /srv/gitlab/config:/etc/gitlab \
    --volume /srv/gitlab/logs:/var/log/gitlab \
    --volume /srv/gitlab/data:/var/opt/gitlab \
    gitlab/gitlab-ce:latest
```
修改gitlab.rb文件
external_url 'http://gitlab.example.com:9090'
gitlab_rails['gitlab_shell_ssh_port'] = 9022

### Jenkins

`注意设置host目录权限:1000`

```bash
sudo docker run \
	--name jenkins \
	--restart always -p 9090:8080 -p 50000:50000 \
	-v /srv/jenkins:/var/jenkins_home \
	jenkins/jenkins:lts
```

### Redis

```bash
docker run --name redis -p 6379:6379 -v /srv/redis/data:/data -d redis redis-server --appendonly yes
```


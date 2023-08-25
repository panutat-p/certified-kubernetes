# Docker CLI

```Dockerfile
FROM nginx:alpine
LABEL env=demo
ENV NGINX_PORT=80
EXPOSE 80
```

```shell
docker image build -t nginx:demo .
```

```shell
docker image save -o nginx-demo.tar nginx:demo
```

https://docs.docker.com/engine/reference/commandline/container_run

```shell
docker container run --name nginx-demo -p 80:80 --rm nginx:demo
```

```shell
docker container ls -a
```

```shell
docker container prune
```

```shell
docker volume prune
```

```shell
docker image prune
```

# Pertemuan 6

## Deploying Your First Docker Container

**Step 1 - Running A Container** 

Mencari image redis
```
$ docker search redis
NAME                             DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
redis                            Redis is an open source key-value store that…   7412                [OK]
bitnami/redis                    Bitnami Redis Docker Image                      129                                     [OK]
sameersbn/redis                                                                  77                                      [OK]
grokzen/redis-cluster            Redis cluster 3.0, 3.2, 4.0 & 5.0               61
rediscommander/redis-commander   Alpine image for redis-commander - Redis man…   31                                      [OK]
kubeguide/redis-master           redis-master with "Hello World!"                30
redislabs/redis                  Clustered in-memory database engine compatib…   23
oliver006/redis_exporter          Prometheus Exporter for Redis Metrics. Supp…   18
arm32v7/redis                    Redis is an open source key-value store that…   17
redislabs/redisearch             Redis With the RedisSearch module pre-loaded…   17
webhippie/redis                  Docker images for Redis                         10                                      [OK]
s7anley/redis-sentinel-docker    Redis Sentinel                                  9                                       [OK]
bitnami/redis-sentinel           Bitnami Docker Image for Redis Sentinel         8                                       [OK]
insready/redis-stat              Docker image for the real-time Redis monitor…   8                                       [OK]
redislabs/redisgraph             A graph database module for Redis               8                                       [OK]
arm64v8/redis                    Redis is an open source key-value store that…   6
centos/redis-32-centos7          Redis in-memory data structure store, used a…   4
redislabs/redismod               An automated build of redismod - latest Redi…   4                                       [OK]
circleci/redis                   CircleCI images for Redis                       2                                       [OK]
frodenas/redis                   A Docker Image for Redis                        2                                       [OK]
tiredofit/redis                  Redis Server w/ Zabbix monitoring and S6 Ove…   1                                       [OK]
runnable/redis-stunnel           stunnel to redis provided by linking contain…   1                                       [OK]
wodby/redis                      Redis container image with orchestration        1                                       [OK]
xetamus/redis-resource           forked redis-resource                           0                                       [OK]
cflondonservices/redis           Docker image for running redis                  0 
```
Menjalankan image redis menjadi container
```
$ docker run -d redis
bf469d862c85780cadfd8c25f5b5b6724d3a65cce16f8f9470aeb706276cb8c1
```
**Step 2 - Finding Running Containers**

Melihat container yang sedang berjalan
```
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
bf469d862c85        redis               "docker-entrypoint.s…"   5 minutes ago       Up 5 minutes        6379/tcp            eager_murdock
```

**Step 3 - Accessing Redis**

Menjalankan beberapa container
```
$ docker run -d --name redisHostPort -p 6379:6379 redis:latest
0b13c0df4079e22cdc4ed919bc49b48c80599879ad3a4759dad3d784b026fd92
```
```
$ docker run -d --name redisDynamic -p 6379 redis:latest
64963511ce2abbe056fb04902b19f9295dce3a75dc50f82137c96079af9b8d52
```
```
$ docker port redisDynamic 6379
0.0.0.0:32768
```

Melihat container yang berjalan
```
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS     NAMES
64963511ce2a        redis:latest        "docker-entrypoint.s…"   36 seconds ago      Up 34 seconds       0.0.0.0:32768->6379/tcp   redisDynamic
0b13c0df4079        redis:latest        "docker-entrypoint.s…"   2 minutes ago       Up 2 minutes        0.0.0.0:6379->6379/tcp    redisHostPort
bf469d862c85        redis               "docker-entrypoint.s…"   11 minutes ago      Up 11 minutes       6379/tcp     eager_murdock
```

**Step 5 - Persisting Data**

```
$ docker run -d --name redisMapped -v /opt/docker/data/redis:/data redis
ebac2ced4c2909c83ec5bc964e288cb5f8f0015d48d576a354782fc635234b70
```

**Step 6 - Running A Container In The Foreground**

```
$ docker run -it ubuntu bash
root@c06387f1fd25:/# cat /etc/issue
Ubuntu 18.04.1 LTS \n \l

root@c06387f1fd25:/#
```

## Deploy Static HTML Website as Container
**Step 1 - Create Dockerfile**

Membuat Dockerfile sebagai berikut.
```
FROM nginx:alpine
COPY . /usr/share/nginx/html
```
**Step 2 - Build Docker Image**

Membuat image dari Dockerfile yang kita buat tadi
```
$ docker build -t webserver-image:v1 .
Sending build context to Docker daemon  3.072kB
Step 1/2 : FROM nginx:alpine
 ---> 4d3c246dfef2
Step 2/2 : COPY . /usr/share/nginx/html
 ---> 630f803c8794
Successfully built 630f803c8794
Successfully tagged webserver-image:v1
```

Melihat image yang kita buat.
```
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
webserver-image     v1                  630f803c8794        8 seconds ago       21.2MB
nginx               alpine              4d3c246dfef2        2 weeks ago         21.2MB
ubuntu              latest              16508e5c265d        13 months ago       84.1MB
redis               latest              4e8db158f18d        14 months ago       83.4MB
weaveworks/scope    1.9.1               4b07159e407b        14 months ago       68MB
alpine              latest              11cd0b38bc3c        15 months ago       4.41MB
```

**Step 3 - Run**

Menjalankan image menjadi container
```
$ docker run -d -p 80:80 webserver-image:v1
b6d0ee7120b74416dacfd7a6cfed8c4564d6c9a3d18de9eac2b93081cd91d33f
```
Buka halaman docker dengan curl
```
$ curl docker
<h1>Hello World</h1>
```


## Building Container Images

**Step 1 - Base Images**

Membuat Dockerfile
```
# This is your Editor pane. Write the Dockerfile here and 
# use the command line to execute commands
FROM nginx:1.11-alpine
```

**Step 2 - Running Commands**

Menambahkan command didalam Dockerfile
```
# This is your Editor pane. Write the Dockerfile here and 
# use the command line to execute commands
FROM nginx:1.11-alpine
COPY . /usr/share/nginx/html
```

**Step 3 - Exposing Ports**

Menambahkan port yang dibuka
```
# This is your Editor pane. Write the Dockerfile here and 
# use the command line to execute commands
FROM nginx:1.11-alpine
COPY . /usr/share/nginx/html
EXPOSE 80
```

**Step 4 - Default Commands**

Menambahkan default commands
```
# This is your Editor pane. Write the Dockerfile here and 
# use the command line to execute commands
FROM nginx:1.11-alpine
COPY . /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

**Step 5 - Building Containers**

Membuat image dari Dockerfile
```
$ docker build -t najibun:v1 .
Sending build context to Docker daemon  3.072kB
Step 1/4 : FROM nginx:1.11-alpine
 ---> bedece1f06cc
Step 2/4 : COPY . /usr/share/nginx/html
 ---> 9f5f83e869e1
Step 3/4 : EXPOSE 80
 ---> Running in 4c4ad60a177e
Removing intermediate container 4c4ad60a177e
 ---> 76957e4216b2
Step 4/4 : CMD ["nginx", "-g", "daemon off;"]
 ---> Running in edd27496d2cf
Removing intermediate container edd27496d2cf
 ---> 58599e453cf3
Successfully built 58599e453cf3
Successfully tagged najibun:v1
```

Melihat image yang kita buat
```
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
najibun             v1                  58599e453cf3        10 seconds ago      54.3MB
ubuntu              latest              16508e5c265d        13 months ago       84.1MB
redis               latest              4e8db158f18d        14 months ago       83.4MB
weaveworks/scope    1.9.1               4b07159e407b        14 months ago       68MB
alpine              latest              11cd0b38bc3c        15 months ago       4.41MB
nginx               1.11-alpine         bedece1f06cc        2 years ago         54.3MB
```

**Step 6 - Launching New Image**

Menjalankan container dari image yang kita buat
```
$ docker run -d -p 80:80 najibun:v1
4047a2c5b5109f576a55a404e1fa05f90d930f4349688486932f498db7cc2f3b
```

melihat container yang berjalan.
```
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS         NAMES
4047a2c5b510        najibun:v1          "nginx -g 'daemon of…"   16 seconds ago      Up 15 seconds       0.0.0.0:80->80/tcp, 443/tcp   upbeat_bhabha
```

melihat halaman docker dengan curl
```
$ curl -i http://docker
HTTP/1.1 200 OK
Server: nginx/1.11.13
Date: Tue, 15 Oct 2019 13:57:23 GMT
Content-Type: text/html
Content-Length: 21
Last-Modified: Tue, 15 Oct 2019 13:48:24 GMT
Connection: keep-alive
ETag: "5da5ce28-15"
Accept-Ranges: bytes

<h1>Hello World</h1>
```


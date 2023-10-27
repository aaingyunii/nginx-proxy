# BMT

- hosts
```
# bmt
127.0.0.1	homepage.flyio.local
```

### docker compose 
- server start
- https://docs.docker.com/engine/reference/commandline/compose_up/#options
```bash
# $ docker compose up -d --build --force-recreate
$ docker compose up -d
[+] Running 3/3
 ✔  Network nginx-proxy_default           Created                              0.1s
 ✔  Container nginx-proxy-homepage-aik-1  Started                              0.0s
 ✔  Container nginx-proxy-nginx-proxy-1   Started                              0.0s
```

- **`date;docker stats --no-stream;echo "vuser=0";`**

```bash
$  date;docker stats --no-stream;echo "vuser=0";
Fri Oct 27 17:25:01 KST 2023
CONTAINER ID   NAME                  CPU %     MEM USAGE / LIMIT   MEM %     NET I/O       BLOCK I/O   PIDS
6e83960eeab2   mproxy                0.00%     7.414MiB / 10MiB    74.14%    1.02kB / 0B   0B / 0B     10
2a8c6a91b34e   bmt_lb-homepage_1-1   0.00%     27.01MiB / 30MiB    90.04%    1.32kB / 0B   0B / 0B     82
vuser=0

# 날짜, vuser가 지정한 수 였을 때, docker stats 보여줌
## nGrinder 테스트 과정에서 에러가 발생한 순간이나
### 성능이 안좋은 부분의 stats를 확인하고 그에 맞춰 scale-out/up 등의 처리를 진행할 수 있다.
```


### Performance test

- https://github.com/aaingyunii/nginx-proxy/pull/7

- Result : https://github.com/aaingyunii/nginx-proxy/pull/7#issuecomment-1782313964


### Deploy fly.io

- https://homepage-aaingyunii.fly.dev/

```bash
$ fly auth login

$ flyctl launch

$ tail -n 13  fly.toml
app = "homepage-aaingyunii"
primary_region = "nrt"

[build]
  dockerfile = "Dockerfile"

[http_service]
  internal_port = 80
  force_https = true
  auto_stop_machines = true
  auto_start_machines = true
  min_machines_running = 3
  processes = ["app"]

$ fly deploy

```

### nginx caching

```bash
$ docker compose -f ./manual-proxy-compose.yml up -d --build
[+] Building 0.7s (7/7) FINISHED                                                                                                                                                                                                                              docker:default
 => [load_balancer internal] load build definition from Dockerfile                                                                                                                                                                                                      0.0s
 => => transferring dockerfile: 114B                                                                                                                                                                                                                                    0.0s
 => [load_balancer internal] load .dockerignore                                                                                                                                                                                                                         0.0s
 => => transferring context: 2B                                                                                                                                                                                                                                         0.0s
 => [load_balancer internal] load metadata for docker.io/library/nginx:1.25.3                                                                                                                                                                                           0.7s
 => [load_balancer internal] load build context                                                                                                                                                                                                                         0.0s
 => => transferring context: 34B                                                                                                                                                                                                                                        0.0s
 => [load_balancer 1/2] FROM docker.io/library/nginx:1.25.3@sha256:add4792d930c25dd2abf2ef9ea79de578097a1c175a16ab25814332fe33622de                                                                                                                                     0.0s
 => CACHED [load_balancer 2/2] COPY [default.conf, /etc/nginx/conf.d/]                                                                                                                                                                                                  0.0s
 => [load_balancer] exporting to image                                                                                                                                                                                                                                  0.0s
 => => exporting layers                                                                                                                                                                                                                                                 0.0s
 => => writing image sha256:ddbbce8e85c2bc0daf2011afefb40f0450bc3ed1500a414de604293d44fab13c                                                                                                                                                                            0.0s
 => => naming to docker.io/library/bmt_lb-load_balancer                                                                                                                                                                                                                 0.0s
[+] Running 2/0
 ✔ Container bmt_lb-homepage_1-1  Running                                                                                                                                                                                                                               0.0s
 ✔ Container mproxy               Running                                                                                                                                                                                                                               0.0s

$ docker compose ps
NAME      IMAGE     COMMAND   SERVICE   CREATED   STATUS    PORTS

$ docker compose -f ./manual-proxy-compose.yml ps
NAME                  IMAGE                                     COMMAND                                          SERVICE         CREATED          STATUS          PORTS
bmt_lb-homepage_1-1   pysatellite/dj-twenty-six.github.io:bmt   "httpd-foreground"                               homepage_1      7 minutes ago    Up 3 minutes    80/tcp
mproxy                bmt_lb-load_balancer                      "/docker-entrypoint.sh nginx -g 'daemon off;'"   load_balancer   45 seconds ago   Up 44 seconds   0.0.0.0:80->80/tcp, :::80->80/tcp

$ sudo docker stats --no-stream
CONTAINER ID   NAME                  CPU %     MEM USAGE / LIMIT     MEM %     NET I/O       BLOCK I/O   PIDS
9f4529c2caf2   mproxy                0.00%     7.172MiB / 7.625GiB   0.09%     796B / 0B     0B / 0B     9
644e9dac4e50   bmt_lb-homepage_1-1   0.01%     22.09MiB / 30MiB      73.62%    1.09kB / 0B   0B / 0B     82
```


### Ref
- [nginx-및-nginx-plus를-사용한-caching](https://nginxstore.com/blog/nginx/nginx-%EB%B0%8F-nginx-plus%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%9C-caching/)


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
 ✔ Network dj-twenty-sixgithubio_default       Created                              0.1s
 ✔ Container dj-twenty-sixgithubio-homepage-1  Started                              0.0s
 ✔ Container dj-twenty-sixgithubio-nginx-lb-1  Started                              0.0s
```

### Performance test

- https://github.com/aaingyunii/nginx-proxy/pull/7

- Result : https://github.com/aaingyunii/nginx-proxy/pull/7#issuecomment-1782313964




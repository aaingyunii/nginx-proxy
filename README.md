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

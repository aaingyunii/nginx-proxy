# nginx-proxy


### Performance Test

- https://github.com/aaingyunii/nginx-proxy/pull/6

### `nGrinder` 활용

- https://github.com/naver/ngrinder/wiki/Installation-Guide

```bash
$  pwd
/home/user1/app

$ tree
.
└── ng
    ├── controller
    │   ├── ngrinder-controller-3.5.8.war
    ├── ngrinder-agent
    │   ├── ...
    │   
    └── ngrinder-agent-3.5.8-localhost.tar


$ java -jar ngrinder-controller-3.5.8.war
# nGrinder 서버 활성화

$ ~/app/ng/ngrinder-agent/run_agent.sh
# agent 계정 활성화

```

### `compose.yml` , `docker compose` 활용

- 활성화한 api 웹 페이지에 client 임의로 부여 및 scale up-down, out-in 을 통해
- 잘 통신이 되는 지 확인하면서
- `Perfomance` 의 정도를 테스트.

### docker stats 2 CSV

- docker container resource usage statistics -> csv log

```bash
$ docker compose ps
NAME                        IMAGE                       COMMAND                                           SERVICE       CREATED         STATUS         PORTS
nginx-proxy-aik-api-1       aaingyunii/aai-api:latest   "uvicorn app.main:app --host 0.0.0.0 --port 80"   aik-api       9 seconds ago   Up 7 seconds   80/tcp
nginx-proxy-nginx-proxy-1   nginxproxy/nginx-proxy      "/app/docker-entrypoint.sh forego start -r"       nginx-proxy   9 seconds ago   Up 7 seconds   0.0.0.0:80->80/tcp, :::80->80/tcp

# SAVE CSV LOG
$ sh docker_stats2log.sh nginx-proxy-aik-api-1 > api.log &
[1] 79415
[1]  + 79415 suspended (tty output)  sh docker_stats2log.sh nginx-proxy-aik-api-1 > api.log

# STOP SAVE
$ ps -ef
# 관련 pid 찾은 뒤
$ kill {pid}

```


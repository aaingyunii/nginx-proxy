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

### REF

- https://github.com/dj-twenty-six/auto-reverse-proxy/pull/5
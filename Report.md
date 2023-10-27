# BMT Report

### Basic setting

- nginx-proxy : 

```bash
  nginx-proxy:
    image: nginxproxy/nginx-proxy
    ports:
      - "80:80"
    volumes:
      - /var/run/docker.sock:/tmp/docker.sock:ro
    deploy:
      resources:
        limits:
          cpus: '0.20'
          memory: 77M
        reservations:
          cpus: '0.01'
          memory: 30M
```

- nGrinder Test

```java
@Test
public void test() {
    grinder.sleep(1000) # 1sec
    
    HTTPResponse response = request.GET("http://homepage.flyio.local/", params)

    if (response.statusCode == 301 || response.statusCode == 302) {
        grinder.logger.warn("Warning. The response may not be correct. The response code was {}.", response.statusCode)
    } else {
        assertThat(response.statusCode, is(200))
    }
```

## Test cases

### Test1

#### homepage resource - cpu0.05 / memory 30M

![image](https://github.com/aaingyunii/nginx-proxy/assets/31847834/4bb02ebb-13ee-490f-a762-4a214ccf684a)


![image](https://github.com/aaingyunii/nginx-proxy/assets/31847834/89dbcbe8-374d-4767-95a4-4fbcbbd7372e)


### Test2

#### homepage resource - cpu0.05 / memory 30M
#### scale out = 10

![image](https://github.com/aaingyunii/nginx-proxy/assets/31847834/dc118e93-6917-4883-a397-c53152a7f268)


![image](https://github.com/aaingyunii/nginx-proxy/assets/31847834/328f8260-645b-408a-8d4a-79aa5eb4f32d)


### Test3

#### homepage resource - cpu0.05 / memory 30M
#### scale out = 10


![image](https://github.com/aaingyunii/nginx-proxy/assets/31847834/2090c446-32c3-4660-83fb-2943e82fcc0e)


![image](https://github.com/aaingyunii/nginx-proxy/assets/31847834/c3f1257f-ac6b-4c7e-bd46-620f3b758cf3)


### Test4

- too much, so trim some

#### homepage resource - cpu0.03 / memory 40M
#### scale out = 6

![image](https://github.com/aaingyunii/nginx-proxy/assets/31847834/e8ded71f-bc76-4d6a-833f-57e185134da6)


![image](https://github.com/aaingyunii/nginx-proxy/assets/31847834/94d9973a-e4f8-4a6e-89b6-2a49c85f2655)

## Test5

- too much, so trim some

#### homepage resource - cpu0.03 / memory 30M
#### scale out = 3

![image](https://github.com/aaingyunii/nginx-proxy/assets/31847834/24c2c1d2-2164-4d65-80a1-2e3cad403636)

![image](https://github.com/aaingyunii/nginx-proxy/assets/31847834/d12bea72-1bbb-45ee-b277-9fa7e93cb946)

### Test6

#### homepage resource - cpu0.05 / memory 30M
#### scale out = 3

![image](https://github.com/aaingyunii/nginx-proxy/assets/31847834/32ee1f19-0a3d-4d10-bddd-b4d2edb971f3)


![image](https://github.com/aaingyunii/nginx-proxy/assets/31847834/8a9ac22c-37a0-4c05-ad02-957301fb7108)


### Test7

#### homepage resource - cpu0.04 / memory 30M
#### scale out = 3

![image](https://github.com/aaingyunii/nginx-proxy/assets/31847834/74937737-0ee2-41cf-8e25-5bae83065291)

![image](https://github.com/aaingyunii/nginx-proxy/assets/31847834/1afce261-3a39-4529-8af7-69ef7a982e4b)

![image](https://github.com/aaingyunii/nginx-proxy/assets/31847834/846f48f8-e188-431b-9806-e56625c4f9b9)


## Result

### Test7 case is fit to this BMT

- cpu 0.04 and memory 30M
- scale out 3

![image](https://github.com/aaingyunii/nginx-proxy/assets/31847834/dc73912e-9ffb-4208-a947-73a057b6a061)

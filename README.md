# Docker Compose를 이용한 Wordpress(with SSL) 배포
Wordpress, nginx and certbot on docker-compose

## How To Use
###1. [docker-compose 설치하기](https://docs.docker.com/compose/install/#install-compose)

###2. Clone
```
git clone https://github.com/elefstudio89/wordpress-nginx-certbot.git
```

###3. 설정 변경하기
#### I. `init-letsencrypt.sh`
2개 이상의 도메인을 지원하고 싶으면 예시 처럼 띄어쓰기로 구분하여 넣을 수 있습니다.
```
#8 domains=(example.org www.example.org) # Add your service domains.
 =>
#8 domains=(www.elefstud.io blog.elefstud.io)
```
이메일을 넣어줍니다.
```
#11 email=""  # Adding a valid address is strongly recommended
 =>
#11 email="elefstud.io.89@gmail.com"
``` 
테스트런 플래그를 설정합니다. `1: 과정 테스트, 0: 실제발급` 이며 기본 값은 `1` 입니다. 
```
#12 staging=1 # Set to 1 if you're testing your setup to avoid hitting request limits.
``` 

[인증서 발급 제한](https://letsencrypt.org/docs/rate-limits/) 조건이 있기 때문에 최종적으로
설정이 완료되었을 때, `0`으로 바꾸어 실행하는 것을 권장합니다.    
#### II. `nginx_initial.conf`
최초 certbot을 통해 인증서를 발급 받기위해 사용 되는 nginx config 파일입니다.
실제 운영하려는 서비스에 대한 정보를 포함하지 않고 있습니다. 

2개 이상의 도메인을 지원하고 싶으면 예시 처럼 띄어쓰기로 구분하여 넣을 수 있습니다.
```
#3, #17 server_name example.org;
 =>
#3, #17 server_name www.elefstud.io blog.elefstud.io;
```
인증서 경로 안의 `example.org`는 `init-letsencrypt.sh`의 `domains`에 입력한 값중
**가장 먼저 입력한 도메인**을 입력합니다(예시의 경우 `www.elefstud.io`).
```
#20 ssl_certificate /etc/letsencrypt/live/example.org/fullchain.pem;
#21 ssl_certificate_key /etc/letsencrypt/live/example.org/privkey.pem;
 =>
#20 ssl_certificate /etc/letsencrypt/live/www.elefstud.io/fullchain.pem;
#21 ssl_certificate_key /etc/letsencrypt/live/www.elefstud.io/privkey.pem;
```
#### III. `data/nginx/app.conf`
실제 운영하는 서비스 정보를 담은 nginx config 파일 입니다.

server_name, ssl_sertificate 설정은 **II. `nginx_initial.conf`**와 동일하며,
운영하고자 하는 서비스의 형태에 맞게 설정합니다.

기본으로는 `docker-compose.yml`에 작성된 wordpress에 대한 설정이 담겨있습니다.

### 4. 인증서 발급
`init-letsencrypt.sh` 파일에 `#12 staging=1`로 세팅되어 있는지 확인하고 테스트 실행을 합니다.
```
./init-letsencrypt.sh
```

어떤 에러(빨간색 글씨)도 없이 과정이 끝나야 합니다.
에러 발생시, 에러 문구를 참조하여 설정을 확인해 주세요.

에러 없이 과정이 잘 끝나면 `init-letsencrypt.sh`의 설정을 바꿔 줍니다.
```
#12 staging=0
```
그리고 이제 실제 인증서를 발급합니다.
```
./init-letsencrypt.sh
```
 

### 5. 서버 실행
```
docker-compose up -d
```

## References
1. https://medium.com/@pentacent/nginx-and-lets-encrypt-with-docker-in-less-than-5-minutes-b4b8a60d3a71
2. https://github.com/wmnnd/nginx-certbot (accompanied by 1.)
3. https://hub.docker.com/_/wordpress/

## License
All code in this repository is licensed under the terms of the `MIT License`. For further information please refer to the `LICENSE` file.
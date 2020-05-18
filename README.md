# Wordpress를 SSL인증서와 함께 배포하기
Wordpress with DB, nginx and certbot on docker-compose 

> 

`init-letsencrypt.sh` fetches and ensures the renewal of a Let’s
Encrypt certificate for one or multiple domains in a docker-compose
setup with nginx.
This is useful when you need to set up nginx as a reverse proxy for an
application.

## Installation
1. [docker-compose 설치하기](https://docs.docker.com/compose/install/#install-compose).

2. Clone: `git clone https://github.com/elefstudio89/wordpress-nginx-certbot.git .`

3. 설정 변경하기:
- 도메인 주소와 이메일을 `init-letsencrypt.sh` 파일 안에 입력합니다.
- `data/nginx/app.conf` 파일을 열어 `example.org` 라고 적힌 부분을 위에서 입력한 도메인 주소로 교체해 줍니다.

4. 스크립트를 실행합니다(인증서 발급 과정).

        ./init-letsencrypt.sh

5. 서버를 실행합니다:

        docker-compose up

## References
1. https://medium.com/@pentacent/nginx-and-lets-encrypt-with-docker-in-less-than-5-minutes-b4b8a60d3a71
2. https://github.com/wmnnd/nginx-certbot (accompanied by 1.)
3. https://hub.docker.com/_/wordpress/

## License
All code in this repository is licensed under the terms of the `MIT License`. For further information please refer to the `LICENSE` file.
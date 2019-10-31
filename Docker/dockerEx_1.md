# NGINX (pronouced "endgin-x")
 
 ## **nginx** 도커 이미지로 부터 이미지 만들기   
 
 웹 서버, HTTP 캐시, 로드밸랜서, IMAP, POP3, SMTP, HTTPS, HTTP
 프토토콜의 오픈소스 리버스 프록시 서버(Reverse Proxy Server)이다.
 
 Nginx Docker를 설치하고 접속 시 보여줄 index 파일을 설정 
 
 1. **nginx** Docker image 설치
 ```sh
 ~$ docker pull nginx:latest
 ```
 **nginx** image 설치 여부는 다음과 images 도커 명령어를 사용한다.
 ```sh
 ~$ docker images
 ``` 
2. **nginx** 컨테이너 생성
```sh
~$ docker run --name nginxServer -d -p 8080:80 nginx 
```
* 명령어 설명 
  * --name: 도커 컨테이너 이름 설정 시 사용
  * run nginx: nginx 이미지를 실행 
  * -d: 백그라운드로 실행
  * -p: nginx 컨테이너의 포트와 로컬 포트와의 매핑, 예(-p 8080:80 --> nginx의 80 포트를 외부에 8080 포트번호로 매핑 

컨테이너가 생성된 후 다음의 도커 명령어를 사용혀 컨테이너의 실행 상태를 확인할 수 있다. 
```sh
~$ docker ps 
```
컨테이너가 실행된 후 브라우저를 통해 nginx 서버에 요청 메시지(request message)를 전송하여 응답(response)을 확인하면 된다. 


실행 중인 컨테이너의 명령어를 실행시기 위해서는 도커 명령어 **exec**를 사용한다. nginx 컨테이너의 shell을 실행시는 명령어는 
다음과 같다.
```sh
~$ docker container exec -it <container id> /bin/sh
```
shell 이 실행되면 컨테이너 내의 shell이 super user의 권한을 가지게 되어 다음과 같이 프롬프트가 표시된다.
```sh
# vi welcome.html
```
컨테이너의 shell에서 vi  문서 편집기를 활용하여 welcome.html 파일을 /usr/share/nginx/html 디렉토리에 저장하면 웹 브라우저를 
통해 접근할 수 있다. 

welcome.html 문서를 폅집하기 위해서 vi 문서 편집기가 필요하기 때문에 다음과 같이 설치를 한다.
```sh
~$ apt-get update
~$ apt-get install -y vim
```
nginx image에 vim이 설치된 현재의 상태를 다음 명령어의 구조를를 이용하여 이미지를 만든다.
```sh
~$ docker commit -a"nginx+vim" 41e86466dbd1 mynginx:1.0
```
## Dockerfile 로 부터 image 만들기
nginx 이미지를 기반으로 vim을 설치하여 이미지를 만들 수 있는  Dockerfile은 다음과 같다.
```dockerfile
FROM nginx:latest
MAINTAINER Geun-Hyung Kim <geunkimkorea@gmail.com>

# install vim
RUN agt-get update
RUN apt-get install -y vim

# define working directyr
WORKDIR /etc/ngix

# define default command
# nginx 명령어를 forground 로 실행 
CMD ["nginx" , "-g", "daemon off;"]            

# expose posts
EXPOSE 80
```

* 명령어 설명 
  * WORKDIR: 명령이 실행될 디렉토리르 설정
  * CMD: 컨테이너가 무엇을 하는지 결정하는 최종  단계를 정의하는 명령어
  * EXPOSE: 컨테이너와 연결을 할 포트 번호 
  
Dockerfile을 이용하여 image를 생성하는 docker 명령어는 다음과 같다.
```shell
~$ docker build -f <도커 파일 이름> -t <도커 이미지 이름> .
```
```shell
~$ docker build -f ./Dockerfile.nginx -t nginx2 .
```
Dockefile.nginx로 부터 nginx2 이미지를 생성하는 명령이이다. 이렇게 만들어진 이미지를 이용하여 
컨테이너를 다음과 같이 생성한다.
```shell
~$ docker run --name=nginx2 -d -p 80:80 nginx2
```

## 생성한 이미지를 Dockerhub 에 업로드하기 (이미지 배포하기)

* dockerhub.com 에 이미지를 업로드 하는 방법  
dockerhub에 이미지를 업로드하기 위해서는 이미지의 이름을 dockerhub의 계정 이름을 기준으로 변경이 필요하다. 
이미지 이름을 변경하는 명령어는 **docker image tag** 이다.  아래의 예는 myngix:1.0 이미지를 geunkim/nginx_vim:1.0 
으로 변경하고 nginx2:latest 이미지를 geunkim/nginx_vim:1.1 로 변경하는 것이다.
```sh
docker image tag myngix:1.0 geunkim/nginx_vim:1.0
docker image tag nginx2:latest geunkim/nginx_vim:1.1
```
변경 후, 다음 명령어로 geunkim/ 사용자 계정을 가진 이미지의 목록을 얻을 수 있다. 
```sh
docker image ls geunkim/*
```



  



 
 

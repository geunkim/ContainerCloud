# Docker Compose 
도커 컴포즈는 멀티 컨테이너 도커 기반 애플리케이션을 정의하고 실행시키기 위한 도구로 
YALM(확장자 \*.yml) 파일을 이용하여 사용할 컨테이너 이미지와 어떠한 컨테이너를 어떻게 실행할 것인지를
기술하고 있으며, 도커는 기술한 대로 컨테이너를 순차적을 실행시킨다. 
도커 컴포즈는 여러 개의 컨테이너를 도커 컴포즈를 이용하여 하나로 묶는 개념으로 다소 복잡하고 

## Docker Compose 버전 확인 
도커 컴포즈를 설치되었는지를 확인하는 방법은 아래와 같이 옵션으로 '--version'을 사용하면 된다. 
```shell
~$docker-compose --version
```
## Docker Compose 설치 
도커 컴포즈의 최신 버전에 대한 정보는 'https://github.com/docker/compose/releases' 에서 얻을 수 있다. 2019년 11월 현재 최신 버전은 1.25.0-rc4로 아래의 명령어으로 도커 컴포즈의 최신 버전(1.25.0-rc4)의 실행 파일을 다운로드 받아 '/usr/local/bin/' 디렉토르에  docker-compose 로 저장된다. 
```shell
curl -L https://github.com/docker/compose/releases/download/1.25.0-rc4/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```
파일이 저장된 후 파일을 실행할 수 있도록 아래와 같이 설정을 변경한다.
```shell
chmod +x /usr/local/bin/docker-compose
```
## Docker Composer실행 


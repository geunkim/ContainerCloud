# docker (도커)

복잡한 리눅스 애플리케이션 실행환경을 이미지 형태로 배포하여 컨테이너의 형태로 실행할 수 있도록 하는 플랫폼 
개발, 테스트, 서비스 환경을 하나로 통합하여 효율적으로 관리할 수 있는 장점을 제공

리눅스 커널에서 제공하는 리눅스 컨테이너(LXC: LinuX Container) 기술을 이용

리눅스 컨테이너는 가상화가 아닌 __격리__

<image src=https://www.docker.com/sites/default/files/d8/styles/large/public/2018-11/container-what-is-container.png>

## 도커의 특징 

* 도커는 게스트 OS를 설치하지 않음
  * 이미지에 애플리케이션 운영을 위한 프로그램과 라이브러리만 격리해서 설치
  * 이미지 용량이 크게 줄어듬
  * 호스트와 운영체제 자원(시스템 콜) 공유

* 도커는 하뒈어어 가상화 계층이 없음
  * 메모리 접근, 파일시스템, 네트워크 전송 속도가 가상 미선에 비해 월등히(?) 빠름
  * 호스트와 도커 건테이너 사이의 성능 차이가 크지 않음 (오차 범위 내)

* 도커는 이미지 생성과 배포에 특화됨

* 도커 이미지 버전 관리를 위해 중앙 저장소에 이미지를 올리고 받을 수 있음 (Push/Pull)

* 개발한 도커 이미지를 공유는 GitHub와 비스하게 Docker Hub를 제공

* 다양한 API를 제공하여 원하는 만큼 자동화가 개발과 서버 운영에 매우 유용

## 도커 이미지와 컨테이너 

### 도커 이미지
* 서비스 운영에 필요한 서버프르그램, 소스코드, 컴파일된 실행파일을 묶은 형태
* 저장소에 올리고 받는 대상
* 이미지 처리방식: 유니온 파일 시스템 (aufs, btrfs, deicemapper)
  * 도커는 베이스 이미지에서 바뀐 부분만 이미지로 생성
  * 컨테이너로 실행할 때는 베이스 이미지와 바뀐 부분을 합쳐 실행
  * 각 이미지는 의존 관계 형성

### 도커 컨터이너 구조
도커 이미지는 파일시스템들의 계층(layer)로 이루어졌다. 베이스 계층(base layer)은 일반적인 리눅스/유닉스 부트 파일 시스템(boot filesystem)과 유사한 bootfs(bt filesystem)이다. 도커 사용자는 bootfs를 직접 사용할 일은 없고 컨테이너가 실제 부팅되면 메모리로 옮겨지고 initrd 디스크 이미지에 의해 사용된 메모리 공간을 해지하기 위해 bootfs는 언마운트(unmount) 된다. 이것은 리눅스 가상화 스텍과 매우 유사하다. bootfs 위의 도커 다음 계층은 rootfs(root filesystem)이 있다. 이 rootfs는 하나 이상의 운영체제(예: 데비안, 우분투 파일시스템) 일 수 있다. 

전통적인 리눅스 부트에서 루트 파일 시스템은 읽기 전용(ready-only)으로 마운트되고 부트된 후 읽기쓰기(read-write)를 전환되고 유효성 검증을 수행한다. 그러나 도커 생태계에서는 rootfs는 읽기 전용 모드를 유지하고 도커는 여러 읽기 전용 파일 시스템을 rootfs에 추가하기 위해서 union mount를 이용한다. 

union mount는 여러 파일시스템이 한번에 마운트되도록 하는 것으로 하나의 파일시스템으로 보여진다. union mount는 파일시스템을 다른 파일시스템위에 오버레이하여 결과적인 파일시스템이 하부 파일시스템의 일부 또는 전체의 서브 디렉터리와 파일을  

도커는 파일시스템 각각을 이미지라 한다. 

### 컨테이너
이미지를 실행한 상태
하나의 이미지로 여러 개의 컨테이너를 만들 수 있음 

## 도커 명령어

### 도커 이미지 만드는 명령어

<image src=https://subicura.com/assets/article_images/2017-02-10-docker-guide-for-beginners-create-image-and-deploy/create-image.png>
 
* commit: 도커 컨테이너의 변경사항으로 이미지로 생성하는 명령어, commit 명령어의 사용 형식은 다음과 같다. 
```shell
$docker commit <current container name> <new image name>:<tag>
```
commit을 사용한 예는 다음과 같으며 이는 ubuntu 라는 이름을 가지는 base 이미지에 git 기능을 추가한 상태를 

ubuntu:git 이라는 이미지로 생성하기 위한 명령어임
```shell
$docker commit ubuntu ubuntu:git 
```

## 도커 파일 (Dockerfile)
도커 이미지 생성과정을 기술한 Docker 전용 DSL(Domain Specific Language) 파일

<image src=https://subicura.com/assets/article_images/2017-02-10-docker-guide-for-beginners-create-image-and-deploy/dockerfile.png>

* Ubuntu 18.04에 git 소프트웨어어를 설치하는 Dockfile의 내용
apt-get의 -y 옵션은 설치 중 인터렉티브 질문을 제거가는 것을 의미  

```Dockerflie
FROM ubuntu:18.04

RUN apt-get update
RUN apt-get install -y git
```

* Dockfile을 저장한 후 터미널 창에서 아래의 명령을 실행하면 됨 

```shell
$docker build -t ubuntu:git02 .
```
### Dockerfile 명령어
* FROM: 어떤 이미지를 기반으로 새로운 이미지를 생성할 베이스 이미지를 섦정, FROM은 반드시 설정되어 하며 맨 처음에 설정되어야 함. Dockerfile 파일에는 여러 개의 FROM을 설정할 수 있다. FROM이 두 개 설전되었다면 이
미지가 두 개 생성됨, --tag 옵션으로 이미지 이름을 설정하면 맨 마지막 FROM에 적용됨

형식: FROM <이미지> 또는 FROM <이미지>:<태그>
예) FROM ubuntu:18.04

* MAINTAINER: 이미지를 생성한 사람의 정보를 설정, 보통 이름과 이메일을 입력
형식: MAINTAINER <이름><e-mail> 
예) MAINTAINER Geun-Hyung Kim<geunkimkorea@gmail.com> 

* ADD: 파일을 이미지에 추가 - 현재 디렉토리에 있는 파일만 추, ADD로 추가되는 파일은 UID 0, GID 0으로 설정되고 권한은 기존 파일의 권한을 따름, URL로 추가하면 권한은 600ㅇ으로 설정
형식: ADD <추가할 파일> <이미지에서 파일이 추가될 경로>
예) ADD data.txt /tmp/data.txt
ADD http://example.com/hello.txt /home/hello/ ; /home/hello/hello.txt에 파일이 복사

* RUN: FROM으로 설정한 이미지 위에서 스크립트 또는 명령을 실행되며 실행 결과가 새 이미지로 생성되고 실행내역은 이미지의 히스토리에 기록 
형식: RUN <명령어>
예) RUN agt-get install -y git, RUN echo "Hello World" 

* WORKDIR: 작업디렉터리 변경, RUN, CMD, ENTRYPOINT에서 설정한 실행파일이 실행될 디렉터리 
형식: WORKDIR <디럭터리>
예) WORKDIR /path/to/workdir

* ENV: 환경변수 기본값 지정, ENV로 설정된 변수는 RUN, CMD, ENTRYPOINT에 적용, 환경 변수를 사용할 때 $를 사용
형식: ENV <환경변수> <값>
예) ENV PATH ./bin:$PATH, ENV AWESOM_VAR FOOBAR

* EXPOSE: 컨테이너로 생행 시 호스트와 연결할 포트번호를 설정, docker run 명령의 --expose 옵션과 동일, EXPOSE 하나로 두 개 이상의 포트 번호를 동시에 설정
EXPOSE는 호스트와 연결만 할 뿐 외부에 노출은 되지 않음, 포트를 외부에 노출하력면 docker run 명령의 -p, -P 옵션을 사용 
형식: EXPOSE <포트번호>
예) EXPOSE 3000

* CMD: 컨테이너가 시작되었을 때 스크립트 혹은 명령을 실행, docker run 명령으로 컨테이너를 생성하거나 docker start 명령으로 정지된 컨테이너를 시작할 때 실행, CMD는 Dockerfile에서 한번만 사용, 셸 스크립트 구문을 사용할 수 있으며 FROM으로 설정한 이미지에 포함된 /bin/sh 실행파일을 사용
형식: CMD <명령>
예) CMD touch ~/hello.txt

* COPY: 

* USER:

* ONBUILD:

* VOLUME:

* ENTRYPOINT:

### 도커 파일로 이미지 만들기

``` dockerfile
$docker build -t <ID> <이미지 이름> .
```
### 도커 이미지 이름 변경하기 



### 도커 허브(Docker Hub)에 이미지 배포하기 
도커허브: hub.docker.com


### 도커 이미지 자동 배포 
#### CI/CD
* CI: Continuous Integration
* CD: continuous delivery

### 도커를 자동



## 도커의 성능 
### 도커 1.1.2에서 우분투 14.04 호스트와 우분투 14.04 컨테이너 성능 측정


## 도커 컨테이너에 아파치 웹 서버 설정

Jenkins: 빌드, 테스트, 코드분석, 배포, 열람등 다양한 기능 제공

## 도커 사용 명령어

* 도커 엔진에 다룬로드 되어 있는 이미지와 실행된(?) 컨터이너 목록 구하는 방법
```shell
~$ docker system df 
```

* 도커 엔진에서 사용하지 않는 모든 데이터 제거하는 방법
```shell
~$ docker system prune
```









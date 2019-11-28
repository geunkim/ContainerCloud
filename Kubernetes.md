# Kubernetes(쿠버네티스) - (k8s)

쿠버네티스는 컨테이너 오캐스트레이션 분야에서 사실 표준이며 이 외에 도커 스웜(Docker Swarm), 아파치 
메소스(Apache Mesos) 등의 컨테이너 오케스트레이션 솔루션이 있다.
 
* 구글이 오랫동안 컨테이너를 운영하면서 얻은 노하우를 담은 오픈 소스 소프트웨어
* 2017년 10월에 열린 DockerCon EU 2017에서 도커와 쿠버네티스의 통합이 정식 발표됨
* 구글 외부로부터도 많은 컨트리뷰션을 받아들일 수 있는 체제 (CNCF)로 프로젝트가 운영 중
* 진입장벽이 높고 설정 파일이 많음

## kubectl

K8 클러스터의 명령어를 실행시키기 위한 명령어 인터페이스(CLI: Command Line Interface) 

## kubelet

클러스터 내 K8 노드 상에서 실행되는 서비스 에이전트로 컨테이너를 관리하고 컨테이너를 실행시키고 주요 기능은 타스크를 실행하고 명령어를 수신하는 마스터 노드와 통신 기능이다.  

## What is Minikube? (MiniKube는 무엇인가?)

 Minikube is a tool that makes it easy to run Kubernetes locally.

Minikube는 기본적으로 호스트 상의 가상머신(VM: Virtual Machine)상에서 쿠버네티스(Kuberntes) 클러스터 (마스터 노드(Master node)와 워커 노드(Worker node)로 구성됨)를 함께 구성하는 방법으로 개발을 목적으로 하나의 로컬 쿠버네티스 클러스터를 하나의 노드에서 실행할 수 있게 한다. Minikube는 VM이 아닌 호스트에서도 실행이 가능하다. Minikube는 Linux, macOS, Windows 시스템에서 실행가능하다. 즉 Minikube는 물리 서버 또는 VM을 구축할 
필요 없이 쿠버네티스 클러스터 환경을 구성해주는 툴이다. 
이는 주로 교육/훈력을 목적으로 k8을 쉽게 사용할 수 있는 단일 노드 클러스터로서 개발 환경이다. 

![Kubernetes features](https://qph.fs.quoracdn.net/main-qimg-6090cfa54f58441223588c9e0bb5a33b-c)
(source:https://www.quora.com/What-are-some-benefits-in-using-Kubernetes)

## 클러스터

쿠버네티스 클러스터는 쿠버 네티스의 여러 자원을 관리하기 위한 집합체이다. 쿠버네티스 자원 중 가장 넓은 개념이 **노드(node)** 이다. 
노드는 쿠버네티스 클라스터 관리 대상으로 등록된 도커 호스트로 컨터에너가 배치되는 대상이다. 쿠버네티스 클러스터 전체를 관리하는 노드인
**마스터 노드(master node)** 가 하나 이상 있어야 한다. 
쿠버네티스는 노드의 자원 사용 현황 및 배치 전략을 바탕으로 컨테이널르 적절히 배치한다. 즉 클러스터에 배치된 노드의 수, 노드의 사양 등에 따라 
배치할 수 있는 컨테이너 수가 결정된다. 클러스터 처리 능력은 노드에 의해 결정한다. 
```kubectl get nodes``` 명령으로 현재 클러스터에 포함된 노드의 목록을 확인할 수 있다.



## 디플로이먼트 (deployment)

## 리플리케이션컨트롤러 

## 서비스 (service)

## 쿠버네티스 애드온 (Addons)
애드온은 클러스터 내부에서 필요한 기능을 위해 실행되는 Pod들이다. 애드온에 사용되는 Pod들은 디플로이먼트,
리플리케이션컨트롤러 등에 의해 관리된다. 에드온이 사용되는 네임스페이스는 kube-system이고 다음과 같은 애드온이
제공된다. 

### 네트워킹 애드온
쿠버네티스는 클러스터 내부에서 가상 네트워크를 구성하여 사용한다. 이 때 kube-proxy 외에 네트워킹 관련 애드온을 
사용한다. AWS, Azure, Google cloud 플랫폼은 네트워킹 관련 기능이 구현되어 있기 떄문에 
상욜 클라우드 플랫폼에서 쿠버네티스를 사용할 경우 별도의 네트워킹 애드온을 사용하지 않아도 된다.
쿠버네티스를 직접 보유한 서버에 설치하여 구성을 하기 위해서 네트워킹 관련 애드온을 설치하여 사용해야 한다. 
네트워킹 애드온의 종류는 10개가 넘을 정도로 다양한다. ACI, Calico, Canal, Cilium, CNI-Genie, Contiv,
Falannel, Multus, NSX-T, Nuage, Romana, Weave Net 등이 있다.

### DNS 애드온
DNS 애드온은 실제 클러스터 내에서 작동되는 DNS 서버이다. 쿠버네티스 서비스에 DNS 레코드를 제공하는 역할을 한다. 
쿠버네티스 내부에서 컨테이너들은 자동으로 DNS 서버에 등록된다. DNS 서비스로는 kube-dns와 core-dns가 있다. 

### 대시보드(DashBoard) 애드온 
대시보드는 웹 UI로 클러스터 현황을 한눈에 쉽게 파악할 수 있도록 한다. 

## 저장 장소
minikube 클러스터를 생성하면 standard라는 이름의 기본 storageClass가 생성된다. 

### 클러스터 생성 
```shell
~$minikube start
```
이전에 실행한 적이 있다면 다음의 에러가 발생한다.
```mhaine does not exst```

 앞의 에러가 발생하면 다음과 같이 클러스터를 삭제하고 다시 생성해 준다.
 ```shell
 ~$minikube delete
 ```





## 예제  (Todo 애플리케이션 구축하기)
### 1. Minikube를 이용하여 쿠버네티스 클러스터를 생성하고 ToDo 애플리케이션 구축 

```shell ~$minikube start ```

### 2. DB 서비스 (master, slave) 실행 
```shell
~$kubectl apply -f mysql-master.yaml
~$kubectl apply -f mysql-slave.yaml
```


### Reference

1. [쿠버네티스 홈페이지](https://kubernetes.io/ko/docs/home/)
2. [도커/쿠버네티스를 활용한 컨테이너 개발 실전 입문 예제 링크](http://wikibook.co.kr/docker-kubernetes/)

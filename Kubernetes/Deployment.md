# 디플로이먼트(Deployment)

디풀로이먼트는 컨테이너화된 애플리케이션을 배포하기 위해 애플리케이션 인스턴스를 생성/갱신하는 방법을 설정한 것이다. 디플로이먼트가 애플리케이션 배포의 기본 단위가
되는 자원이다. 컨테이너화된 애플리케이션을 배포하기 위해서 쿠버네티스 **디플로이먼트** 설정을 만들어야 한다. 디플로이먼트가 만들어지면 쿠버네티스 마스터가 해당 애플리케이션 인스턴스를 클러스터의 개별노드에 스케줄한다. 

쿠버네티스는 디플로이먼트를 단위로 애플리케이션을 배포한다. 실제 운영에서는 리플리케이션셋을 직접 다루기 보다 디폴로이면트 매니패스트 파일을 통해 다루는 것이 보통이다. 
디폴로이먼트가 관리하는 리프리카셋은 지정된 포드 수를 생성하거나 포드를 새로운 버전으로 교체하거나 이전 버전으로 롤백하는 등 중요한 역할을 한다. 애플리케이션 배포를 잘 운영하려면 리플리카셋이 어떻게 동작하는지 파악할 수 있어야 한다. 디폴로이먼트를 수정하면 리플리카셋가 새로 생성되고 기존 리플리카셋과 교체된다. 

디플로이먼트에에는 포드 템플릿 레이블과 적절한 셀렉터를 반드시 명시해야 한다. 셀렉터는 다른 컨트롤러와 겹치지 않아야 한다. 쿠버네티스는 겹치는 것을 막지 않아 다중 컨트롤러가 겹치는 셀렉터를 가지는 경우 해당 컨트롤러의 충돌 또는 예기치 않은 동작을 야기할 수 있음  


## 디폴로이먼트 생성하기
```shell
~$
```

ㅍ

```YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.15.11
        ports:
        - containerPort: 80
```

* **apiVersion**: 연결하길 원하는 API 서버의 API 종단점(endpoint)을 규정하는 것으로 정의한 오브젝트 유형(object type)의 존재하는 버전과 일치해야 함
* **kind**: 오브젝트 유형을 명시, 위의 예에서는 **Deployment** 이고 Pod, Replicaset, Namespace, Service 등이 존재 
* **metadata**: 이름, 레이블, 네임스페이스등과 같은 오브잭트의 기본 정보 포함
* **metadata.name**: 디폴로이먼트 이름을 규정
* **spec**: 오브젝트의 원하는 상태를 정의하는 블록의 시작점을 표시. 앞의 예의 경우 세 개의 포드(Pod) 실행을 요구, 세 개의 포드는 **spec.template** 에
정의된 Pod Template를 사용하여 생성됨, 디플로이먼트의 일부로서 Pod와 같이 중첩된 오브젝트는 ```metadata```와 ```spec```은 유지하고 ```apiVersion```과
```kind```는 잃게되고 템블릿으로 대치된다. 
* **spec.template.spec**: Pod의 원하는 상태를 정의, 앞의 예에는 ```nginx:1.15.11``` 이미지의 컨테이너를 생성
* **replicas**: 복제할 포드의 개수를 규정 
* **selector**: 디플로이먼트가 관리하는 포드를 찾는 방법을 정의, 포드 템플릿에 정의된 레이블 (app: nginx)을 선택 
* **selector.matchLabels**: (key, value)는 ```matchExpressions``` 의 요소에 해당 

디블로이먼트 오브젝트 (Deployment ojbjet)가 생성되면 쿠버네티스 시스템은 오브젝트에 **status*** 필드를 추가한다. 

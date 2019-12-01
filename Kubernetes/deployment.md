# deployment (디플로이먼트)



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
* **spec**: 오브젝트의 원하는 상태를 정의하는 블록의 시작점을 표시. 앞의 예의 경우 세 개의 포드(Pod) 실행을 요구, 세 개의 포드는 **spec.template** 에
정의된 Pod Template를 사용하여 생성됨, 디플로이먼트의 일부로서 Pod와 같이 중첩된 오브젝트는 ```metadata```와 ```spec```은 유지하고 ```apiVersion```과
```kind```는 잃게되고 템블릿으로 대치된다. 
* **spec.template.spec**: Pod의 원하는 상태를 정의, 앞의 예에는 ```nginx:1.15.11``` 이미지의 컨테이너를 생성

디블로이먼트 오브젝트 (Deployment ojbjet)가 생성되면 쿠버네티스 시스템은 오브젝트에 **status*** 필드를 

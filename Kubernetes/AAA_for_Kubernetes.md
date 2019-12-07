# AAA for Kubernetes

* Authentication(인증): 
* Authorization(허가):
* Admission Control (접근 제어): 

## Authentication (인증)

쿠버네티스는 두 가지 유형의 사용자를 가진다.

* Normal Users (일반 사용자)
일반 사용자들은 User/Client Certificates (사용자/클라이언트 인증), usernames/passwords 목록 파일, 구글 어카운트와 같은 
독립된 서비스를 통해 쿠버네티스 클러스터 외부에서 관리된다. 

* Service Accounts (서비스 계정)
서비스 계정 사용자를 가진 클러스터 내부 프로세스들은 다른 오퍼레이션을 실행하기 위해서 API 서버와 통신을 한다. 대부분의 
서비스 계정 사용자는 API 서버에 의해서 자동으로 생성되고 수동적으로 생성될 수 있다. 서비스 계청 사용자는 네임스페이스와 결합되고
API 서버와 보안이 보장된 통신을 하기 위해서 개별적인 인증서를 마운트한다.

쿠버네티스는 일반 사용자와 서비스 계정의 요청과 함께 익명의 요청을 지원할 수 있다. 
관리자가 권한정책 관련 문제를 해결할 때 유용한 특징인 사용자가 다른 사용자처럼 활동할 수 있도록 하는
user impersonation을 지원한다. 

쿠버네티스는 인증을 위해 다음과 같은 여러 인증 모듈을 사용한다. 

* **Client Centificates**: 클라이언트의 신원을 인증하기 위해, ```--client-ca-file=SOMEFILE``` 옵션의 형태로 
하나 이상의 CA(Certicate Autority)의 정보를 저장하는 파일을 참조할 필요가 있다. 파일에 명시된 CA는 API 서버에 제출된
client certificate을 검증한다. 

* **Static Token File**: ```--token-auth-file=SOMEFILE```로 이미 정의된 베어러 토큰들을 포함한 파일을 API 서버로 전달할 수 있다.
이 토큰은 API 서버를 재 시작하지 않으면 변경되지 않는다. 

* **Bootstrap Tokens**: 새로운 쿠버네티스 클러스터를 부트스트래핑에 사용되는 것으로 아직 베타 상태이다.

* **Static Password File**: Static Token File과 같이 ```--basic-auth-file=SOMEFILE``` 옵션으로 기본 인증에 대한 상새한 내용을
포함한 파일을 API 서버로 전달한다. API 서버를 재시작하기 전 까지 패스워드는 변경되지 않는다.

* **Servic Account Tokens**: 요청을 검증하기 위해 사인한 베어러 토큰을 사용하는 자동적으로 활성화된 인증자이다.
이 토큰들은 클러스터 내 프로세스들이 API 서버와 통신하게 하는 ServiceAccount Admission Controller를 통해서 포드에 연결된다. 

* **OpenID Connect Tokens**: OpenID 커넥트는 인증 업무의 부하를 줄이기 위해  
Azure Active Directory, Salesforce, Google 등과 같은 외부의 OAuth2 제공자에 연결할 수 있도록 한다. 

* **Webhook Token Authentication**: Webhook 기반 인증으로 베어러 토큰의 검증을 외부의 리모트 서비스를 통해 행할 수 있다. 

* **Authenticating Proxy**: 부가적인 인증 로직을 프로그램하길르 원할 때  authenticating proxy를 사용할 수 있다. 

## Authorization (허가)

성공적으로 인증이 이루어진 사용자는 다른 오퍼레이션들을 실행하기 위해서 API서버로 API 요청을 보낼 수 있다. 이들 API 요청은 다양한 
허가 모듈을 사용하여 쿠버네티스의 의해 허가를 얻는다. 쿠버네티스가 라뷰하는 API 요청의 속성들은 정책에 의해 평가된다. 평가결과가 성공적이면 
요청은 받아들여진다. 인증 모듈과 같이 허가도 여러 형태의 모듈과 허가자가 있다.
다음이 허가 모듈이 있다.

* **Node Autorizer**:
* **Attributed-Based Access Control (ABAC) Authorizer**: 
* **Webhook Authroizer**:
* **Role-Based Access Control (RBAC) Authorizer**:
* **Role**:
* **ClusterRole**:

## Admission Control (접근 제어)



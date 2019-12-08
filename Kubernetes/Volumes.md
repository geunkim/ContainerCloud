# Volume (볼륨)

포드에서 실행 중인 컨테이너는 임시적이기 떄문에 컨테이너가 중지되면 컨테이너 내 저장된 모든 데이터는 삭제된다. 
그러나 ```kubelet```은 깨끗한 슬레이트로 컨테이너를 재시작하며 이전 데이터는 가지지 않을 것이다. 

이 문제를 해결하기 위해서 쿠버네티스는 볼륨을 사용한다. 볼륨은 기본적으로 저장 매채가 지원하는 디렉토리이다. 
저장 매체, 컨텐츠 및 액세스 모드는 볼륨 유형에 따라 결전된다.
쿠버네티스의 볼륨은 포드에 연결되고 포드의 컨테이너 간에 공유될 수 있으며 컨테이너를 다시 시작해도 데이터를 보존할 수
있도록 한다. 
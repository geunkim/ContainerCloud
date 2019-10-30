# YAML(YAML Ain't Markup Language)

설정 파일을 기술하는 것을 목적으로 사용되고 있다. 

## 기본 구조 
기본적으로 XMLx과 같이 트리구조를 가지며 부모 자식의 구분은 들여쓰기로 한다. 들여쓰기에 <tab> 키를 인식을 하지 못하기 때문에 <Space>
키를 사용하여 구분한다.

* YAML의 기본 자료형 
  * 스칼라(scalar): 스트링 또는 숫자
  * 시퀀스(sequence): 배열 또는 리스트
  * 매핑(mapping): 해시 또는 딕셔너리, <키(key), 값(value)> 형태
  * 매핑 시 키와 값은 콜론(:) 으로 구분한다. 
  * 시퀀스는 들여쓰기와 -를 통해 표현한다 
  * 주석은 #으로 시작한다
  * 하나의 스트림으로 여러개의 문서를 표현할 때는 하이폰 세 개(---)로 나누고 마침표 세 개(...)는 스트림의 끝을 나타낸다. 
  * 반복되는 값은 &을 통해서 alias를 설정한다.
  
## 스칼리, 시퀀스, 매핑 예제

```YAML
#  스칼라의 시퀀스
- Mark McGwire
- Sammy Sosa
- Ken Griffey

# 스칼라와 스칼라의 매핑 
hr: 65
avg: 0.278
rbi: 147

# 스칼라와 시퀀스의 매핑
american:
  - Boaston Red Sox
  - Detroit Tigers
  - New York Yankees
national:
  - New York Mets
  - Chicago Cubs
  - Atlanta Braves

# 매핑의 시퀀스
- 
  name: Mark McGwire
  hr: 65
  avg: 0.278
  
-
  name: Sammy Sosa
  hr: 63
  avg: 0.288
```

```YAML
parent:
  child-1: first child
  child-2:
     grandchild-1: first grand child
     grandchild-2: secnod grand child
 ```
같은 부모를 가지는 자식 노드들은 들여쓰기가 정확히 일치해야 한다. 틀리면 예러다. 

## 노드
기본적으로 하나의 노드는 key:value의 쌍으로 이루어진다. value에 해당하는 항목에는 3가지고 있다. 

* POD 타입 비슷한 스칼라(scalar): 문자열, 정수, 실수, 날짜 등 여러 가지 자료형이 사용가능하다. 
```YAML
key: value
```
* 시퀀스로 이루어진 자식노드들
```YAML
key: 
  - child: 1
  name: tom
  - child: 2
  name: john
```

* 매핑으로 이루어진 자식 노드들, 각 노드 간의 구분은 문자열로 구성되며 순서는 보장되지 않는다. 
```YAML
key:
  child-1: tom
  child-2: john
```
자식 노드를 가지는 노드는 스칼라 값을 가질 수 없다.

```YAML
version: '1'
services:
  mdb:
    image: mariadb:latest
    envirinment:
      MYSQL_ROOT_PASSWORD: 1234
  web:
    image: apache_df:web
    port:
      - "80:80"
    links: 
```
## 앵커(Anchor)와 알리아스(Alias) 

 ```YAML
 anchors:
    first-anchor: &first <-- 앵커 선언.
        Name: tom
        Birth: 1977.4.21
    second-anchor: &second <-- 앵커 선언.
        Name: john
        Birth: 1979.7.15

first-child  : *first <-- 앵커 알리아스.
second-child : *second <-- 앵커 알리아스.
```

[YAML 유효성 검증 사이트](http://www.yamllint.com/) 

## YAML 

### 주석: #으로 시작

문서의 시작 (선택 사항): ---
문서의 끝 (선택 사항): ...

### 기본 자료형 
* Scalar: 숫자 또는 String
* Sequence: array 또는 list
* Mapping: Hash, Dictionary, key-value pair

### 들여쓰기
* Tab 대신 ``space bar``을 사용 (기본적으로 2칸 또는 4칸을 지원)

### 데이터 정의 (map)
* 기본적으로 데이터는 ```key: value``` 형식으로 정의 
* ``:`` 다음에는 공백 문자가 있어야 한다.

에 
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: echo
  labels:
    type: app
```

### object 표현

```yaml
object:
  key1: value
  key2: value

# 또는

object {
  key1: velue,
  key2: value
}
```

### 배열 표현

배일은 ``-``으로 표현한다.

```yaml
key:
- item1
- item2

# 또는

key: [
  item1, item2
]
```

### 참/거짓 

- 참/거짓은 ``true``, ``false`` 외에 ``yes``, ``no``를 지원한다.

```yaml
study_hard: yes
give_up: no
hello: True
world: TRUE
manual: false
```

- 숫자: 정수 또는 실수를 따움표(") 없이 사용하면 숫자로 인식한다.

```yaml
# number
version: 1.2

# string
version: "1.2"
```

### 주의사항

#### 띄어쓰기
key와 value 사이에는 반드시 공백이 필요하다.

```yaml
key:value #error

key: value # ok
```

### json2yaml

[JSON을 YALM로 변환해 주는 사이트](https://www.json2yaml.com/)

## yamllint

YAML 문법을 체크하고 어뗗게 해석하는지 결과를 보여준다.

[https://www.yamllint.com/](https://www.yamllint.com/)



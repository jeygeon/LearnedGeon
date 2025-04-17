  
---  
## 📚 인덱스(Index)란?<br>  
`인덱스`는 oracle이나 mysql 같이 관계형 데이터베이스의 **테이블과 비슷한 개념**이다. 인덱스를 설계하는 것이 Elasticsearch를 사용하기 위한 첫 단계이다.  
  
### 🔍 정의 <br>  
> 인덱스는 유사한 구조를 가진 문서들의 모음이며,  
Elasticsearch에서 데이터를 저장하고 검색하는 논리적인 단위이다.  
  
  
예를 들어  
* user_index → 사용자 정보 문서들의 모음  
* blog_index → 블로그 글 문서들의 집합  
* log_index → 시스템 로그 데이터 문서들의 집합  
  
### 💡 특징<br>  
하나의 인덱스는 수 많은 문서를 가질 수 있으며, 인덱스는 내부적으로 `샤드(shard)`라는 단위로 나뉘어져 저장된다.  
  
---  
## 🧩 샤드(Shard)란?<br>  
`샤드(shard)`는 인덱스를 구성하는 물리적인 저장 단위이다. 즉 하나의 인덱스가 작은 샤드 조각들도 나뉘어져 있는 것이다. 실제 문서는 샤드에 색인되고 검색된다.  
* 하나의 인덱스는 한 개 이상의 샤드로 구성된다.  
* 각 샤드는 독립적인 인스턴스이며, 자체적으로 색인과 검색 기능을 수행한다.  
* Elasticsearch는 이러한 샤드들을 여러 노드에 분산시켜 성능을 향상시킨다.  
  
### 🧱 종류<br>  
* 🟦 Primary Shard : 실제 데이터를 저장하는 기본 샤드  
* 🟨 Replica Shard : Primary의 복재본. Primary가 장애 발생 시 Replica 샤드가 Primary기능을 수행한다.  
  
인덱스에 샤드는 아래와 같이 나뉘어진다.  
```plain text  
[index]
  ├── Primary Shard 0
  ├── Replica Shard 0
  ├── Primary Shard 1
  ├── Replica Shard 1
  ├── Primary Shard 2
  └── Replica Shard 2  
```  
  
문서를 색인하게 되면, Elasticsearch는 적절한 Primary Shard를 자동으로 선택하여 저장하게 된다. 이후 검색 요청은 모든 샤드에서 병렬로 실행되어 빠른 응답을 제공한다.  
  
하지만 샤드는 많으면 많을수록 좋은 것은 아니다. 인덱스 하나에 너무 많은 샤드를 쓰게 되면 리소스가 낭비될 수 있기도 하지만 인덱스를 만든 뒤에는 Primary shard 수는 변경이 불가능하다는 특징이 있**다.**  
> 따라서 처음 인덱스를 만들 때 적절한 샤드 수를 정하는 것이 좋다.  
  
  
### 🧠 정리<br>  
1. 샤드는 인덱스를 구성하는 물리적인 단위  
1. Primary는 실제 데이터 저장, Replica는 복제본  
1. 샤드는 분산 저장 + 병렬 검색 처리를 통한 빠른 응답  
1. 효율적인 샤드 설계는 성능과 안정성의 핵심  
  
---  
## 🚩 실습<br>  
로컬에 `Elasticsearch`와 `Kibana`를 설치 한 뒤 학습한 내용을 실습해 보도록 하겠다.  
일단 로컬에 설치한 `Kibana`를 접속 한 뒤 **Dev Tools**에 들어오면 아래와 같이 테스트를 진행해 볼 수 있다.  
### ✅ 1단계: 새로운 인덱스 생성해보기<br>  
```json  
/*
	my_index라는 이름의 인덱스를 생성한다.
	Primary Shard 3개
	Replica Shard 각각 1개
	총 6개의 샤드 생성
*/

PUT /my_index
{
  "settings": {
    "number_of_shards": 3,
    "number_of_replicas": 1
  }
}  
```  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/4b7e27ab-07f9-4ad8-9737-ff58c4979865-image.png)  
  
### ✅ 2단계: 인덱스 정보 확인하기<br>  
```json  
// 현재 클러스터에 있는 인덱스 목록과 상태를 보여준다.

GET /_cat/indices?v  
```  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/8a6ead83-66c6-4916-92c5-07f515056086-image.png)  
  
```json  
// 샤드 상태 확인

GET /_cat/shards/my_index?v  
```  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/92eaccfc-a9a8-4ed9-b25d-5df671321bc3-image.png)  
  
### ✅ 3단계: 문서 색인<br>  
```json  
POST /my_index/_doc
{
  "name": "Alice",
  "age": 30
}  
```  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/14eb28c0-08bc-4162-84a4-33edc6b922ed-image.png)  
  
이렇게 문서를 색인하게 되면, Elasticsearch가 자동으로 하나의 샤드에 문서를 저장하게 된다. (어떤 샤드에 저장될 지는 내부 알고리즘으로 결정)  
  
  
  
### ✅ 4단계: 검색<br>  
```json  
// 전체 검색

GET /my_index/_search  
```  
  
검색에 성공했을 경우 아래와 같은 반환 값을 응답 받는다.  
  
  
---  
## 📌 Reference<br>  
  
  

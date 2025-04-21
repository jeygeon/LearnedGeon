  
---  
  
`Elasticsearch`와 전통적인 관계형 데이터베이스(`RDBMS`)는 데이터 검색 방식에서 차이를 보인다. `Elasticsearch`가 `DB`보다 더 빠른 검색 속도를 보이는 이유는 단순한 데이터 저장이 아닌 **색인 과정**이 있기 때문인데, 이제 `Elasticsearch`와 `DB`의 **검색 방식 차이**에 대해 살펴보려고 한다.  
  
DB에서는 아래와 같이 데이터가 저장되어 있을 때, `Java`를 검색하면 1번부터 4번까지의 데이터를 순차적으로 확인하면서 `Java`라는 단어의 존재 여부를 검사한다. 이러한 구조에서는 결과적으로 2번과 4번 데이터가 조회가 된다.  
  
> 이러면 데이터가 늘어날 수록 검색 대상도 늘어나기 때문에 시간이 오래 걸리는 단점이 생기게 된다.  
  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/3846eba5-8cdc-4fd8-9a65-c93dacaae6be-image.png)  
  
### 🗝️ 역 인덱스 (Inverted Index)<br>  
Elasticsearch에서는 저장되는 데이터들을 각 키워드(`Term`) 별로 분류한 뒤 해당 키워드가 있는 문서의 아이디를 저장한다. 이렇게 되면 사용자가 Java를 검색하면 바로 2, 4번 데이터를 가져올 수 있어서 속도가 매우 빠르다.  
  
그리고 데이터가 추가됐을 때 Term이 가리키는  ID의 배열에 번호만 추가되는 방식이기 때문에 데이터가 추가된다고 해서 속도 변화의 큰 차이가 없다.  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/990aa5cd-7b33-47ce-80fa-8b5c104986a6-image.png)  
  
이렇게 데이터를 저장하는 구조를 `역 인덱스(Inverted Index)`라고 한다.  
## 🚩 애널라이저 (Analyzer)<br>  
데이터가 들어오게 되면 **Inverted Index**를 구성하기 위한 `term`을 만들어내는 과정이 있는데 이 과정을 `Text Analysis`라고 하며, 이러한 과정을 처리하는 기능을 **애널라이저** (`Analyzer`)라고 부른다.  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/4bdcd92e-14ee-4563-9df5-0ddd67a4df3d-image.png)  
  
Analyzer의 구성 요소인 Character Filter, Tokenizer, Token Filter가 각각 하는일은 다른 포스트에 상세히 정리해보도록 하겠다. 각 요소들이 하는 일을 대강 요약하자면 아래와 같다.  
* `character filter` → 특수 문자 제거  
* `tokenizer` → 공백을 기준으로 나누고  
* `token filter` → 소문자로 만든다, 불용어 제거(ex the, a, an, for …) 등..  
  
> 물론 Analyzer를 자신만의 custom analyzer 로 만들어낼 수도 있다.  
  
  
마지막으로 Elasticsearch가 텍스트를 어떻게 분해하는지 확인할 수 있는 API를 이용해서 테스트를 해보도록 하겠다.  
```json  
GET /_analyze
{
  "analyzer": "standard",
  "text": "Running quickly through the park with excitement."
}  
```  
```json  
{
  "tokens": [
    {
      "token": "running",
      "start_offset": 0,
      "end_offset": 7,
      "type": "<ALPHANUM>",
      "position": 0
    },
    {
      "token": "quickly",
      "start_offset": 8,
      "end_offset": 15,
      "type": "<ALPHANUM>",
      "position": 1
    },
    {
      "token": "through",
      "start_offset": 16,
      "end_offset": 23,
      "type": "<ALPHANUM>",
      "position": 2
    },
    {
      "token": "the",
      "start_offset": 24,
      "end_offset": 27,
      "type": "<ALPHANUM>",
      "position": 3
    },
    {
      "token": "park",
      "start_offset": 28,
      "end_offset": 32,
      "type": "<ALPHANUM>",
      "position": 4
    },
    {
      "token": "with",
      "start_offset": 33,
      "end_offset": 37,
      "type": "<ALPHANUM>",
      "position": 5
    },
    {
      "token": "excitement",
      "start_offset": 38,
      "end_offset": 48,
      "type": "<ALPHANUM>",
      "position": 6
    }
  ]
}  
```  
  
위의 테스트는 `analyzer`를 `standard`를 해서 나온 결과이고 **불용어 제거 + 어간 추출**을 하는 `analyzer`인 `english`로 테스트를 한다면 아래와 같은 결과를 확인할 수 있다.  
```json  
GET /_analyze
{
  "analyzer": "english",
  "text": "Running quickly through the park with excitement."
}  
```  
```json  
{
  "tokens": [
    {
      "token": "run",
      "start_offset": 0,
      "end_offset": 7,
      "type": "<ALPHANUM>",
      "position": 0
    },
    {
      "token": "quickli",
      "start_offset": 8,
      "end_offset": 15,
      "type": "<ALPHANUM>",
      "position": 1
    },
    {
      "token": "through",
      "start_offset": 16,
      "end_offset": 23,
      "type": "<ALPHANUM>",
      "position": 2
    },
    {
      "token": "park",
      "start_offset": 28,
      "end_offset": 32,
      "type": "<ALPHANUM>",
      "position": 4
    },
    {
      "token": "excit",
      "start_offset": 38,
      "end_offset": 48,
      "type": "<ALPHANUM>",
      "position": 6
    }
  ]
}  
```  
  
---  
## 📌 Reference<br>  
  

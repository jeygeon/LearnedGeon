  
---  
## 🧹 Token Filter란?<br>  
`tokenizer`에서 `term` 분리 과정 이후에는 분리된 각각의 `term`들이 지정된 규칙에 따라서 처리되는데 이 역활을 `token filter`가 진행한다. `token filter`는 **filter항목에 배열로 지정**해야 하고, **지정한 배열 순서대로 필터가 동작**하기 때문에 순서를 잘 고려해야 한다.  
  
예를 들어   
```plain text  
"I'm learning Elasticsearch"  
```  
`whitespace` **tokenizer**로 나누면  
```json  
["I'm", "learning", "Elasticsearch"]  
```  
이걸 `lowercase`, `stop` 토큰 필터를 적용하면  
```json  
["learning", "elasticsearch"]  
```  
  
## ⚙️ 주요 Token Filter 종류<br>  
### 1. `lowercase`<br>  
모든 term들을 소문자로 바꾼다  
```json  
POST _analyze
{
  "tokenizer": "whitespace",
  "filter": ["lowercase"],
  "text": "Elasticsearch IS Awesome"
}  
```  
```json  
["elasticsearch", "is", "awesome"]  
```  
  
### 2. `stop`<br>  
불용어(**stopwords**)를 제거한다 (ex. `is`, `the`, `a`, `an` …)  
```json  
POST _analyze
{
  "tokenizer": "whitespace",
  "filter": ["stop"],
  "text": "this is a test"
}  
```  
```json  
["test"]  
```  
  
### 3. `stemmer`<br>  
어근(stem)만 남긴다. (ex. `running` → `run`)  
```json  
POST _analyze
{
  "tokenizer": "standard",
  "filter": ["stemmer"],
  "text": "running runs runner"
}  
```  
```json  
["run", "run", "runner"]  
```  
  
### 4. `edge_ngram`<br>  
term을 앞부분에서부터 자른다. 보통 자동 완성에서 많이 사용된다.  
```json  
POST _analyze
{
  "tokenizer": "standard",
  "filter": [
    {
      "type": "edge_ngram",
      "min_gram": 2,
      "max_gram": 4
    }
  ],
  "text": "search"
}  
```  
```json  
["se", "sea", "sear"]  
```  

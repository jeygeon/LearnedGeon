  
---  
  
이번 포스트는 `Analyzer`중 가장 먼저 동작하는 기능인 `Character filter`에 대해서 정리해 보려고 한다.  
## 🔍 캐릭터 필터란?<br>  
**캐릭터 필터**(`Character filter`) 란 `Elasticsearch` `Analyzer` 중 첫 번째 기능으로 **텍스트를 문자 수준에서 전처리를 수행**하게 된다.  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/eb0628bd-d0b0-4417-8b70-8efdb3d3fdd2-image.png)  
## ✏️ 주요 특징<br>  
1. 텍스트 내에서 특정 문자를 치환하거나 제거한다   
1. HTML, 특수 문자 등을 제거한다  
  
## 🧩 필터 종류<br>  
Elasticsearch에서 기본적으로 제공하는 character filter는 다음과 같이 3가지가 있다.  
1. `HTML Strip`  
1. `Mapping`  
1. `Pattern Replace`  
  
### 1. HTML Strip<br>  
해당 필터에서는 `<>`로 된 **XML태그를 제거**하고 `&nbsp;`같은 **HTML 용어들을 제거**하게 된다. `escaped_tags` 옵션을 통해 특정 태그만 유지하는 것도 가능하다. 이 필터는 마크업 태그를 제거하고 순수한 텍스트만 남기는데 유용하다.  
  
요청을 보낸 text를 어떻게 term으로 나뉠지 확인할 수 있는 `_analyze API`를 이용해서 아래와 같이 요청을 전달해보면  
```json  
POST _analyze
{
  "tokenizer": "standard",
  "char_filter": [
    {
      "type": "html_strip",  // HTML_Strip 필터를 사용 한다.
      "escaped_tags": ["b"]  // <b> 태그는 유지하도록 한다.
    }
  ],
  "text": "<p>This is a <b>test</b> sentence with <i>italic</i> text.</p>"
}  
```  
  
Term은 아래처럼 나뉠 것 이다.  
```json  
"This", "is", "a", "b", "test", "b", "sentence", "with", "italic", "text"  
```  
  
### 2. Mapping<br>  
`Mapping` 필터는 **특정 문자를 다른 문자로 치환**하는데 사용되는 도구이다.  
ex)  
* `+` → `plus`  
* `-` → `minus`  
* `&` → `and`  
  
```json  
POST _analyze
{
  "tokenizer": "standard",
  "char_filter": [
    {
      "type": "mapping",
      "mappings": [
        "+ => plus",
        "- => minus"
      ]
    }
  ],
  "text": "C++ and C--"
}  
```  
  
Term은 아래처럼 나뉠 것 이다.  
```json  
"Cplusplus", "and", "Cminusminus"  
```  
  
### 3. Pattern Replace<br>  
`Pattern Replace` filter는 **정규 표현식을 사용하여 특정 패턴을 다른 문자열로 바꾸는 것**이 가능하다.  
→ `Mapping`과 달리 정규식 패턴을 사용할 수 있어서 훨씬 복잡한 치환 작업이 가능하다.   
  
```json  
POST _analyze
{
  "tokenizer": "standard",
  "char_filter": [
    {
      "type": "pattern_replace",
      "pattern": "\\d",
      "replacement": "#"
    }
  ],
  "text": "Phone number: 010-1234-5678"
}  
```  
* `type` : 항상 `“pattern_replace”`  
* `pattern` : 적용할 정규식  
* `replacement` : 대체할 문자열  
  
Term은 아래처럼 나뉠 것 이다.  
```json  
"Phone", "number"  
```  
(참고. `tokenizer : standard`는 **특수 문자를 제거**해서 위의 결과에 나오지 않는다.)  
  

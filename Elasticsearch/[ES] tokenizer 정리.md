  
---  
## 🔍 Tokenizer란?<br>  
`Tokenizer`는 `Elasticsearch` `Analyzer`  중 2번째로 동작하는 구성 요소로써 `Character Filter`에서 처리된 **문자열을 잘게 나누어서 토큰을 만드는 역할**을 한다.  
  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/0e7c5723-9492-4f78-8b10-b2e2dabdc02a-image.png)  
  
예를 들어, `“elasticsearch is a search engine”` 이라는 문장을 standard tokenizer로 분석하게 되면 아래와 같이 나누어지게 된다.  
```json  
["elasticsearch", "is", "a", "search", "engine"]  
```  
  
## ⚙️ Tokenizer의 종류<br>  
1. `Standard` : 공백으로 term을 구분하면서 특수 문자를 제거한다. (**default**)   
1. `whitespace `: 공백으로만 term을 구분  
1. `keyword` : 문자열 전체를 한 덩어리로 저장  
1. `uax_url_email` : URL이나 이메일을 하나의 토큰으로 유지  
  
  

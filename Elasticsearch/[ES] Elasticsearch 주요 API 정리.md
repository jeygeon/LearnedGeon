  
---  
## 1. 인덱스(Index)<br>  
### 생성<br>  
```json  
PUT /book
{
  "mappings": {
    "properties": {
      "title": { "type": "text" },
      "author": { "type": "keyword" },
      "year": { "type": "integer" }
    }
  }
}  
```  
  
### 조회<br>  
```json  
GET /book  
```  
  
### 삭제<br>  
```json  
DELETE /book  
```  
  
### 인덱스 존재 여부 확인<br>  
```json  
HEAD /book  
```  
  
## 2. 문서(Document)<br>  
### 색인 (자동 ID)<br>  
```json  
 POST /book/_doc
{
  "title": "HTML 정복하기",
  "author": "Hong",
  "year": 2025
}  
```  
  
### 색인 (지정 ID)<br>  
```json  
 POST /book/_doc/2
{
  "title": "CSS 정복하기",
  "author": "Hong",
  "year": 2025
}  
```  
  
  
### 조회 (by ID)<br>  
```json  
GET /book/_doc/2  
```  
  
### 삭제 (by ID)<br>  
```json  
DELETE /book/_doc/2  
```  
  
### 문서 존재 확인 (by ID)<br>  
```json  
HEAD /my_index/_doc/2  
```  
  
## 3. 검색 (Query)<br>  

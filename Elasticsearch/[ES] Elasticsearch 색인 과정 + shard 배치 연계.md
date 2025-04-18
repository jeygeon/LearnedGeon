  
---  
  
이번 글에서는 색인에 대해서 자세히 정리하기 보다는, 문서가 들어왔을 때 색인되는 과정과 Elasticsearch shard 배치와 연관 지어서 전체적인 흐름을 정리해보려 한다.  
## ✏️ 색인(Indexing) 이란?<br>  
> 문서를 분석하고 저장하는 과정을 색인이라고 정의한다.  
Elasticsearch에서 문서를 분석하고 저장하는 일련의 과정을 색인 이라고 한다.  
  
  
한 클러스터 안에 3개의 노드가 있고 특정 인덱스가 4개의 샤드로 구성 되어 있다면 아래와 같은 이미지의 형태를 하고 있을 것 이다.  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/db4bd4d8-fb27-4708-94fd-3b1c30d89823-image.png)  
  
`Primary shard`는 실제 데이터를 저장하는 기본 단위이며 `Replica shard`는 Primary shard의 복사본이다. Primary shard와 Replica shard는 **장애 복구나 백업, 읽기 부하 분산용**으로 **반드시 다른 노드에 위치**해 있어야 한다.   
  
이때 클라이언트가 문서 색인을 요청하면 Elasticsearch는 문서 ID와 샤드 수를 기반으로 어느 Primary 샤드에 저장할지를 결정한다.  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/4e3761d1-d9ed-4bb3-bdb5-89dd76315b3d-image.png)  
  
Primary 샤드에 색인이 실행되게 되고 성공하면 → Replica 샤드로 색인 요청이 복제된다.  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/9fdf3660-cf78-4548-97db-93503ede4c20-image.png)  
  
이후 Replica 샤드로 복제가 성공하게 되면 사용자에게 `200 OK` 또는 `201 Created` 응답을 반환하게 된다.  
  
만약 특정 노드가 다운돼서 해당 노드에 있는 샤드가 유실이 된다면 다른 노드들에 서 복제를 해서 데이터의 유실없이 사용할 수가 있다.  
  
3번 노드가 다운된 상황에서 1번과 3번 Primary 샤드는 다른 노드에 복제본을 생성하게 되고, 2번 Replica 샤드는 Primary 샤드로 승격하고 이 후 Replica 샤드를 생성하게 된다.  
![1번, 3번 Primary 샤드가 Replica 샤드를 새로 생성](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/9b2a15fe-18b4-41cd-bfcf-3f3d0a86b4c1-image.png)  
  
![2번 Replica 샤드가 Primary 샤드로 승격 후 Replica 샤드 새롭게 생성](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/982864fd-d101-414d-8bbb-5f34b07f0ab7-image.png)  
  
이런식으로 Elasticsearch는 운영 중에 노드가 유실되어도 데이터를 잃어버리지 않고 데이터의 가용성과 무결성을 유지하게 된다.  
  
---  
## 📌 Reference<br>  
  
  

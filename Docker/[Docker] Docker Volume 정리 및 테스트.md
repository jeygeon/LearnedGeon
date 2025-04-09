  
---  
## 1️⃣ Docker Volume 이란?  
---  
Docker 컨테이너의 데이터는 휘발성으로 존재한다. 컨테이너가 삭제된다면 그 안에 있던 데이터도 모두 삭제가 된다.  
  
이 때 Docker Volume을 사용하게 된다면 컨테이너 외부에서도 데이터가 저장되기 때문에 데이터 영속성을 부여할 수 있다. 그래서 컨테이너를 삭제하고 다시 생성해도 그 안에 있던 데이터들을 그대로 복원이 가능하다.  
  
## 2️⃣ 저장 방식  
---  
Docker Volume에서 사용하고 있는 저장 방식은 Volume, Bind Mount, tmpfs Mount 이렇게 3가지 방식이 존재한다.  
### 1. Volume  
![https://docs.docker.com/engine/storage/volumes/](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/92aa6453-79e4-42db-9023-e1ab45d97641-image.png)  
* docker volume create 명령어로 볼륨 생성 가능  
* (Linux 기준) Docker가 /var/lib/docker/volumes 아래에 저장  
* 컨테이너끼리 공유 가능  
* Docker가 직접 관리  
  
💡 볼륨 생성 : docker volume create [이름]  
  
💡 볼륨 확인 : docker volume ls  
  
💡 볼륨 사용  
  
  
### 2. Bind Mount  
![https://docs.docker.com/engine/storage/bind-mounts/](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/34d35a1c-ac34-4790-a28d-3e3dc8ee2ad2-image.png)  
* 호스트 OS의 저장 경로를 그대로 컨테이너에 연결  
* 호스트 OS가 직접 저장을 제어  
* 실시간으로 양방향 바인딩이 가능하여 개발 시 자주 사용된다.  
### 3. tmpfs Mount  
![https://docs.docker.com/engine/storage/tmpfs/](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/50f212af-f773-428a-aedd-9df20c3400a4-image.png)  
* 메모리(RAM) 기반 저장 방식이며, 디스크 사용은 하지 않는다  
* 컨테이너가 종료되면 데이터 사라진다.  
  
### 📦 Docker 볼륨 유형 비교  
  
  
## 3️⃣ 실습  
---  
메모리에 저장 하는 방식은 제외하고 Volume 방식과 Bind Mount 두 가지 방식을 이용해서 실습을 진행해 보도록 하겠다.  
  
### 1. Volume 방식  
```bash  
docker run -d -p 3306:3306 --mount type=volume,source=volume_test,target=/var/lib/mysql mysql  
```  
이렇게 되면 volume_test라는 Docker가 관리하는 실제 저장소로 경로 설정이 된다.  
Volume의 실제 위치는 리눅스 기준 /var/lib/docker/volumes/volume_test/_data가 되지 만 직접 접근은 권장되지 않고 Docker 명령어로 접근, 백업하는 것이 일반적이다.  
  
### 2. Bind Mount 방식  
* Host OS 디렉토리 : C:\jeygeon\docker-mysql\mysql-data   
* Mysql Cotainer 디렉토리 : /var/lib/mysql  
```bash  
$ docker run -d -p 3306:3306 -v C:\jeygeon\docker-mysql\mysql-data:/var/lib/mysql mysql  
```  
  
위에서도 설명했지만 위의 명령어는 아래로 변경해도 똑같다.  
```bash  
docker run -d -p 3306:3306 --mount type=bind,source=C:\jeygeon\docker-mysql\mysql-data,target=/var/lib/mysql mysql  
```  
  
변경된 내용:  
-v C:\jeygeon\docker-mysql\mysql-data:/var/lib/mysql  
--mount type=bind,source=C:\jeygeon\docker-mysql\mysql-data,target=/var/lib/mysql  
  
호스트 OS 의 디렉토리에 Mysql Container 속 데이터 파일들이 정상적으로 동기화 된 모습을 확인 할 수 있다.  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/b012281e-6177-4cb1-bbb7-aa415713f644-image.png)  

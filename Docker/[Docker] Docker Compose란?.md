  
  
---  
  
## 1️⃣ Docker Compose란?  
---  
### 💡 Docker Compose  
  
  
Spring Boot 프로젝트와 DB 컨테이너를 띄운다고 했을 때 Docker Compose가 없었다면 아래 두 명령어를 각각 입력해서 컨테이너를 따로 띄웠을 것 이다.  
```bash  
docker run -d -p 8080:8080 my-app  
```  
```bash  
docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD={설정할 root 패스워드} -v {사용할 볼륨}:/var/lib/mysql mysql  
```  
  
이렇게 했을 때의 단점은 크게 두 가지가 있다  
1. 명령어를 각각 쳐서 컨테이너를 올려야 한다는 번거로움  
1. 들어가는 옵션의 종류  
  
mysql 컨테이너를 기동할 때만 하더라도 포트 지정, root 패스워드 지정, 볼륨 지정, 백그라운드로 실행 등 들어가는 명령어 옵션들이 매우 많다. 컨테이너를 기동할 때 마다 이런 것들을 일일이 입력해줘야 하는 것은 매우 번거로운 일일 것이다.  
  
여기서 Docker Compose를 사용한다면, 아래의 단 하나의 명령어로 모두 해결이 가능하다.  
```bash  
docker compose up  
```  
  
### 👉 Docker Compose 사용 방법  
Docker Compose의 사용 방법은 compose.yml 파일을 작성 한 뒤 해당 파일이 있는 경로에서 docker compose up 명령어를 실행시키면 된다.  
compose.yml 파일에는 내가 기동 시킬 컨테이너의 정보를 작성한다.  
  
## 2️⃣ Docker Compose 실습  
---  
compose.yml 파일에서는 당연히 한 개의 컨테이너 기동도 가능하고, 두 개 이상도 가능하다.  
아래는 nginx 이미지를 사용하여 compose.yml 을 작성하여 컨테이너를 기동하는 과정이다.  
### 👉 compose.yml 작성  
```yaml  
services: # docker compose 에서는 하나의 컨테이너를 service 로 부른다
  my-web-server:  # services 바로 밑에 service 에 대한 이름을 붙인다. (서비스에 대한 이름은 아무렇게 지어도 된다.)
    container_name: webserver  # 컨테이너의 이름을 webserver 로 짓겠다.
    image: nginx  # 이미지는 nginx 를 사용하겠다.
    ports:
      - 80:80 # 포트 포워딩은 80:80 으로 하겠다.

  my-cache-server:
	  conatainer_name: cacheserver
    image: redis
    ports:
      - 6379:6379  
```  
  
참고로 위의 내용을 컨테이너를 기동하는 명령어로 변환하자면 아래와 같다.  
```bash  
$ docker run --name webserver -p 80:80 nginx  
```  
```bash  
$ docker run --name cacheserver -p 6379:6379 redis  
```  
  
### 👀 실행  
( 백그라운드로 실행 모드는 yml파일에 작성하지 않고 명령어를 실행할 때 -d 옵션을 붙힌다. )  
```bash  
$ docker compose up -d  
```  
  
이후 docker ps 명령어로 기동 되어 있는 컨테이너를 확인할 수 있다.  
  
좀 더 다양한 키워드를  사용해서 compose 파일을 작성해보겠다.  
```yaml  
services:
  db:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: 1234
      MYSQL_DATABASE: myapp
    volumes:
      - db_data:/var/lib/mysql
    ports:
      - "3306:3306"

  backend:
    build: ./backend
    ports:
      - "3000:3000"
    environment:
      DB_HOST: db
      DB_USER: root
      DB_PASSWORD: 1234
      DB_NAME: myapp
    depends_on:
      - db

volumes:
  db_data:  
```  
  
### 🔑 주요 키워드  
* services : 사용할 컨테이너 그룹 정의  
* image : 사용할 Docker 이미지  
* build : Dockerfile의 경로 지정  
* ports : 호스트:컨테이너 포트 매핑  
* environment : 환경 변수 지정  
* volumes : 볼륨 경로 지정  
* deponds_on : 의존 관계 설정 (실행 순서)  
  
---  
## 📌 Reference  
  

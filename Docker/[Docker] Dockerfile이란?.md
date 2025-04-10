  
  
---  
  
Docker의 핵심 기능 중 하나인 Dockerfile에 대해서 정리해 보면서 내가 만든 간단한 Spring Boot 프로젝트를 이미지화 해서 컨테이너에 띄어보는 것 까지 실습을 해보도록 하겠다.  
  
## 💡 Dockerfile 정리  
---  
### 1️⃣ Dockerfile이란?  
  
```bash  
Dockerfile 👉 이미지 생성 👉 이미지로 컨테이너 실행  
```  
  
Dockerfile에서는 어떤 OS를 기반으로 이미지를 만들지, 해당 이미지로 컨테이너를 실행한 뒤 어떤 명령어를 실행할지 등의 설정이 가능하다.  
  
### 2️⃣ Dockerfile의 기본 구조  
```docker  
# 1. 어떤 OS나 이미지 기반으로 만들건지
FROM node:18

# 2. 작성자 정보 (선택)
LABEL maintainer="hong@gmail.com"

# 3. 컨테이너 안에 코드 복사
COPY . /app

# 4. 현재 디렉토리 설정
WORKDIR /app

# 5. 패키지 설치 등 명령 실행
RUN npm install

# 6. 컨테이너 시작 시 실행할 명령어
ENTRYPOINT ["npm", "start"]  
```  
  
### 3️⃣ Dockerfile의 주요 명령어  
* FROM : 기반으로 할 이미지 지정 ( ex. jdk, nginx, tomcat … )  
* LABEL : 메타 데이터 ( 작성자, 설명 등 … )  ( 선택사항 )  
* WORKDIR :  컨테이너 내의 작업 디렉토리 설정  
* COPY : 호스트의 파일 or 디렉토리 → 컨테이너로 복사  
* RUN : 컨테이너 생성 중 실행할 명령어  
* ENV : 환경 변수 설정  
* ENTRYPOINT : 컨테이너 생성 후 실행할 명령어  
  
💡 참고사항  
1. RUN vs ENTRYPOINT   
1. RUN할 때 여러 명령어를 사용하고 싶으면?  
  
### 4️⃣ 이미지 빌드 및 실행  
(상세한 이미지 빌드 방법과 실행은 아래 내가 만든 Spring Boot 프로젝트 이미지를 만들면서 다시 한번 보도록 하겠다.)  
1. 이미지 만들기  
1. 생성된 이미지 확인  
1. 컨테이너 실행  
  
  
## 💡 Spring Boot 프로젝트 이미지 빌드 및 컨테이너 기동 실습  
---  
### 1️⃣ Spring Boot 프로젝트 build  
  
1. 최상위 디렉토리에 Dockerfile 이라는 이름으로 파일을 생성한 뒤 내용을 작성한다. 최소한의 테스트를 위해 아래와 같이 작성했다.  
  
### 2️⃣ 이미지 생성  
인텔리제이 터미널을 켜서 docker build -t my-app . 명령어를 실행시켜 이미지를 생성한다.  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/0b46deba-e3ac-4e11-9182-3364cace8cca-image.png)  
  
docker image ls  로 이미지 목록을 확인해 보면 내가 생성한 이미지가 있는 것을 확인할 수 있다.  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/73d68abc-0c51-418d-b768-f8a38bf833c1-image.png)  
  
### 3️⃣ 컨테이너 기동 후 접속 확인  
docker run -p 8080:8080 my-app 으로 내가 만든 이미지로 컨테이너를 실행시켰다.  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/ca43e496-9478-4e75-95d1-4cda94b48495-image.png)  
  
이후 localhost:8080으로 접근 시 아래와 같이 정상 동작 하는 모습을 확인 할 수 있다.  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/634db691-a816-48c4-9b29-b561323bc3a5-image.png)  
  
---  
## 📌 Reference  
  

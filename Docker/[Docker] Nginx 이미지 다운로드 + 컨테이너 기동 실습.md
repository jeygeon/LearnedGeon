  
---  
이번엔 `docker`에서 `nginx` 이미지를 다운로드 한 뒤 컨테이너를 기동해서 실행시켜보는 실습을 진행해 보도록 하겠다.  
나는 윈도우에서 실습을 진행하였는데, 실습 전 docker 설치는 아래 블로그를 참고하면 된다.  
  
* window  
  
  
* macOS  
  
  
설치가 다 된 후 각 OS의 터미널에서 `docker` 명령어가 잘 입력되는지 확인 한 뒤 아래 실습을 진행해주면 된다.  
## 1️⃣ Nginx 이미지 다운로드<br>  
* nginx 이미지 다운로드  
```bash  
$ docker pull nginx  
```  
  
* 다운로드 된 이미지 확인  
```bash  
$ docker image is

REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
nginx        latest    4cad75abc83d   2 months ago   192MB  
```  
  
## 2️⃣ 컨테이너 기동<br>  
* 컨테이너 기동  
```bash  
$ docker run --name webserver -d -p 80:80 nginx  
```  
`docker run` : 새로운 컨테이너를 생성하고 실행하는 명령어  
`--name webserver` : 컨테이너의 이름을 `webserver`로 지정 ( 없다면 docker가 임의로 지정 )  
`-d` : 컨테이너를 백그라운드에서 실행  
`-p 80:80` : 포트 포워딩 설정. 호스트의 포트 `80`을 컨테이너의 포트 `80`으로 연결  
`nginx` : 사용할 docker 이미지  
  
* 현재 실행 중인 컨테이너의 목록 확인  
```bash  
$ docker ps

CONTAINER ID   IMAGE     COMMAND                   CREATED          STATUS          PORTS                NAMES
55aea3183c1a   nginx     "/docker-entrypoint.…"   40 seconds ago   Up 38 seconds   0.0.0.0:80->80/tcp   webserver  
```  
## 3️⃣ 컨테이너 접근<br>  
```bash  
$ docker exec -it webserver /bin/bash  
```  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/66a654a9-0791-4b4c-a49c-df4061d43aba-image.png)  
## 4️⃣ 접속 테스트<br>  
주소 창에 `localhost:80`으로 접근 가능한지 확인  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/jeygeon/images/0e27dcc8-fe52-4055-a4e7-02acedb32e77-image.png)  
## 5️⃣ 컨테이너 종료<br>  
```bash  
$ docker stop webserver  
```  
`docker stop` : 컨테이너 정지  
`webserver` : 정지할 컨테이너 이름  
  
---  
### 📌 References<br>  
  

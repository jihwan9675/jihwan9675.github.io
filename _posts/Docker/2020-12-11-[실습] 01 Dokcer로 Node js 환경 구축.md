---
title:  "[실습] Dokcer로 Node.js 환경 구축"
categories:
  - Docker
tags:
  - Docker
last_modified_at: 2020-12-11
---
# [실습] 01 Dokcer로 Node.js 환경 구축

실습을 진행할 디렉토리를 만든다.

![0.png](/assets/images/docker/실습 01 Dokcer로 Node js 환경 구축/0.png)

아래 주소를 클론한다.

```bash
git clone https://gitlab.com/yalco/practice-docker.git
```

VS Code로 창을 띄운다.

![1.png](/assets/images/docker/실습 01 Dokcer로 Node js 환경 구축/1.png)

아래 명령어를 실행한다. 그러면 node 환경이 구축된다. run 명령어의 옵션으로 사용되는 -it는 이 컨테이너를 연 다음 그 환경에서 CLI를 사용한다는 것이다.

```bash
docker run -it node
```

![2.png](/assets/images/docker/실습 01 Dokcer로 Node js 환경 구축/2.png)

docker hub에서 node를 검색해보면 Docker Image가 올라와있다. Docker Image란 리눅스 컴퓨터의 특정 상태를 캡쳐해서 박제해놓은 것을 말한다.

![3.png](/assets/images/docker/실습 01 Dokcer로 Node js 환경 구축/3.png)

터미널을 하나 더 열어서 docker images 명령어로 이미지를 확인한다.

![4.png](/assets/images/docker/실습 01 Dokcer로 Node js 환경 구축/4.png)

docker ps 명령어를 입력했을 경우다. 임의로 지어진 ID와 이미지 정보, 커맨드,  Names 등 나온다.

![5.png](/assets/images/docker/실습 01 Dokcer로 Node js 환경 구축/5.png)

아래 명령어를 치면 리눅스 화면이 나오는 것을 확인할 수 있는데, 이는 bash명령어를 통해 컨테이너 내부의 가상의 리눅스 환경으로 들어간 것이다.

```bash
docker exec -it quirky_shaw bash
```

![6.png](/assets/images/docker/실습 01 Dokcer로 Node js 환경 구축/6.png)

다시 원래 터미널로 돌아와서 [Ctrl + c]를 눌러서 노드를 끄고 docker ps를 치면 다음과 같이 아무것도 나오지 않는다. ps는 현재 작업이 진행중인 즉 내부의 근무자가 일하고 있는 컨테이너만 표시가 된다. 그래서 -a 를 붙혀서 실행하면 모든 컨테이너의 정보가 나타난다.

![7.png](/assets/images/docker/실습 01 Dokcer로 Node js 환경 구축/7.png)

아래 명령어로 컨테이너를 삭제하도록 한다.

```bash
# 실행중인 컨테이너 종료
# 삭제
docker stop $(docker ps -aq)
docker system prune -a
```

![8.png](/assets/images/docker/실습 01 Dokcer로 Node js 환경 구축/8.png)

Frontend 폴더 안에 Dockerfiles를 분석해보겠다.

![9.png](/assets/images/docker/실습 01 Dokcer로 Node js 환경 구축/9.png)

```docker
FROM node:12.18.4

# 이미지 생성 과정에서 실행할 명령어
RUN npm install -g http-server 

# 이미지 내에서 명령어를 실행할(현 위치로 잡을) 디렉토리 설정
WORKDIR /home/node/app

# 컨테이너 실행시 실행할 명령어
CMD ["http-server", "-p", "8080", "./public"]

# 이미지 생성 명령어 (현 파일과 같은 디렉토리에서)
# docker build -t {이미지명} .

# 컨테이너 생성 & 실행 명령어
# docker run --name {컨테이너명} -v $(pwd):/home/node/app -p 8080:8080 {이미지명}
```

- RUN : 이미지를 생성하는 과정에서 실행할 명령어
- WORDKIR : 이 안(컨테이너)에서 명령어를 실행할 위치를 설정한다.
- CMD : 배쉬 명령어

RUN vs CMD : RUN은 환경을 구축 시 실행되는 문장이고 CMD는 환경 구축 후 실행될 때 실행 됨.

frontend 디렉토리로 옮기고 아래 코드를 실행한다. 이미지를 생성하는 명령어이다. 쉽게 이미지가 생성된다.

```docker
# docker build -t {이미지명} .
# dockerfile 이름이 dockerfile이면 .만 쳐도 됨 원래는 상대경로 적는 곳

docker build -t fontend-img .
```

![10.png](/assets/images/docker/실습 01 Dokcer로 Node js 환경 구축/10.png)

![11.png](/assets/images/docker/실습 01 Dokcer로 Node js 환경 구축/11.png)

이번에는 컨테이너를 여러가지 옵션을 줄 것이다. 먼저 컨테이너의 이름을 지정해주고 

```docker
# 컨테이너 생성 & 실행 명령어
docker run --name {컨테이너명} -v $(pwd):/home/node/app -p 8080:8080 {이미지명}
```

볼륨의 약자인 v옵션을 통해서 볼륨 위치도 지정할 것이다. 도커에서 볼륨이란, 컨테이너와 특정 폴더를 공유하는 것을 말한다. pwd 옵션은 현위치이다. 즉 해석을 하자면 도커파일의 위치한 폴더 안의 내용이 컨테이너의 /home/node/app 위치에 들어간다는 의미다. 

p는 포트옵션이다. 가령 8080:8080이라면 컨테이너도 8080포트로 나가고 나의 로컬도 8080으로 나간다는 소리다

![12.png](/assets/images/docker/실습 01 Dokcer로 Node js 환경 구축/12.png)

만약 같은 이미지의 컨테이너가 같은 포트로 나가려하면 충돌이 나기 때문에 로컬의 포트를 수정해준다.

```docker
docker run --name {name} -v $(pwd):/home/node/app -p 8080:8081 {image}
```

![13.png](/assets/images/docker/실습 01 Dokcer로 Node js 환경 구축/13.png)

실행해준다.

![14.png](/assets/images/docker/실습 01 Dokcer로 Node js 환경 구축/14.png)

![15.png](/assets/images/docker/실습 01 Dokcer로 Node js 환경 구축/15.png)

백엔드와 DB를 연동되지 않아 저런 화면이 나온다. 일단 앞서 배운 삭제코드로 컨테이너를 삭제해본다.

그렇다면 백엔드와 DB 도커파일도 마찬가지로 작성하면 되는데... 번거롭다.  방법이 없을까?

방법이 있다. docker-compose.yml을 살펴보자.

```docker
version: '3'
services:
  database:
    # Dockerfile이 있는 위치
    build: ./database
    # 내부에서 개방할 포트 : 외부에서 접근할 포트
    ports:
      - "3306:3306"
  backend:
    build: ./backend
    # 연결할 외부 디렉토리 : 컨테이너 내 디렉토리
    volumes:
      - ./backend:/usr/src/app
    ports:
      - "5000:5000"
    # 환경변수 설정
		# DBHOST = backend.py에 해당 환경변수가 들어가 있다.
    environment: 
      - DBHOST=database
  frontend:
    build: ./frontend
    # 연결할 외부 디렉토리 : 컨테이너 내 디렉토리
    volumes:
      - ./frontend:/home/node/app
    ports:
      - "8080:8080"
```

실행화면 : `docker-compose up`

![16.png](/assets/images/docker/실습 01 Dokcer로 Node js 환경 구축/16.png)

시간이 오래 소모되며 완성되면 127.0.0.1:8080 에 접속해보자.

![17.png](/assets/images/docker/실습 01 Dokcer로 Node js 환경 구축/17.png)
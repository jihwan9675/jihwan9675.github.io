---
title:  "[실습] Docker에 Python Flask 웹 서버 올리기"
categories:
  - Docker
tags:
  - Docker
last_modified_at: 2020-12-12
---
# [실습] Docker에 Python Flask 웹 서버 올리기

- Docker를 활용하여 배포를 해볼 것이다. 먼저 깃 클론을 해준다.

    [jihwan9675/eardrum_web](https://github.com/jihwan9675/eardrum_web)

git clone https://github.com/jihwan9675/eardrum_web

### 서버 구성

![0.png](/assets/images/docker/실습 Docker에 Python Flask 웹 서버 올리기/0.png)

- Dockerfile

```docker
FROM python:3.7.9
COPY . /app
WORKDIR /app
RUN apt-get update && \
    apt-get install libgl1-mesa-glx -y # 이 명령어가 없으면 OpenCV load 시 오류 발생
RUN pip install --upgrade pip
RUN pip install --no-cache-dir -r requirements.txt
EXPOSE 80 # 80번 포트 개방
CMD [ "python","server.py","&" ]
```

### Local에서 실행해보기

1. build 하기

    ```bash
    docker build -t ear .
    ```

    ![1.png](/assets/images/docker/실습 Docker에 Python Flask 웹 서버 올리기/1.png)

2. 시간이 조금 걸린다 약 4분정도 완성 되면 실행을 시켜본다.

    ```bash
    docker run --name=flask -p 80:80 test
    # p는 포트옵션 --name은 이름 지정인데 처음 실행할때만 해주면 된다 !
    ```

    ![2.png](/assets/images/docker/실습 Docker에 Python Flask 웹 서버 올리기/2.png)

### AWS EC2 에 올려보기

결과적으로 EC2에선 실패를 했다. 잘 몰랐는데 아마존 클라우드가 램을 1기가밖에 지원을 안해서였다. 하지만 언젠가 좋은 회사에 가서 좋은 인스턴스를 사용하면 할 수 있기때문에 흐름만 정리를 하였다.

- [Local → AWS EC2] WinSCP을 통한 파일 전송

    ![3.png](/assets/images/docker/실습 Docker에 Python Flask 웹 서버 올리기/3.png)

    [설치 완료]

![4.png](/assets/images/docker/실습 Docker에 Python Flask 웹 서버 올리기/4.png)

- Ubuntu에서 간단 도커 설치 과정(그대로 복붙)

    ```docker
    sudo apt update
    sudo apt install apt-transport-https
    sudo apt install ca-certificates
    sudo apt install curl
    sudo apt install software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
    apt-cache policy docker-ce
    sudo apt install docker-ce
    ```

- 도커로 이미지를 만들어 본다.

    ```bash
    sudo docker build -t earweb .
    # -t 옵션은 tty, 터미널 입력 옵션
    # earweb은 컨테이너 이름
    # . 은 현재 위치 파일 올리기
    ```

    ![5.png](/assets/images/docker/실습 Docker에 Python Flask 웹 서버 올리기/5.png)

### 한계

AWS EC2 프리티어의 메모리는 1기가인데 프로그램을 실행시켜보면 killed 라고 떠서 로그를 찾아보니 Out of Memory 오류가 났다. (dmesg | grep -E -i -B100 'killed process')

![6.png](/assets/images/docker/실습 Docker에 Python Flask 웹 서버 올리기/6.png)
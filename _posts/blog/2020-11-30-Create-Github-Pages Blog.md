---
title:  "Github Pages Blog 생성"
categories:
  - Blog
tags:
  - Blog
last_modified_at: 2020-11-30T08:06:00-05:00
---


# Github Pages Blog 생성

---

## 1. Create Github Repository

![1.png](/assets/images/2020-11-30-Create-Github-Pages-Blog/1.png)

- 1번 박스는 레포지토리 이름을 지정하는 것인데, 본인은 미리 만들어 놨기 때문에 다음과 같은 화면이다. 이름 양식은 `자신의 GithubID.github.io`이다.
- 2번 박스는 리드미(레포지토리 설명) 파일을 추가하는 것인데 안넣어도 된다.
- 3번 박스는 생성 버튼

## 2. [minimal-mistakes](https://github.com/mmistakes/minimal-mistakes)

나는 Jekyll theme를 사용하였다. 마크 다운을 사용할 수 있다는 강력한 장점이 있고 많은 사용자들이 있기 때문에 정보를 얻기 편했기 때문이다. 그리고 선택한 테마는 [minimal-mistakes](https://github.com/mmistakes/minimal-mistakes)를 선택. 가장 마음에 들고 깔끔한 테마였다.

---

### 2.1 테마 다운로드

먼저 테마를 다운 받아야 한다. [minimal-mistakes](https://github.com/mmistakes/minimal-mistakes)에서 Download ZIP을 통해 다운받았다. git clone으로 해도 된다. 각자 편한 방법을 사용하기로 한다.

![2.png](/assets/images/2020-11-30-Create-Github-Pages-Blog/2.png)
### 2.2 _config.md 파일 수정

홈페이지의 기초적인 설정을 하는 파일이다. 원하는 대로 설정 값을 변경하도록 한다.

![3.png](/assets/images/2020-11-30-Create-Github-Pages-Blog/3.png)
### 2.3 레포지토리에 올리기

```powershell
$ git init
$ git add .
$ git commit -m "First commit"
$ git push
```

다음과 같은 코드로 커밋과 푸쉬를 하여 내 레포에 올린다.

![4.png](/assets/images/2020-11-30-Create-Github-Pages-Blog/4.png)

### 2.4 접속해보기

아까 만든 레포지토리 이름 그대로 접속할 수 있다. [https://jihwan9675.github.io/](https://jihwan9675.github.io/)

![5.png](/assets/images/2020-11-30-Create-Github-Pages-Blog/5.png)

## 3. 참고 사이트

---

[Jekyll theme을 사용하여 블로그 생성하기](https://devinlife.com/howto%20github%20pages/new-blog-from-template/)

[Quick-Start Guide](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/)
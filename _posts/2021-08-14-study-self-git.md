---
layout: post
title:  "[Git] 2021/8/13 git push 오류"
subtitle:   "깃 보안 관련 이슈"
categories: study
tags: learn git
comments: true
---

> 갑작스러운 깃 보안 관련 이슈 해결



## 문제 발견

![image-20210814163305184](https://kihoonahn1.github.io/assets/img/study/2020-08-13-study-self-git1.png)

분명 어제까지는  잘 되던 깃이 갑자기 오류를 일으켰다.

 ```bash
 $ git push origin master
 remot: Support for password authentication was removed on August 13, 2021. Please use a personal access token instead.
 remote: Please see https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/ for more information
 fatal: unable to access 'https://github.com/KiHoonAhn1/Algorithms.git/': The requested URL returned error: 403
 ```



'무슨 권한이 없다는 걸까?' 하고 적혀있는 [주소](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations)로 들어가봤다.

비밀번호 대신 토큰을 써야 한다고 써있는 것 같긴 한데... **그래서 어떻게 하라고?**

그냥 검색으로 해결하자 하고 오류 메시지를 구글링한 결과 답을 찾아냈다!



## 문제 해결 방법

### 토큰 발급

우선 위에 나왔던 대로 토큰을 발급받아야 한다. [Github](https://www.github.com)에 들어가 로그인 후 `Settings -> Developer Settings -> Personal access tokens`로 이동한다.

<img src="https://kihoonahn1.github.io/assets/img/study/2020-08-13-study-self-git2.png" alt="image-20210815120442074" style="zoom:50%;" />

<img src="https://kihoonahn1.github.io/assets/img/study/2020-08-13-study-self-git3.png.png" alt="image-20210815120644257" style="zoom:50%;" />



그럼 아래와 같은 화면을 볼 수 있다. 여기서 `Generate new token` 클릭!

![image-20210815120845024](https://kihoonahn1.github.io/assets/img/study/2020-08-13-study-self-git4.png)



**Note**는 토큰 이름인데 맘대로 정해도 된다.

비밀번호 입력 후 **Expiration**은 다시 갱신하기 귀찮아서 'No expiration'을 선택했는데, 원하는 대로 하면 될 것 같다.

**Select scopes**는 다음과 같이 체크를 했다. 검색해서 보면 `delete_repo`도 선택한 분들이 많은데 나는 repository 삭제는 항상 깃허브 홈페이지에서 하기 때문에 굳이 체크하지 않았다. 나중에 필요하면 토큰을 편집해야겠다.

![image-20210814165531675](https://kihoonahn1.github.io/assets/img/study/2020-08-13-study-self-git5.png)



Generate Token을 하면 토큰을 확인할 수 있는 페이지가 나온다. 이걸 그냥 넘어가면 토큰을 재생성해야하니까 꼭 복사하고 넘어가도록 하자!

![image-20210815121641174](https://kihoonahn1.github.io/assets/img/study/2020-08-13-study-self-git6.png)

>혹시 토큰을 복사하지 않고 그냥 넘어갔다면 목록에서 토큰 클릭 후 `Regenerate token`을 하면 새로운 토큰 번호가 발급된다



### 토큰 입력

토큰을 발급받았으면 이제 쉬운 일만 남았다.

`자격 증명 관리자` 에 들어가  `Windows 자격 증명`을 클릭하면 아래와 같은 화면이 나올 것이다.

`git:https://github.com` 을 찾아 클릭하면 아래와 같이 상세 정보가 나올텐데 **편집**을 누르자.

![image-20210814165729019](https://kihoonahn1.github.io/assets/img/study/2020-08-13-study-self-git7.png)

여기서 암호에 복사해뒀던 토큰을 넣고 저장하면  간단하게 끝난다.

![image-20210814165755605](https://kihoonahn1.github.io/assets/img/study/2020-08-13-study-self-git8.png)


---
title: "<DAY 3> Gatsby blog"
date: "2017-08-18T22:12:03.284Z"
template: "post"
draft: false
slug: "/posts/the-birth-of-movable-type/"
category: "Typography"
tags:
  - "Open source"
  - "Gatsby"
  - "Typography"
description: "German inventor Johannes Gutenberg developed a method of movable type and used it to create one of the western world’s first major printed books, the “Forty–Two–Line” Bible."
socialImage: "/media/gutenberg.jpg"
---

*The Lord is my shepherd, I lack nothing. PSLAM 23:1*



## 1.블로그 설치 필요한 용어

+ Domain(도메인)
<br>원래는 110.13.109.111과 같은 숫자로 이루어진 ip주소인데, 사람들이 기억하기 쉽도록 영문주소로 변경한 것을 의미합니다.  
나만의 블로그를 갖고 싶다면 도메인을 구입하고, 직접 ip 주소에 연결하는 과정을 거쳐야 하지만 **github에서 Github Pages 라는 서비스를 사용**할 예정입니다.   **username.github.io 라는 도메인**을 갖게됩니다. 개인적으로 도메인을 구매했다면 자신만의 도메인을 연결할 수 있습니다.

+ Depoly(배포)
<br>배포란, 그동안 개발하던 것을 인터넷상에 공개하고 local 컴퓨터 뿐만 아니라 다른 컴퓨터에서도 접근해서 볼 수 있게 하는 것을 의미합니다.

+ localhost
<br>웹상에 html, css, js를 작동시켜서 사이트를 보려면 서버가 필요합니다. aws, cafe24 등에서 서버를 빌려서 우리의 블로그 파일들을 올리고 어느 누구든지 접근할 수 있게 하는 것을 호스팅이라고 합니다. 서버를 빌렸으니 렌탈비용을 내야하는데 이런 것을 호스팅 비용이라고 하죠. (참고로 우리는 github의 서버를 통해 호스팅하고, 물론 무료입니다!)
<br>그런데 markdown으로 작성된 포스팅이 잘 나오는지 매번 github에 올린 후, 내 사이트에 접속해서 확인하기는 번거롭습니다. 또는 블로그 css를 바꾸고 싶은데, 버튼 색 하나만 바꾸더라도 잘 나오는지 확인하려면 github에 올린 후, 사이트에 접속해서 확인을 해야겠죠.
<br>이렇게 매번 복잡한 과정을 거쳐서 실제 사이트에 반영된 결과물을 보는 대신에, 수정 직후 내 컴퓨터에 서버를 띄워서 바로바로 확인할 수도 있습니다.
<br>내 컴퓨터에 서버를 띄웠기 때문에 로컬환경 이라고 말합니다.


+ git

+ npm



## 2. 블로그 설치 시작

1. 사전준비
<br>터미널에 **git**, **npm**이 설치되어 있어야 합니다.

2. Gatsby 테마 선택
<br><u>Gatsby Starter</u> 사이트에서 테마를 하나 선택해주세요. 저는 gatsby-starter-lumen 으로 하겠습니다. 테마가 다르면 설정이 조금 달라집니다. 다음의 스텝을 그대로 따라하지 못 할 수도 있습니다.

3. Gatsby Starter로 블로그 설치<br>터미널을 열어서 gatsby 명령어를 사용할 수 있도록 <u>gatsby-cli를 전역에 설치</u>합니다. 터미널에서 아래 코드를 실행합니다.
```css
npm install -g gatsby-cli
```
그다음에는 우리가 정한 theme의 source code를 가져오려고 합니다. 블로그를 만들고 싶은 directory에서 아래와 같이 실행합니다. <u>gatsby 명령어를 사용하여 blog라는 디렉토리에 블로그의 소스코드를 가져온다</u>는 의미입니다.
starter-lumen이라는 theme의 github 주소입니다.
```css
gatsby new blog https://github.com/alxshelepenok/gatsby-starter-lumen
pwd //현재 directory 위치 확인
ls -l // blog directory가 만들어 졌는지 확인
cd blog // blog directory로 이동
```
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/DAY3_1.png)

4. 내 컴퓨터(local 환경)에서 블로그 띄우기
<br>설치가 제대로 되었는지 확인해봅시다. 앞으로 블로그 글을 작성할 때마다 중간 점검을 하기 위해서는 아래 명령어로 로컬 서버를 띄워줘야합니다.
```CSS
yarn develop // 또는 gatsby develop
```
아래와 같이 나오면 성공!
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/DAY3_2.png)
이제 브라우저(크롬)를 열고 주소창에 localhost:8000으로 접속해보세요. 아래와 같은 화면이 떠야합니다. 제가 선택한 테마의 메인 화면입니다.
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/DAY3_3.png)

<u>블로그를 작성하는 도중에 생각대로 markdown이 잘 적용됐는지, 아닌지를 확인하려면 내 컴퓨터에서 yarn develop 명령어로 서버를 띄워서 localhost:8000으로 접속하면 됩니다.</u>
<br><br>위 방법으로 서버를 띄우지 않으면 localhost:8000 로 중간 결과물을 볼 수가 없습니다. 또한 로컬서버가 켜있는 터미널을 닫으면 서버가 종료되므로 더이상 localhost:8000에 접속할 수 없고요. 포스팅 작성 시작할때부터 배포 직전까지 로컬서버를 띄우고 터미널을 계속 열어두면 됩니다. <u>만약 멈추고 싶으면 서버를 실행한 터미널에서 ctrl c를 입력</u>해보세요.

5. github.io repo 만들기
<br>자신의 github에 소스코드를 올리기 위해서 github에 접속해서 새로운 repository(이하 repo)를 만들어 주세요. 그런데 <u>repo 이름에 유의하셔야 합니다!</u> 아래와 같이 github의 username 뒤에 .github.io 을 붙여서 만들어주세요. 제 github username은 **lordmyshepherd**이기 때문에 **lordmyshepherd**.github.io 으로 만들었습니다.
<br><br> 이제 lormyshepherd@github.io의 도메인으로 접속할 수 있는 블로그 repository가 생성뵈었습니다!!

<br>6. Gatsby config 파일 수정
<br>**config.js** 파일 찾기 --> title,subtitle,author를 내 정보로 수정 --> url은 4번에서 만들어진 도메인 주소를 쓰면 되는데 https://깃헙유저네임.github.io/ 입니다.
<br>**package.json** 파일 찾기 (src/package.json) --> script 아래 depoly 수정
이렇게..
```css
"deploy": "yarn run clean && gatsby build && gh-pages -d public -b master",
```
이제 포스팅 작성을 완료하고, 로컬에서 잘 확인했으면 <u>yarn deploy로 배포</u>합니다. 이때, 소스코드 빌드 후의 public 폴더를 github master 브랜치에 푸시한다는 뜻이고, 이 소스코드로 **lordmyshepherd**.github.io/에 배포됩니다.

6. 배포하기
<br>로컬에 있는 블로그 소스코드를 github에 올리겠습니다.
<br>(1)blog root에서 git을 세팅합시다. lordmyshepherd 부분은 각자의 username으로 바꿔주세요.
```css
git init
git remote add origin https://github.com/lordmyshepherd/lordmyshepherd.github.io.git
``` 
origin git과 잘 연결되어도 아무 반응이 없습니다. 아래의 명령어로 origin 주소가 제대로 나오는지 확인해주세요.
```css
git remote -v
```
(2)push 하기
```css
git add .
git commit -m "first commit"
git push origin master //master는 branch가 master로 되어있다는 의미.
```
github에 가셔서 소스코드가 잘 올라왔는지 확인해보세요.
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/DAY3_4.png)
해당 소스는 포스팅을 작성할 수 있고, <u>커스터마이징 할 수 있는 개발 소스가 아니라 블로그 사이트에 배포될 수 있도록 md 파일이 모두 html, js로 바뀐 빌드된 파일</u>입니다.
(3)depoly 하기
```css
yarn deploy
```
yarn deploy 라는 명령어가 알아서 build도 해주고, git의 master에 push도 해주고 배포도 해준것입니다.

(3)브랜치 생성 및 이동
앞으로 항상 deploy만 해도 되지만, 혹시 컴퓨터를 바꾸거나 다른데서 포스팅 작성을 할 경우를 대비하여 <u>개발코드도 보존하기로 하죠. develop이라는 브랜치를 따로 만들어서 여기에다만 올리도록 하겠습니다.</u>
```css
git branch develop // 브랜치 생성
git checkout develop // 브랜치 이동
```
소스코드를 git에 올리는 과정
```css
git add .
git commit -m “blog posting~~”
git push -u origin develop //branch가 develop으로 바뀜
```
내 github 의 develop repo에 들어가서 소스코드가 모두 잘 올라갔는지 확인해주세요.
<br>잘 올라갔다면 github default 브랜치를 develop으로 바꾸겠습니다. 어차피 빌드 결과물인 master 브랜치의 빌드 파일들을 파악할 필요도 없으니까요. github의 해당 블로그 repo 페이지에 들어가서 Settings -> Branches 메뉴에서 Default branch 를 develop으로 바꾸고 update 버튼을 눌러주세요!



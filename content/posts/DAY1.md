---
title: "CSS Position"
date: "2019-00-30T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/DAY1/"
category: "CSS"
tags:
  - "Linotype"
  - "Monotype"
  - "History of typography"
  - "Helvetica"
description: "#Position, #Block and inline, #float"
socialImage: "/media/image-0.jpg"
---

*The Lord is my shepherd, I lack nothing. PSLAM 23:1*

## 1. Position <br>  relative, absolute, fixed

- 왜 position property을 배울까요?  
css의 position property를 이용하면 html 코드와 상관 없이 원하는 위치에 요소를 넣을 수 있다.

 - relative (property value)  
relative 자체로는 특별한 의미가 없습니다. 딱히 어느 위치로 이동하지는 않습니다.   
위치를 이동시켜주는 top, right, bottom, left 프로퍼티가 있어야 **원래의 위치**에서 이동할 수 있습니다.  
<br>
예를 들어서, 아래와 같이 HTML 파일을 만들었다고 생각해봅시다.

```HTML
<div>div</div>;
<div class = "relative">div.relative</div>;
<div class = "relative" "top-20">div.relative.top-20</div>;
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **각 element의 원래 위치가 어디인가???**  
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;첫번째 빨간점 = div의 원래 위치  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;빨간점 = div.relative의 원래 위치  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;세번째 빨간점 = div.relative.top-20의 원래 위치  
<br>
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/DAY1_1.png)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;원래 위치를 알았으니 relative 속성을 이용해서 div.relative.top-20 element를<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;이동시켜 보죠.
```css
.top-20{
  top:-20px;
  left:30px;
  }
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;div.relative.top-20은 위로 20px 이동하고, 왼쪽에서 30px만큼 떨어졌습니다. 
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;마이너스 값을 주면 아래로 떨어지지 않고, 위로 올라가게 됩니다.


- absolute<br>
position의 속성값 중에 하나인 absolute는 이름과 같이 요소를 절대적인 위치에 둘 수 있다. 어떤 기준으로 절대적이냐 하면, **특정 부모에 대해 절대적으로** 움직이게 됩니다.<br>
<br>부모 중에 position이 relative, fixed, absolute 하나라도 있으면 그 부모에 대해 절대적으로 움직이게 됩니다. <u>일반적으로 absolute를 쓸 경우, 절대적으로 움직이고 싶은 부모에게 position: relative; 를 부여</u>하면 됩니다.

```html
<div class="relative">
      <input type="text" placeholder="검색어 입력">
      <img class="absolute position" src="https://s3.ap-northeast-2.amazonaws.com/cdn.wecode.co.kr/icon/search.png">
    </div>
 ```   

 ```css
 .relative{
  width: 300px;
  position: relative;
}

.absolute{
  width: 17px;
  position: absolute;
  margin : 0;
}
.position{
  top :10px;
  right:12px;
}
```

- fixed<br>
fixed는 말그대로 지금 보고있는 브라우저 화면 크기만큼, 화면 내에서만 움직입니다.
화면을 스크롤을 움직여도 fixed값이 적용된 element는 그대로 있게 됩니다.  
fiexd 값의 특징은 <u>absolute는 relative를 가진 부모가 필요했는데, fixed는 필요없습니다.</u>
아래 코드를 보시죠.

```html
<header class="fixed">
      <img class="absolute" src="https://www.apple.com/ac/globalnav/4/en_US/images/globalnav/apple/image_small.svg" width="20" height="48">
    </header>
```
```css
.coupon {
position: fixed;
right: 0;
bottom: 0;
background-color: red;
color: white;
font-size: 20px;
}
```
right: 0; //브라우저의 오른쪽에서 0 만큼 떨어져있다는 뜻.<br>
bottom: 0 //하단에서 0 만큼 떨어져있다는 뜻.<br>
아래 그림에 보시는 것처럼 왼쪽 하단에 이미지가 고정되게 됩니다.
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/DAY1_2.png)

## 2. block, inline, inline-block (property value)

- block<br>
: block 요소의 의미는, 이 요소 바로 옆(좌우측)에 다른 요소를 붙여넣을 수 없다는 뜻입니다.
```html
<h1>, <h2>, <div>, <ol>, <ul>, <p>.. // block element 종류
```

- inline<br>
 : inline이라는 말 그대로 inline 요소는 요소끼리 서로 한 줄에, 바로 옆에 위치할 수 있다는 뜻입니다. block 요소와 성질이 반대인
```html
<span>, <a>, <img> // inline element 종류
```

- inline-block<br>
: block 요소의 성질을 가진 element를 css를 사용하여 inline 성질을 갖도록 하는 'property value'입니다. 적용 방법은 아래와 같습니다. 
```html
<p> // 원래 block element입니다.
```
``` css
p{
display : inline-block;
}
```

## 3. float

+ 왜 float 속성을 배울까요?<br>
: 페이지 전체의 레이아웃을 잡을 때 중요한 도구로 사용됩니다.  

+ float 속성 사용법<br>
: element의 float 속성값(property value)이 left이면 그 요소는 부모 요소의 가장 좌측에 배치됩니다.
(1)
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/DAY1_3.png)
(2)
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/DAY1_4.png)
+ float 속성에 대한 궁굼증..<br>
2번 그림에서 float 속성이 적용된 element의 <u>부모 element의 높이가 왜 0이 되는걸까요??</u>
그것은 float이 만들어진 목적에 따로 있기 때문이다.<br>
float는 본질적으로 가로배치 전용 속성이 아니다. 원래는 아래 사진과 같이 이미지와 내용이 어우러지게  하기 위해서 만들어진 속성이다.
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/DAY1_5.png)
그래서 사실은 사용법만 알고 넘어가도 됩니다. 하지만 원리가 궁굼하시면 좀 더 읽어보세요.
<br>완전히 주관적인 생각입니다.. 한번 3차원적으로 생각해볼게요.
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/DAY1_6.png)
(1)float이 적용된 element들은 위로 붕 뜨면서 element의 속성값이 block에서 inline으로 바뀌게 되고, 그 결과 왼쪽으로 순차적으로 붙는 결과가 나오는 것입니다. <br>(2)float된 element 다음에 있는 요소가 생긴 빈공간을 채우게 되면서 다음과 같은 결과가 나오게 됩니다.
부모 입장에서 자식들이 없어져서 속상합니다.<br>(3)그래서 자식들이 돌아올 때를 대비해서 다른 형제 element들에게 '그 방은 이제 네가써 하지만 그 방안에 형들 침대는 그대로 놓고 남은 공간에 네 짐을 놓으렴'이라고 규칙을 알려주는 것 같다.

+ clear 속성 - float을 이용하여 layout만들때 사용하는 방법 1 <br>

clear 속성을 가진 element는 float 속성의 영향을 받지 않게 됩니다. 다시 말하면 #red, #green, #blue이 float 되었다는 사실을 모르고 여전히 그 자리에 있는 줄 알고 #가 아래로 내려가게 되는 것입니다.
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/DAY1_7.png)


```css

//#yellow에 clear 속성 부여을 부여 합니다.
clear : left ; //왼쪽을 비우라는 의미

```
<br>
+ clearfix 핵 - float을 사용할 경우 나타날 수 있는 문제를 해결하는 방법<br>
<br>

```css
img {
  float: right;
}
```
문제 발생
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/DAY1_8.png)

해결방법

```css
.clearfix {
  overflow: auto; //적용
  zoom: 1; //적용
}
```
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/DAY1_9.png)


+ overflow - float을 이용하여 layout만들때 사용하는 방법 2 
<br>주로 많이 사용하는 방법인데 바깥 div(.wecode-box)에 overflow : hidden; 을 주는 것입니다. 
float속성을 가진 element의 부모 element에 overflow 속성을 주는 것이다.

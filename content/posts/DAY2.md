---
title: "<DAY 2> Javascript"
date: "2019-10-01T23:46:37.121Z"
template: "post"
draft: false
slug: "/posts/perfecting-the-art-of-perfection/"
category: "Design Inspiration"
tags:
  - "Handwriting"
  - "Learning to write"
description: "#Array 조작하는 방법, #예제 관련 Error"
socialImage: "/media/image-2.jpg"
---

*The Lord is my shepherd, I lack nothing. PSLAM 23:1*

## 1. Array 조작방법

빈 배열에 요소를 추가해주는 방법

1. index로 접근하는 방법(수정,추가)

```js
let cities =[]; //빈 배열 할당
cities[0] = "서울";
cities[1] = "대전";
cities[2] = "대구";
```

2. push/unshift 함수 (추가)
<br>: push와 unshift 함수는 요소를 추가해주는 함수입니다.

```js
let cities = [];
cities.push("경주", "전주");
cities.unshift("인천");
```

3. pop 함수 (제거)<br>
: pop 함수를 사용하면  요소가 제거되고,  마지막 요소의 값을 반환합니다.
```js
cities.pop();
```
####합쳐서 보면 아래와 같습니다.
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/DAY2_1.png)


## 2. Error -  Unreachable "if" after "return"
Array 관련 예제

1. addFirstAndLast 함수 안ss에 작성해주세요.
2. addFirstAndLast 함수에 주어진 인자 myArray는 숫자 값으로만 이루어진 array 입니다.
3. addFirstAndLast 함수에 주어진 인자 myArray 의 첫번째 element와 마지막 element의 값을 더한 값을 리턴해주세요.  
4. 만일 myArray에 한 개의 요소만 있다면 해당 요소의 값을 리턴해 주시고 요소가 없는 비어있는 array라면 0을 리턴해주세요.


1~9행은 문제를 1,2,3,4를 순서대로 풀었을 때 unreachable "if" after "return" 오류가 발생했다.  
오류를 해석해보면 "'return' 다음에 'if'에 도달할 수 없다"라는 의미인 것 같다.   
여기서, <u>return은 함수 내에서 사용할 경우에 return 뒤에 따라오는 값을 함수의 결과로 반환한다</u>. 
동시에 함수를 종료 시킨다. 이런 이유 때문에 if에 도달할 수 없었던 것 같다.

11~19행처럼 코드를 수정하면 잘 실행되는 것을 확인 할 수 있었다.

![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/DAY2_3.png)

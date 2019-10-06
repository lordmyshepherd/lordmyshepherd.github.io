---
title: "<DAY 5> Javascript"
date: "2019-10-04T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/humane-typography-in-the-digital-age/"
category: "Typography"
tags:
  - "Design"
  - "Typography"
  - "Web Development"
description: "#class, #arrow function"
socialImage: "/media/42-line-bible.jpg"
---
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
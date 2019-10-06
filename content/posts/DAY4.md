---
title: "<DAY 4> Javascript"
date: "2019-10-03T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/humane-typography-in-the-digital-age/"
category: "Typography"
tags:
  - "Design"
  - "Typography"
  - "Web Development"
description: "#Object, #Object-다시 :)"
socialImage: "/media/42-line-bible.jpg"
---
## 1. Object

1. Object가 왜 필요할까요?
배열에는 데이터를 저장할때 순서대로 데이터를 저장하게 됩니다. 하지만 배열은 여러가지 불편한 문제가 있습니다.
<br>객체는 순서가 없이 데이터를 저장할 수 있고 원하는 데이터에 이름을 붙여서 필요할때마다 바로 불러올 수 있습니다.

2. 객체는 어떻게 생겼는지 볼까요?
객체가 저장한 데이터를 **property**라고 합니다.  
배열과 다른점에 이 property에 이름과 값을 지정해준다는 사실이에요.  
아래에 보시는 것과 같이 <u>프로퍼티의 이름은 key</u>라고 하며, <u>프로퍼티 값은 value</u>라고 부릅니다.
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/DAY4_1.png)  

3. 객체의 규칙
- property 이름은 중복될 수 없다.
- property를 추가할 때는 ,(쉼표)를 붙여준다.
- key와 value 사이에 :(콜론)으로 구분한다.
- porperty value로는 어떤 data type도 가능하다.<br>(string, number, array, object, function ..)

4. 객체 선언 및 사용법
- 객체 리터럴
{}를 이용해서 객체를 만드는 방법입니다.

```js
var obj = { 
  name: 'jaehee', // 멤버명을 'name' 처럼 인용부호로 감싸는 것도 가능 
  age : 10, 
  increaseAge : function (i) {
      this.age + i; 
     } 
};
```
- Object 생성자
아래 코드를 봐주세요.
```js
var obj = new Object();  // obj = {}와 같은 의미
```
new Object()를 선언하게되면 빈 객체가 만들어지고 그 만들어진 객체는 변수 obj에 담기게 됩니다. 그 후에 5번에서 배우겠지만, 아래와 같이 빈 객체에 property를 넣어줍니다.
```js
var obj = {}; 
obj.name = 'jaehee'; 
obj.age = 10; 
obj.increaseAge = function (i) {
   this.age + i; 
   };
```

**객체 리터럴을 사용해 객체를 생성하는 방법은 내부적으로 new Object를 수행한 후 멤버를 구성하는 방법과 동일한 과정을 따른다.**

4. Property에 접근하는 방법
아래 예제를 통해서 알아보겠습니다.
```js
let difficult = {
  33: '숫자 형식도 되네', //아래 (2)번에 설명
  'my name': '스페이스 포함 가능',//아래 (2)번에 설명
  color: 'silver', //아래 (1)번에 설명
  키: '한글인 키는 따옴표가 없어도 되는군!!',
  '!키': '느낌표 있는 키는 따옴표가 필요하군',
  $special: '$는 없어도 되는군'
};
```
+ dot(.)과 대괄호([])로 접근하는 방법이 있습니다.

(1) dot(.)은 key로 바로 접글할 때 사용하는 것
<br>dot으로 절근할 때는 따옴표 없이 key를 바로 써줘야 합니다.
```js
console.log(difficult.color); 
```
(2) 대괄호([])
<br>프로퍼티에 space가 포함된 경우, 키는 객체에 저장할 때 키 이름을 따옴표로 감싸고 아래와 같이 대괄호로 접근하면 됩니다.
```js
console.log(difficult['my name']);
``` 
프로퍼티가 숫자로 저장된 경우도 ['']로 접근하면 됩니다.
```js
console.log(difficult['33']);
```
+ 변수로 접근하는 방법이 있을까요?
```js
let name = '키';
```
dot(.)과 []을 이용해서 접근해보겠습니다.
<br>console.log(difficult.name); --> undefined
<br>console.log(difficult[name]); --> 한글인 키는 따옴표가 없어도 되는군 !!
dot(.)으로 접근한다는 뜻은 실제 키이름을 쓸 때입니다. 변수로 접근할 때는 항상 대괄호로 접근해야 합니다.

6. property를 할당하는 방법
```js
difficult[name] = '값 바꾼다';
console.log(difficult[name]);

difficult.color = '색깔';
console.log(difficult.color);

console.log('생성전: ' + difficult.new);
difficult.new = '새로 추가된 프로퍼티';
console.log('생성후: ' + difficult.new);
```
객체에 이미 키가 존재하는데, 다시 한 번 할당하면 값이 교체(수정)됩니다.
<br>이전에 없던 키로 접근하면, 새로운 프로퍼티가 추가 됩니다.

7. Method(메서드)
객체에 저장된 값이 함수일 때, 메서드라고 부릅니다.
```js
console.log(); //log는 console 객체에 저장된 함수인 것이다!
```
객체에서 메서드를 정의하는 방법은 다음과 같습니다.
```js
let methodObj = {
  do: function() {
    console.log('메서드 정의는 이렇게');
  }
}
```
호출방법은 다음과 같습니다.
```js
methodObj.do();
```
8. Nested Object(중첩된 객체)

실무에서 사용되는 객체는 거의 중첩되어 있습니다.
<br>프로퍼티 값이 객체일 수도 있고, 프로퍼티 값인 배열의 요소가 객체일 수도 있습니다.
<br>예제를 살펴보겠습니다.
```js
let nestedObj = {
  type: {
    year: '2019',
    'comment-type': [{
      name: 'simple'
    }]
  }
}
```
위에서 'simple'을 출력하려면 어떻게 해야 될까요?
```js
console.log(nestedObje.type['comment-type'][0].name);
```

9. 객체는 reference로 저장된다?

(1)복제
우선 원시데이터 타입이 변수에 어떻게 저장되는지 살펴볼게요.
```js
let a = 1;
let b = a;
b = 2;
console.log(a); // 결과 1
```
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/DAY4_3.png) 

원시데이터 타입은 위에 방식 처럼 변수에 데이터가 저장됩니다.
<br>**데이터를 복제해서 저장**하는 방식입니다. 그렇기 때문에 값을 변경한 것은 변수 b이기 때문에 변수 a에 담겨있는 값은 그대로인 것입니다.

(2)참조
<br>객체는 reference가 저장됩니다.. 음 무슨말일까요?
<br>객체를 변수에 저장하면 객체 리터럴 자체가 저장되는 것이 아니라 reference가 저장됩니다.

```js
let a = {'id':1};
let b = a;
b.id = 2;
console.log(a.id); //는 결과 1이 아니라 2가 된다.. 복제와 다른 개념인 것 같다
```
아래 그림이 이해를 도와줄것입니다.
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/DAY4_4.png)
변수에 담겨 있는 값이 객체인 경우 b = a라고 했을 때, b와 a는 똑같은 객체를 바라보게 됩니다. 하지만 값이 원시데이터 타입인 경우 b =a라고 하면 순간 a에 담겨있던 값이 복제되어 새로만들어져 b에 담기게됩니다.

<br><br>다음 경우는 어떨까요?
<br>아래 두 객체는 들어 있는 내용이 같은데 false라고 출력이 됩니다.
<br>그 이유는 무엇일까요?
```js
const hiObj = { 
  name: '안녕' 
};
const helloObj = {
  name: '안녕'
};
console.log('객체비교 =>', hiObj === helloObj);
```
그 이유는 객체는 변수에 저장할 때, 객체 자체를 그대로 저장하는 것이 아니기 때문입니다. 객체가 담긴 어느 메모리의 reference 를 저장하기 때문입니다.
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/DAY4_2.png)
hiObj가 갖고 있는 진짜 값은 메모리 주소인 reference입니다.
<br>하지만 hiObj를 불러올 때 메모리 주소를 반환하는 것이 아니라,
<br>해당 메모리에 저장된 데이터를 반환해 줍니다.


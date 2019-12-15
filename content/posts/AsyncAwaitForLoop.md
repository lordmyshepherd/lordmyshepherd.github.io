---
title: "JavaScript async and await in loops"
date: "2019-12-15T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/AsyncAwaitForLoop/"
category: "Django"
tags:
description: "#동시성을 활용한 비동기 호출 제어"
socialImage: ""
---
*The Lord is my shepherd, I lack nothing. PSLAM 23:1*

### Preparing an example
+ 과일 바구니에서 과일 수를 얻고 싶다고 가정 해 봅시다.

    ```javascript 
    const fruitBasket = {
    apple: 27, 
    grape: 0,
    pear: 14 
    }

    const getNumFruit = fruit => {
    return fruitBasket[fruit]
    }

    const numApples = getNumFruit('apple')
    console.log(numApples) // 27
    ```

### Await in a for loop
과일 바구니에서 얻고 싶은 과일이 여러 개 있다고 가정 해 봅시다.

```javascript
    const fruitsToGet = ['apple', 'grape', 'pear']
```
이 배열을 반복 할 것입니다.
```javascript
    const forLoop = async _ => {
    console.log('Start')

    for (let index = 0; index < fruitsToGet.length; index++) {
        // Get num of each fruit
    }

    console.log('End')
}
```
for-loop에서 getNumFruit를 사용하여 각 과일의 수를 가져옵니다. 또한 번호를 콘솔에 기록합니다.  
getNumFruit는 promise를 반환하므로 해결 된 값을 로깅하기 전에 기다릴 수 있습니다.

```javascript
    const forLoop = async _ => {
    console.log('Start')

    for (let index = 0; index < fruitsToGet.length; index++) {
        const fruit = fruitsToGet[index]
        const numFruit = await getNumFruit(fruit)
        console.log(numFruit)
    }

    console.log('End')
}
```
대기를 사용하면 대기 약속이 해결 될 때까지 JavaScript가 실행을 일시 정지 할 것으로 예상합니다.  
이것은 for-loop에서 대기가 직렬로 실행되어야 함을 의미합니다.<br>
당신은 아래와 같은 결과를 기대할 것이고, 결과가 기대한대로 나올 것 입니다.
```javascript 
    'Start'
    'Apple: 27'
    'Grape: 0'
    'Pear: 14'
    'End'
```

### Await in a forEach loop
for-loop 예제에서와 동일한 작업을 수행합니다. 먼저 과일 배열을 반복합니다.
```javascript
    const forEachLoop = _ => {
    console.log('Start')

    fruitsToGet.forEach(fruit => {
        // Send a promise for each fruit
    })

    console.log('End')
    }
```
다음으로 getNumFruit로 과일 수를 구하려고합니다.  
(콜백 함수에서 async 키워드에 유의하십시오. await는 콜백 함수에 있으므로이 async 키워드가 필요합니다.)
```javascript
    const forEachLoop = _ => {
    console.log('Start')

    fruitsToGet.forEach(async fruit => {
        const numFruit = await getNumFruit(fruit)
        console.log(numFruit)
    })

    console.log('End')
    }
```
당신은 아래와 같은 결과를 기대할 것 입니다.
```javascript
//기대 결과
'Start'
'27'
'0'
'14'
'End'
```
그러나 실제 결과는 다릅니다. JavaScript는 forEach 루프의 약속이 해결되기 전에 console.log ( 'End')를 호출합니다.
```javascript
//실제 결과
'Start'
'End'
'27'
'0'
'14'
```
이 동작은 while 및 for-of 루프와 같은 대부분의 루프에서 작동합니다.  
그러나 콜백이 필요한 루프에서는 작동하지 않습니다. 폴 백이 필요한 루프의 예로는 forEach, map, filter 및 reduce가 있습니다.
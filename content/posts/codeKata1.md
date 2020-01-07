---
title: "JavaScript code kata"
date: "2019-10-10T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/codeKata1/"
category: "Javascript"
tags:
description: ""
socialImage: ""
---
*The Lord is my shepherd, I lack nothing. PSLAM 23:1*

### 1. Question
The new "Avengers" movie has just been released! There are a lot of people at the cinema box office standing in a huge line. Each of them has a single `100`, `50` or `25` dollar bill. An "Avengers" ticket costs `25 dollars`.
Vasya is currently working as a clerk. He wants to sell a ticket to every single person in this line.
Can Vasya sell a ticket to every person and give change if he initially has no money and sells the tickets strictly in the order people queue?
Return `YES`, if Vasya can sell a ticket to every person and give change with the bills he has at hand at that moment. Otherwise return `NO`.
**Examples**
```
tickets([25, 25, 50]) // => YES 
tickets([25, 100]) // => NO. Vasya will not have enough money to give change to 100 dollars
tickets([25, 25, 50, 50, 100]) // => NO. Vasya will not have the right bills to give 75 dollars of change (you can't make two bills of 25 from one of 50)
```
### 2. My Solution
```javascript
function tickets(peopleInLine){
  let cashBox = {
    25:0,
    50:0,
    100:0
  }
  
  for (let i = 0 ; i < peopleInLine.length ; i++){
  
    if(peopleInLine[i] === 25){
      cashBox[25]++;
    }
    
    else if(peopleInLine[i] === 50){
      if(cashBox[25] !== 0){
        cashBox[25]--;
        cashBox[50]++;
      }
      else return "NO";
    }
    
    else if(peopleInLine[i] === 100){
      if(cashBox[25] !== 0 && cashBox[50] !== 0){
        cashBox[25]--;
        cashBox[50]--;
        cashBox[100]++;
      }
      else if(cashBox[25] > 2){
        cashBox[25] = cashBox[25] - 3;
        cashBox[100]++;
      }
      else return "NO";
    }
  }
  return "YES";
}
```
### 3. Methods for Best Solution
[*function*.**bind**(thisArg)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)<br>
[*array*.**every**(callback)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/every)
### 4. Best Solution
```javascript
function Clerk(name) {
  this.name = name;
  
  this.money = {
    25 : 0,
    50 : 0,
    100: 0 
  };
  
  this.sell = function(element, index, array) {
    this.money[element]++;
    switch (element) {
      case 25:
        return true;
      case 50:
        this.money[25]--;
        break;
      case 100:
        this.money[50] ? this.money[50]-- : this.money[25] -= 2;
        this.money[25]--;
        break;
    }
    return this.money[25] >= 0;
  };
}
function tickets(peopleInLine){
  var vasya = new Clerk("Vasya");
  return peopleInLine.every(vasya.sell.bind(vasya)) ? "YES" : "NO";
}
```

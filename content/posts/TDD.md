---
title: "TDD(Test Driven Development)"
date: "2020-01-08T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/TDD"
category: ""
tags:
description: "TDD, BDD"
socialImage: "/media/42-line-bible.jpg"
---
*The Lord is my shepherd, I lack nothing. PSLAM 23:1*

### 목표
TDD(테스트 주도 개발)에 대해서 알아보도록 하자.

+ TDD란 무엇인가?
+ TDD를 적용해야되는 이유가 무엇인가?
+ TDD를 활용하기 어려운 이유와 잘 하는 방법

### TDD란 무엇인가?
쉽게 설명하면 테스트가 개발을 이끌어 나간다는 의미이다.
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/TDD_cycle.png)

즉, 진짜로 구현하고자 하는 코드를 짜기 전에 test 코드를 작성하는 것이다. 처음에는 test를 실행해도 구현되있는 코드가 없기 때문에 실패하게 된다.  
이 test를 통과하도록 코드를 작성하는 것이 TDD의 개념이다.

1. 구체적인 행동 레벨에서의 TDD의 개념
<br>  
테스트를 먼저 만들고 테스트를 통과하기 위한 것을 짜는 것이다.백앤드 API 앤드포인트를 만든다고 생각해보자. 만드 과정에서 우선 테스트 코드를 작성하고 이를 통과하는 엔드포인트 코드를 만들고를 반복하면서 제대로 동작하는지에 대한 피드백을 적극적으로 받는 것이다.

   + 보통은 SW 개발을 할 때 코딩을 다 끝내고 난 후 테스트를 한다.   
   이것의 순서를 바꾸는 것이 TDD를 적용하는 것이다.
   + TDD 적용 사례  
      예를 들어, 생년월일(input)을 입력받으면 현재 나이(output)를 출력하는 프로그램을 만든다고 생각해보자.  
      1. 처음에는 간단한 것으로 목표를 정한다. (태어난 해와 올해의 연도를 입력) 2015, 2018 -> (만)3살 우선 이것을 만들겠다는 생각을 한다.
      2. 만들기도 전에 만든 후에 무엇을 테스트할지를 설계한다.  
2015, 2018를 입력하면 2가 나오는 테스트 프로그램(장차 만들 프로그램을 테스트할 코드)를 만든다.
      3. 그 다음에 그 테스트를 통과할 프로그램(1.을 목표로 작성한 코드)를 만든다.  
      올해의 연도 - 태어난 해  
      2018 - 2015
      4. 테스트 프로그램으로 이 프로그램(3.에 해당하는 코드)을 실행한다.  
      5. 통과했으면 새로운 테스트를 추가한다.  
      이번에는 생월을 추가했을 때 계산하는 프로그램
      위와 같은 작업을 계속 왔다갔다 수행한다.

2. 추상적인 레벨에서의 TDD의 핵심 개념 (중요)
<br>  
결정(decision)과 피드백(feedback) 사이의 갭을 조절하기 위한 테크닉이라고 할 수 있다.
   + 익스트림 프로그래밍의 창시자 켄트 벡(Kent Beck)은 **TDD란** 결정과 피드백 사이의 갭에 대해 인식하고 갭을 조절하기위한 테크닉이라고 말한다.

   + 결정(decision)이란?  
   프로그램을 하다보면 ‘이 방법으로 해야지’, ‘이 부분은 이걸 이용해서 짜야지’라는 것을 결정한다.
   + 피드백(feedback)이란?  
   프로그램을 하다보면 성공/실패(에러)라는 피드백을 받는다.
   + 결정과 피드백 사이에 갭이 생긴다!  
   (1) 결정 : 2015, 2018을 입력받아 현재 나이를 출력하는 코드를 목표를 가진 코드를 작성할 때, '나는 빼기로 나이를 구해야겠다.'라는 것을 결정한다.  
   (2) 피드백 : 빼기로 계산했을 때의 코드를 테스트 프로그램을 실행한 결과로, 된다/안된다라는 프로그램 상의 피드백을 받는다.  
   (3) 이 둘 사이의 갭을 내가 인식하고 갭을 조절한다.

### TDD를 왜 적용해야할까?

  ![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/tdd-collaboration(2).png)

1. 높아진 SW 개발의 불확실성 때문에 '피드백'과 '협력'이 중요하다.

   피드백과 협력이 중요한 이유는 불확실성이 높을 때 '피드백'과 '협력'을 이용하면 더 좋은 결과가 나올 확률이 높아진다.  
   **TDD도 마찬가지로 '피드백'과 '협력'을 증직시키는 것이기 때문에 불확실성이 높을 때 도움이 되는 것이다.**

2. TDD의 효과
모든 애자일의 개발 방법론은 피드백과 협력을 동시에 증진시킨다.  

   (1) 피드백  
   TDD를 하면 테스트를 통과하는 것으로 잘되고 있는가를 자주 확인할 수 있으므로 피드백이 증가한다.  

   (2) 협력(중요!)  
   테스트 코드에는 개발자의 의도(고민,결정)가 담겨있다. 그렇기 때문에 팀원들과 테스트 코드를 공유하면 협력이 증진된다.  
      + 남이 짠 코드를 쉽고,빠르게 이해할 수 있다.
      + 내가 남의 코드를 수정해야될 상황이 오더라도... 자동화된 테스트가 알려주기 때문에 걱정이 줄어들 수 있다.
      + 왜 이렇게 짰을까 궁굼할 때 'test'코드를 보면 이해하기 쉽다.

### TDD의 장단점
TDD를 적용하게 되면 피드백과 협력을 증진시키기 때문에 불확실성에 대해 대비를 하게 해주지만, 처음부터 2개의 코드를 짜야하고, 중간중간 테스트를 하면서 고쳐나가야 하기 때문에 개발 속도가 느려진다고 생각하는 사람도 많다.

1. 장점
   + SW 개발에서 예상하지 못했던 시간을 많이 소요하는 것은 대부분 버그 때문인데 이런 버그를 줄일 수 있다.
   + 코드의 가독성이 좋아진다.
   + 유지보수 비용이 낮아진다.

2. 단점
   + 개발 시간이 TDD를 적용하지 않을 때에 비해서 10~50% 늘어난다.

### TDD를 잘 활용하는 방법
나 스스로 ‘어떻게 해야 피드백을 더 자주 받을까’, ‘어떻게 해야 내가 하는 작업에 대해 협력이 잘 일어나게 할까’를 고민하면서 계속해서 내가 일하는 방식을 업그레이드 해야한다.  

예를 들어, 게임을 개발하면서 1,2단계를 클리어한 후 3단계를 테스트 해야한다면 테스트 비용이 증가한다.
   + "어떻게 하면 테스트 비용을 낮출 수 있을까?" 고민해보자
   + 바로 3단계를 테스트할 수 있도록 만든다.  
   백도어 접근법(테스트를 할 때 어떤 파라미터를 적용하면 내가 원하는 시스템의 시작점으로 가게하는 것)
   + 중복을 제거하면서 조금 더 자동화하려고 노력해보자

### BDD란?

BDD(Behaviour Driven Development)는 TDD와 유사한 개발 프로세스 중 하나이다!  

TDD 는 테스트를 먼저 작성하고 그 테스트를 통과시키는 코드를 작성하는 흐름을 기본으로 한다. 게다가 테스트 단위도 함수 단위로 매우 작아서 작성하는 거의 모든 함수가 테스트 대상에 포함된다. 그렇기에 현업에서 매끄럽게 사용하는 것이 힘이 들 수도 있다.  

BDD는 함수 단위 테스트를 권장하지 않고 시나리오를 기반으로 테스트 케이스를 작성한다. 이 시나리오는 개발자가 아닌 사람이 봐도 이해할 수 있을 정도의 수준이다. 하나의 시나리오는 Given, When, Then 구조를 가지는 것을 기본 패턴으로 권장하며 각 절의 의미는 다음과 같다.
```python
Feature : 테스트에 대상의 기능/책임을 명시한다.

Scenario : 테스트 목적에 대한 상황을 설명한다.

Given : 시나리오 진행에 필요한 값을 설정한다.

When : 시나리오를 진행하는데 필요한 조건을 명시한다.

Then : 시나리오를 완료했을 때 보장해야 하는 결과를 명시한다.
```

예를들면, 사용자가 회원가입하고 로그인하고 친구추가하는 행동을 한다고 하면 각 usecase에 맞는 코드가 정상적으로 실행이 되야될 것이다.  
즉, 사용자의 행동을 시나리오 기반으로 테스트 케이스를 작성하는 것이다.

원래 나온 목적은 비개발자가 "뒤로가기 버튼을 눌러라"와 같이 직접 입력해서 테스트를 할 수 있게 하기 위함이였다.
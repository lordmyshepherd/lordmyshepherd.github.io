---
title: "Clean Architecture"
date: "2019-11-01T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/clean_architecture"
category: "node.js express, typescript"
tags:
  - "Design"
  - "Typography"
  - "Web Development"
description: "#IOC, DI, Domain Driven Design"
socialImage: "/media/42-line-bible.jpg"
---
*The Lord is my shepherd, I lack nothing. PSLAM 23:1*

#### Clean Architecture가 무엇이며 왜 필요한가? 

![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/Django/clean_architecure.png)

위 그림처럼 유사한 관심들을 레이어로 나눠 분리하는 것이 Uncle Bob이 언급한 클린아키텍처입니다. 2012년도에 만들어진 개념이며 현대 트렌드에 맞게 변화가 있었지만 그 생각의 본질은 동일합니다.  
바로 각 레이어는 안으로만 의존성을 향하고 있다. 즉 단방향 의존성을 가지는 구조로 바깥 레이어로만 종속성이 있어야 합니다.  
**클린아키텍처를 기반으로 코드를 작성하면 얻을 수 있는 장점**으로는 각각의 관심사에 특화된 독립적인 테스트를 할 수 있어 코드의 테스트 가능성이 증대되고, 바깥 레이어에 포함되는 프레임워크가 바뀌어도 크게 시스템이 영향을 받지 않고, 그리고 클래스가 집중화 되어 유지보수가 쉬워집니다.

#### Clean Architecture를 적용할 경우 Data flow
Let’s start explaining Data Flow in Clean Architecture with an example.
Imagine opening an app that loads a list of posts which contains additional user information. The Data Flow would be:

1. UI(View) calls method from Controller/Presenter/ViewModel.
2. Controller/Presenter/ViewModel executes Use case.
3. Use case combines data from User and Post Repositories.
4. Each Repository returns data from a Data Source (Cached or Remote).
5. Information flows back to the UI where we display the list of posts.

From the example above we can see how the user action flows from the UI all the way up to the Data Source and then flows back down.  
**This Data Flow is not the same flow as the Dependency Rule.**


#### Dependancy Rule
Dependency Rule is the relationship that exists between the different layers. Before explaining the Dependency Rule in Clean Architecture lets rotate the onion 90 degrees. This helps to point out layers & boundaries.
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/Django/clean2.png)
Let’s identify the different layers & boundaries.

**Presentation Layer** contains UI (Activities & Fragments) that are coordinated by Presenters/ViewModels which execute 1 or multiple Use cases. Presentation Layer depends on Domain Layer.  

**Domain Layer** is the most INNER part of the onion (no dependencies with other layers) and it contains Entities, Use cases & Repository Interfaces. Use cases combine data from 1 or multiple Repository Interfaces.  

**Data Layer** contains Repository Implementations and 1 or multiple Data Sources. Repositories are responsible to coordinate data from the different Data Sources. Data Layer depends on Domain Layer.
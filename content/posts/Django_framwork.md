---
title: "RESTful HTTP API"
date: "2019-11-09T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/Django_framwork/"
category: "기본 개념"
tags:
  - "Restful API"  
description: "Django Restful API 개발(2) - 기본 개념"
socialImage: ""
---
*The Lord is my shepherd, I lack nothing. PSLAM 23:1*
# API 앤드포인트 아키텍처 패턴

### API 엔드포인트 구조를 구현하는 방법
+ REST 방식
+ GraphQL 방식

### RESTful HTTP API
REST(Representation State Transfer)ful의 약자로 REST 기반으로 API 시스템을 구현하기 위한 아키텍처의 한 방식이다.  
<br>HTTP URI(Uniform Resource Idntifier)를 통해 특정 Resurce(Database에 저장되어 있는 정보)의 위치를 나타내고, HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한 행위(CRUD Operation)를 정의하고 Payload로 데이터를 주고 받는다.  
<br>
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/Django/restful_api.png)

### RSET 구성 요소
   1. 자원(Resource): URI  
       + 모든 자원에 고유한 ID가 존재하고, 이 자원은 Server에 존재한다.  
       + 자원을 구별하는 ID는 ‘/groups/:group_id’와 같은 HTTP URI 다.  
       + Client는 URI를 이용해서 자원을 지정하고 해당 자원의 상태(정보)에 대한 조작을 Server에 요청한다.  
   2. 행위(Verb): HTTP Method  
       + HTTP 프로토콜의 Method를 사용한다.  
       + HTTP 프로토콜은 GET, POST, PUT, DELETE 와 같은 메서드를 제공한다.
   3. 표현(Representation of Resource)
       + Client가 자원의 상태(정보)에 대한 조작을 요청하면 Server는 이에 적절한 응답(Representation)을 보낸다.
       + REST에서 하나의 자원은 JSON, XML, TEXT, RSS 등 여러 형태의 Representation으로 나타내어 질 수 있다.
       + JSON 혹은 XML를 통해 데이터를 주고 받는 것이 일반적이다.

### RESTful API 개발시 주의사항
+ /는 계층관계를 나타낼때 사용된다.  
예) https://stackoverflow.com/c/wecode/questions/197 라는 구조라면, wecode에 속해있는 questions중에 197을 나타내는 구조이다.  
예) https://api.shopping.com/books/novel/stephenking 라는 구조라면, 책들중 소설 그리고 소설중 Stephen King의 소설을 나타내는 구조이다.
+ URI에 _(underscore)는 주로 포함하지 않고 또한 영어 대문자보다 소문자를 쓴다. 가독성을 높이기 위해서 긴 단어는 사용하지 않는다.
+ URI는 명사를 사용한다.  
예) books/novel/stephenking 라고 하지 books/novel/stephenking 라고 잘 하지 않는다. 그 이유는 이미 HTTP Method에서 행위를 결정하지 때문이다.  
한 문장에 동사가 두 번 들어지 못하는 것처럼~

### RESTful API의 장점
+ 자기 설명력(self-descriptiveness) : 엔트포인트의 구조만 보더라도 해당 엔드포인트가 제공하는 리소스와 기능을 파악할 수 있다.
+ API를 구현하다 보면 엔드포인트 수가 많아지면서 역할과 기능을 파악하기가 쉽지 않은데 REST 방식은 직관적이다.

### RESTful API의 단점
+ 사용할 수 있는 메소드가 4가지 밖에 없다.(HTTP Method 형태가 제한적)
+ 구형 브라우저에서는 PUT, DELETE를 사용하지 못한다.

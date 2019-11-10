---
title: "WEB, HTTP"
date: "2019-11-08T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/web_system/"
category: "기본 개념"
tags:
  - "Web History"
  - "HTTP"  
description: "#Django Restful API 개발(1)- 기본 개념"
socialImage: ""
---
*The Lord is my shepherd, I lack nothing. PSLAM 23:1*
# 웹 시스템들의 발전 역사


### 1세대 웹
+ 1989년에 팀 버너스 리(Time Berners_Lee)가 WWW(World Wide Web)을 발명했다.
+ 웹 브라우저가 요청하면 HTML 페이지를 보내주고, 웹 브라우저는 전달받은 HTML 파일을 렌더링하여 사용자에게 보여주는 정도(static).

### 2세대 웹
+ Javascript의 역할이 커지기 시작했다.
+ 웹 서버가 적적인 HTML뿐만 아니라 동적인 기능을 구현하는 Javascript code까지 같이 웹브라우저에 전송하면서 동적인 기능을 구현하게 되었다.
서비스가 정적(static)에서 동적(dynamic)으로 변화되어 가면서 커져 갔지만, 동일한 서버에서 HTML, Javascript data가 전부 웹 브라우저로 XML의 구조로 전송되고 있었다.
+ 여전히 동일한 서버에서 HML, Javascript 그리고 데이터가 전부 웹브라우저 등의 클라이언트로 전송되고 있었다. 참고로 XML(Extensible Markup Language)는 데이터를 전송하기 위한 markup language로 HTML과 비슷하지만 데이터를 표현하기 위해 사용됨.

### 3세대 웹 
+ SPA(Single Page Application) 방식의 프론트엔드 개발이 인기를 끌기 시작했다.
+ 단일 페이지로 모든 웹사이트의 기능을 구현하게 된 것이다.  이렇게 단일 페이지의 자바스크립트를 통해 모든 페이지를 동적으로 구현하게 되다보니 자연스럽게 웹 브라우저가 필요한 서버와의 통신은 데이터 전송이나 생성 및 수정에 과한 것이 대부분이 되었다.
+ **사이트의 페이지를 rendering하는데 필요한 HTML,Javascript 코드는 최초의 통신에서 Frontend Server에서 받아오고, 다음부터는 Backend Server에서 데이터만 주고 받는다.** 참고로 Data 형식은 Json, XML 등으로 주고 받는다.

###현대 웹 시스템들의 구조 및 아키텍처
현대에 들어와서 웹 시스템의 규모가 더욱더 커지고 무엇보다 처리해야 하는 동시 요청 수와 데이터의 규모가 기하학적으로 증가하면서 웹 시스템들의 구조 또한 더욱 방대해지고 복잡해지고 있다.
+ MSA(Micro Servie Architechure) : 수 많은 동시 요청의 문제를 해결하기 위한 아키텍처 개념
+ ETL(Extract, Transfer, Load) : 분석해야 하는 데이터의 양이 늘어나면서 등장
+ Hadoop : 대용량 분석 프레임워크로서 "Big Data" 분석 시스템이 많은 회사들의 백엔드 시스템에 도입
+ 현대 웹 시스템에서 백앤드 개발자의 역할  
클라이언트 시스템과 데이터를 실시간으로 주고받을 수 있는 기능을 구현하는 역할을 담당해야 한다.특히 많은 수의 동시 요청을 장애 없이 실시간으로, 최대한 빠른 속도로 처리할 수 있는 시스템을 구현하는 것이 중요하다.  
그러므로 백엔드 개발에는 주로 안정적이고 확장성이 높으며 실행 속도도 높은 시스템을 구현하기 유리한 언어를 사용한다. 대표적으로 Java, Scala, Rust, Python이 있다.


# HTTP 
### HTTP란 무엇인가?
+ HyperText Transfer Protocol의 약자로서, 웹상에서 서로 다른 서버 간에 통신할 때 **서로 이해할 수 있는 형식**으로 통신하자고 규정해 놓은 통신 규약이다.
+ 웹 초기에는 정말 단순하게 HTML(하이퍼텍스트)를 전송하는데 주로 쓰였지만, 기술이 발전함에 따라서 다양한 데이터를 주고 받는데 사용된다.


### HTTP 통신 방식
HTTP 통신 방식에는 요청(request)과 응답(response) 2가지가 있고, 각 각의 통신은 stateless라는 틍징을 가지고 있다.  
+ Request
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/Django/request3.png)
   1. Start line
       + HTTP 메소드  
        해당 HTTP 요청이 의도하는 액션(동사)을 정의하는 부분이다.  
        POST, GET, PUT, DELETE, OPTIONS 등이 있다.
       + Request target  
       해당 HTTP 요청이 전송되는 목표 주소 (엔드포인트의 target 주소) 
       + HTTP version  
       해당 요청의 HTTP 버전을 나타낸다. 명시하는 이유는 버전에 따라 HTTP 요청 메시지의 구조나 데이터가 약간씩 다를 수 있기 때문이다.       
   2. Headers  
       요청 자체에 대한 정보를 담고 있다. 예) 요청 메시지의 전체 크기(Content length)  
       헤더는 파이썬의 dictionary처럼 Key와 Value로 되어 있다.
       + Host
       요청이 전송되는 target의 호스트의 URL 주소를 알려주는 헤더
       예)localhost:8000
       + User-Agent
       요청을 보내는 클라이언트에 대한 정보
       예)Httpie
       + Accept
       해당 요청이 받을 수 있는 response body의 데이터 타입을 알려주는 헤더
       API에서 자주 사용되는 MIME(Multipurpose Internet Mail Extionsions) type은 application/json
       + Connection
       통신 상태를 유지할 것인지 중단할 것인지 알려주는 헤더
       Connection:Keep-alive 연결 유지
       Connection:close 연결 해제
       + Content-Type
       HTTP 요청이 보내는 메시지 body의 타입을 알려주는 헤더
       얘)application/json
       +Content-Length
       HTTP 요청이 보내는 메시지의 body의 총 크기를 알려주는 헤더
   3. Body
       HTTP 요청이 전송하는 데이터를 담고 있는 부분
       현재 로그인을 위해서 email과 password 정보를 body에 담아서 보내고 있다.

+ Response
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/Django/response3.png)
   1. Status line
       + HTTP 메소드
       POST, GET, PUT, DELETE, OPTIONS 등이 있다.
       + **status code**  
       HTTP 응답 상태를 미리 지정되어 있는 숫자로 된 코드로 나타내준다.
           + 200 OK
           성공적으로 잘 처리됨
           + 301 Move Permanently
           HTTP 요청을 보낸 엔드포인트의 URL 주소가 바뀌었다는 것을 나타내는 status code이다.
           301 status code의 HTTP 응답은 Location(새로운 엔드포인트의 주소가 포함되어 나온다.) 헤더가 포함된다.
           + 400 Bad Request
           잘못된 요청일 때 보내는 응답 코드로, 주로 input으로 잘못된 값들이 보내졌을 때 사용된다.
           예)사용자의 전화번호를 저장하는 요청인데, 숫자가 아니라 스트링이 보내지는 경우
           + 401 Unathorized
           HTTP 요청을 처리하기 위해서는 요청을 보낸 사람의 인증이 필요할 때, 인증에 실패 했을 경우에 보내는 응답 코드이다.
           얘)신분(credential) 확인이 필요한 경우!
           + 403 Forbidden
           HTTP 요청을 보내는 사용자가 해당 요청에 대한 권한이 없을 때 나타내는 status code이다.
           예) 인터넷 강의를 결제한 사용자만 볼 수 있는데 아직 결제하지 않은 사용자가 접근할 경우 
           + 404 Not Found
           HTTP 요청을 보내고자 하는 URI가 존재하지 않을 경우 보내는 status code이다.
           + 500 Internal Server Error
           해당 요청을 처리하던 과정에서 서버에서 오류가 발생해서 요청을 처리할 수 없을 경우 사용되는 응답코드이다.

   2. Headers  
     HTTP 요청의 heares와 동일하게 응답에 대한 정보를 담고 있는 부분이다.
   3. Body  
   HTTP 요청 메시지의 body와 동일하다.

+ Stateless
HTTP 프로토콜에서는 동일한 클라이언트와 서버가 주고받은 HTTP 통신들이라도 서로 연결되어 있지 않다. 즉, 각각의 HTTP 통신은 독립적이며 그 전에 처리된 HTTP 통신에 대해서 전혀 알지 못한는 특징을 가지고 있다.  
   + 장점 : 이전에 보냈던 HTTP 통신들의 상태를 서버에 저장할 필요가 없어서 관리하지 않아도 된다.
   + 단점 : HTTP 요청을 보낼 때는 해당 요청을 처리하기 위해 필요한 모든 데이터를 매번 포함시켜서 요청을 보내야 한다.
   예)로그인한 상태일 경우에만 열람할 수 있는 페이지가 있다고 할 때, 이전 통신을 통해서 이미 로그인할 상태라고 하더라도 새로 보내는 HTTP 통신의 경우 사용자의 로그인 사실 여부를 포함시켜서 보내야된다.  
   + 해결방법 : 쿠키(coockie)나 세션(sesstion)을 사용하여 HTTP 요청을 처리할 때 필요한 데이터를 저장해 놓으면 된다.


### API 엔드포인트 아키텍처 패턴(architecure pattern)에 대해서 알아보고 Django framework를 이용해서 API를 만들어보자.
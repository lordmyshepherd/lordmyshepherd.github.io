---
title: "Django Restful API 개발(3)"
date: "2019-11-09T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/API_architecher"
category: "Python"
tags:
  - "Design"
  - "Typography"
  - "Web Development"
description: "#API 엔드포인트 architecher"
socialImage: "/media/42-line-bible.jpg"
---
*The Lord is my shepherd, I lack nothing. PSLAM 23:1*

## Django

#### 1. Django는 무엇이며, 웹 개발을 왜 Django라는 API 프레임워크를 사용하는가? 
Backend 구성
+ 웹서버 : Apachem Nginix 등..
+ 데이터베이스 : MySQL, Orcle, PostgreSQL, Mongo DB 등..
+ 스크립트 엔진 : 스크립트 언어(javascript, python 등..) 위에서 웹프레임워크가 동작할 수 있게 해주는 통역사
+ 웹 프레임워크 : PHP, JSP, ASP, Django, Ruby on Rails, Node.js, Flask 등..

여기서, 웹 프레임워크는 client의 다양한 요청에 대해서 Database에 있는 data를 동적으로 만들어주기 위해 필요한 프로그램이다.  
&nbsp;&nbsp;&nbsp;&nbsp;Django의 장점은 굉장히 쉽게 배울 수 있는 프로그램 언어인 Python을 기반으로 했고, adim 기능을 같이 제공해준다 점이다. 요즘 대부분 AWS, Google Cloud를 사용한 다는 점으로만 봐도 AWS, Google Cloud에서 전폭적으로 초기 단계부터 지원한 프레임워크인 Django는 서비스 운용에 큰 도움이 될 것이다.
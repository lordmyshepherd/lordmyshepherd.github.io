---
title: "Requests module을 통해서 json 데이터를 요청하고 받아서 파싱하기"
date: "2019-11-14T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/requests/"
category: "Django"
tags:
description: ""
socialImage: ""
---
*The Lord is my shepherd, I lack nothing. PSLAM 23:1*

### 환경 및 선수조건
+ Python
+ requests module, json module

### Requests?
+ HTTP 통신을 위한 파이썬 라이브러리로 urllib.request 처럼 with문 없이 직관적으로 사용 가능하다.

### Requests 기본 사용법
+ 기본 예제

    ```python
    >>> import requests
    >>> r = requests.get('https://api.github.com/user', auth=('user', 'pass'))
    >>> r.status_code
    200
    >>> r.headers['content-type']
    'application/json; charset=utf8'
    >>> r.encoding
    'utf-8'
    >>> r.text
    u'{"type":"User"...'
    >>> r.json()
    {u'private_gists': 419, u'total_private_repos': 77, ...}
    ```
+ 다른 Method를 사용할 때
    ```python
    >>> import requests
    >>> r = requests.get('https://api.github.com/events')
    >>> r = requests.post('http://httpbin.org/post', data = {'key':'value'})
    >>> r = requests.put('http://httpbin.org/put', data = {'key':'value'})
    >>> r = requests.delete('http://httpbin.org/delete')
    >>> r = requests.head('http://httpbin.org/get')
    >>> r = requests.options('http://httpbin.org/get')
    ```
+ .json() : json data를 HTTP 요청을 통해서 받아오고 그 data를 python에서 사용할 수 있는 dictionary 형태로 파싱하는 방법
    ```python
    import requests
    url = 'https://gist.githubusercontent.com/TWpower/771f9dfc8d9e1ddc0ecbdaea5b2e379e/raw/2c7785b4835138255bdadb71bd83702e53ac2677/test-example.json'

    data = requests.get(url).json()
    ```

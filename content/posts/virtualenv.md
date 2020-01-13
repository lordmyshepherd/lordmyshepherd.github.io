---
title: "Virtualenv MacOS 설치 및 사용법"
date: "2020-01-12T22:44:32.169Z"
template: "post"
draft: false
slug: "/posts/virtualenv"
category: ""
tags:
description: ""
socialImage: "/media/42-line-bible.jpg"
---
*The Lord is my shepherd, I lack nothing. PSLAM 23:1*

### 목표
**가상환경 설치**
   + Virtualenv란?
   + Virtualenv, virtualenvWrapper 설치[MAC OS]
   + Virtualenv의 기본 명령어(사용법)
   + VirtualenvWrapper 설정
   + VirtualenvWrapper 명령어(사용법)



### Virtualenv란?
Virtualenv란 시스템 OS에 설치된 주 python뿐만 아니라 여러 버전의 Python과 프로젝트별로 다른 종류의 라이브러리를 사용하는 것에 있어 가장 핵심된 기능을 제공합니다.

예를들어, 어떤 옛날 프로젝트에서는 Python2.7버전에 pip로 Django1.6을 사용했다고 가정해봅시다. 하지만 이번에 새로 시작하는 프로젝트는 Python3.6에 pip로 Django1.10을 사용하려고 합니다. 물론 가장 쉬운 방법은 개발 환경별로 다른 컴퓨터를 사용하는 것이지만, 공간적/금전적/편의적으로 어렵습니다.

따라서 우리는 Python실행파일과 pip로 설치된 라이브러리들을 독립된 폴더에 넣어버리는 방법을 선택할 수 있는데, 이것이 Virtualenv의 핵심입니다.

### Virtualenv 설치[MAC OS]
MAC OS에는 시스템 전역에 기본적으로 Python2가 설치되어있기 때문에 아래 명령어로 쉽게 pip를 설치할 수 있습니다.
```python
sudo easy_install pip
```
만약 sudo로 시스템 전역에 설치하기가 싫다면 HomeBrew를 이용해 Python을 유저영역에 설치할 수도 있습니다.
```python
$ brew install python
```
pip가 성공적으로 설치되었는지 확인하려면 다음 명령어로 pip의 버전을 확인해 보면 됩니다.
```python
$ pip -V
#Python3 pip의 경우에는 pip3 -V
```
만약 pip나 pip3이라는 명령어가 먹히지 않는다면 아래의 명령어로 Python의 모듈로서 pip를 호출할 수 있습니다.
```python
# Python2의 경우
$ python -m pip -V

# Python3의 경우
$ python3 -m pip -V
```
Virtualenv와 VirtualenvWrapper는 pip를 통해 설치가 가능합니다.
```python
# Python2의 경우
$ pip install virtualenv virtualenvwrapper

# Python3의 경우
$ pip3 install virtualenv virtualenvwrapper
```
만약 pip/pip3 명령이 먹지 않는다면 아래 명령어로 대체할 수 있습니다.
(시스템에 easy_install로 pip를 설치한 경우 sudo권한이 필요할 수 있는데, 이때는 sudo pip install으로 명령어 앞에 sudo를 붙여줍시다.)
```python
# Python2 pip의 경우
$ python -m pip install virtualenv virtualenvwrapper

# Python3 pip의 경우
$ python3 -m pip install virtualenv virtualenvwrapper
```

### Virtualenv의 기본 명령어(사용법)
Virtualenv는 기본적으로 아래의 명령어로 동작합니다.
1. 가상화면 만들기
```python
$ virtualenv --python=파이썬버전 가상환경이름
# ex)
# $ virtualenv --python=python3.5 test_env
# $ virtualenv --python=python2.7 test_env2
```
```python
#만약 The path x.x does not exist라는 에러가 난다면 PYTHON의 PATH을 절대경로로 맞춰줘야 합니다. which python3을 했을 때 /usr/bin/python3이 나왔다면, virtualenv --python=/usr/bin/python3와 같이 절대경로로 입력해주시면 됩니다.
```
이와 같이 Python버전을 명시해주고 가상환경을 만들 수 있습니다. (단, 선택할 Python은 시스템에 깔려있는 버전이어야 합니다.)

2. 가상환경 활성화(가상환경에 진입)
```python
$ source 가상환경이름/bin/activate
```

이후 pip를 통해 외부 모듈과 라이브러리들을 설치하는 경우, source 명령어로 가상환경에 진입하지 않으면 라이브러리들을 불러쓸 수 없게됩니다. 즉, 프로젝트 별로 다른 라이브러리만이 설치된 환경을 구성한 것이죠.

### VirtualenvWrapper 설정
VirtualEnv를 사용하기 위해서는 source를 이용해 가상환경에 진입합니다. 그러나, 이 진입 방법은 가상환경이 설치된 위치로 이동해야되는 것 뿐 아니라 가상환경이 어느 폴더에 있는지 일일이 사용자가 기억해야 하는 단점이 있습니다. 이를 보완하기 위해 VirtualenvWrapper를 사용합니다.

또한, VirtualenvWrapper를 사용할 경우 터미널이 현재 위치한 경로와 관계없이 가상환경을 활성화할 수 있다는 장점이 있습니다.

VirtualenvWrapper는 .bashrc나 .zshrc에 약간의 설정과정을 거쳐야 합니다.

1. 우선 홈 디렉토리로 이동후 가상환경이 들어갈 폴더 생성
```python
$ cd ~
$ mkdir ~/.virtualenvs
```
2. 홈 디렉토리의 .bashrc나 .zshrc의 파일 제일 마지막에 아래 코드를 복사해 붙여넣어줍시다.
(파일이 없다면 만들어 사용하시면 됩니다.)
```python
# python virtualenv settings
export WORKON_HOME=~/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON="$(which python3)"  # Usage of python3
source /usr/local/bin/virtualenvwrapper.sh
```
3. 저장하고 나온 후 터미널을 종료후 새로 켜주면, VirtualenvWrapper의 명령어들을 사용할 수 있습니다. 만약 /usr/local/bin/virtualenvwrapper.sh파일이 존재하지 않는다면 다음 명령어로 virtualenvwrapper.sh파일을 찾아서 위 코드를 바꿔 사용하세요.
```python
find /usr -name virtualenvwrapper.sh
또는
which virtualenvwrapper.sh
```

### VirtualenvWrapper 명령어(사용법)
VirtualenvWrapper의 명령어는 여러가지가 존재하지만, 이 포스팅에서는 기본적인 것만 다루고 넘어갑니다.
1. 가상환경 만들기
```python
$ mkvirtualenv 가상환경이름
# 예시
# $ mkvirtualenv test_env3
```
mkvirtualenv 명령어를 사용할 경우 홈 디렉토리의 .virtualenvs폴더 안에 가상환경이름을 가진 폴더(test_env3)가 생깁니다.
2. 가상환경 삭제
```python
$ rmvirtualenv 가상환경이름
# 예시
# $ rmvirtualenv test_env3
```
rmvirtualenv 명령어를 사용할 경우 mkvirtualenv로 만든 가상환경을 지워줍니다.  
만든 가상환경을 지우는 방법은 이방법 뿐 아니라 홈 디렉토리의 .virtualenvs폴더 안의 가상환경이름을 가진 폴더를 지우는 방법도 있습니다.
3. 가상환경 활성화(진입) 및 가상환경 목록 보기
```python
$ workon 가상환경이름
# 가상환경으로 진입시 앞에 (가상환경이름)이 붙습니다.
(가상환경이름) $
# 예시
# $ workon test_env3
# (test_env3) $
```
workon명령어를 통해 mkvirtualenv로 만든 가상환경으로 진입할 수 있습니다.  
workon명령어를 가상환경이름 없이 단순하게 칠 경우, 현재 만들어져있는 가상환경의 전체 목록을 불러옵니다.
```python
$ workon
test_env3
```
4. 가상환경 비활성화(빠져나오기)
```python
(가상환경이름) $ deactivate
# 예시
# (test_env3) $ deactivate
# $
```
가상환경에서 빠져나오는 것은 다른것들과 동일하게 deactivate명령어로 빠져나올 수 있습니다.
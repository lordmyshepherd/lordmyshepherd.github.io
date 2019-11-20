---
title: "Docker를 이용한 AWS상에 서버 구축하기"
date: "2019-11-20T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/docker"
category: "Django"
tags:
description: "#DoesNotExist vs ObjectsDoesNotExist"
socialImage: ""
---
*The Lord is my shepherd, I lack nothing. PSLAM 23:1*

### AWS(Amazon Web Service)란? 
Server 등의 인프라스트럭쳐를 필요한대로 요청하면 사용할 수 있는 서비스 ***클라우드 서비스***입니다. 유저가 직접 서버를 구입하고 설치할 필요 없이 AWS상에서 클릭 몇번으로 ***서버를 구축***하고 사용할 수 있습니다. 
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/Django/AWS.png)
[출처 : https://stackoverflow.com/c/wecode/questions/271]

### 웹서비스 배포를 위한 AWS 필수 개념
1. EC2
   + Elastic Compute Cloud
   + AWS 상에서 사용하는 Server. EC2 서버에 API를 배포하게 된다.
   + EC2는 다양한 사양 옵션을 제공한다. t2.nano (CPU 1, 0.5 GB memory) 부터 x1.32xlarge (CPI 128, 1952 GB) 까지 다양하게 제공함으로 필요한 사양의 EC2 인스턴스(instnace)를 선택해서 사용하면 된다 (물론 사양이 좋을 수록 비싸다).
   + 사용법 : https://stackoverflow.com/c/wecode/questions/176  
2. Security Group
   + EC2 인스턴스에 대한 네트워크 트래픽을 제어하는 가상 방화벽 역활을 한다.
   + 즉 security group 설정을 해줘야 EC2 인스턴스에 HTTP와 SSH 접속이 가능하다.
3. RDS
   + AWS의 database 서비스
   + RDS를 사용하면 사용자가 직접 서버를 생성해서 데이터 베이스를 설치하고 설정하고 관리 하지 않아도 된다.
   + 그러면서 동시에 비용도 더 저렴하다. 사용자가 직접 데이터 베이스를 설치하고 운영하는 것보다 RDS를 사용하는것이 더 저렴함. 즉, RDS를 사용 하지 않을 이유가 거의 없다.
   + 사용법 : https://stackoverflow.com/c/wecode/questions/172
4. Load Balancer (ALB)
   + 로드발란서는 HTTP 요청들을 여러 서버에 분산할때 사용된다.
   + HTTP 요청이 많을때는 서버 하나만으로 모두 처리 하기 힘들기 때문에 서버 수를 늘리는것이 일반적이다. 그럼으로 여러 서버를 실행하고 로드발런서가 HTTP 요청들을 서버들에 분산 해주는 형태로 시스템이 구성된다.
5. Route 53
   + AWS의 DNS 서비스.
   + API 시스템을 실제 도메인과 연결 시키주는 기능을 제공한다.
+ S3
   + AWS S3(Simple Storage Service)는 이름 그대로 파일을 쉽게 저장할 수 있는 공간을 제공하는 서비스.
   + 파일을 저장 할 수 있을 뿐만이 아니라 파일마다 고유 주소를 부여해주기 때문에 S3에 저장한 파일을 웹상에서 쉽게 읽어들일수 있다.
   + 주로 사이트상의 이미지들을 저장하고 사이트에서 읽어들여 렌더링 해주는데 사용한다.

### Docker란?
원하는 개발환경을 파일에 저장하면, docker는 이를 내가 원하는 어떤 머신에든 환경을 시뮬레이션 해준다. 이런 환경들은 각기 독립적으로 존재하기 때문에, 원하는 어떤 환경이든 모듈처럼 관리가 가능하다. 그래서 파이썬 서버, 데이터베이스 써버, 자바써버를 따로 살 필요가 없다.
+  Docker는 가상화 기술이다.
+  Docker는 가상화 컨테이너에 application 배포를 자동화 시켜주는 오픈소스 엔진이다.
+  Docker는 container 가상화 실행 환경 위에 application 배포 엔진을 더함으로서 사용자의 코드를 어디서든 빠르고 가볍게 실행시킬수 있는 기술을 제공한다.

### Hypervisor 가상화 VS Container 가상화
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/Django/hypervisor.png)

+ Hypervisor  
Hypervisor는 물리적인 서버 에서 하나 혹은 그 이상의 독립적인 운영체제가 돌아가는 구조이다. 즉 ***물리적 서버의 OS 위에 여러 다른 독립적인 OS가 가상적(virtually)으로 돌아가는 구조***이다. 각각의 OS는 서로에 대해서 알지 못하며 base OS (물리적 서버의 OS)도 알지 못한다. ***하나의 물리적 서버에서 실행되고 있지만 가상적으로서 완전한 독립적 OS로 운영 되는 것***이다. 이러한 가상화의 장점은 물리적 서버의 리소스를 더 효율적으로 사용할수 있다는 것에 있다. 한 서버에 하나의 OS만 운영을 하는 경우 해당 OS가 서버의 모든 리소스를 항상 full로 사용하기 어려우므로 서버 리소드들이(예를 들어 CPU) idle 상태로 낭비되어 지는 경우가 있을수 있다. 반면에 hypervisor 가상화를 사용하여 하나의 서버에 여러 OS를 실행시키면 CPU를 idle 상태로 두지않고 필요한 OS나 서비스에 할당이 되어질 수 있음으로 리소스를 훨씬 효율적으로 쓰게 되는것이다. 그럼으로 고급 사양의 좋은 서버에 여러 가상화 OS들을 운영하는 것이 저급이나 중급 사양의 여러 서버를 운영하는것보다 훨씬 효율적이라는 것이다. 단점은 기술적을 너무 무겁다(heavy-weight)는 것이다. 독립적인 OS를 실행시키는 것이기 때문에 booting 시간이 길 수 밖에 없으며 리소스를 많이 차지할수 밖에 없다.

+ Docker   
***Docker 같은 컨테이너 가상화 기술***은 hypervisor 가상화와 틀리게 ***OS의 커널 위의 유저 공간(user space)에서 실행***된다. 즉 완전히 독립적인 운영체제를 가상화 하는 것이 아니라 독립적인 user space를 가상화 한다고 생각하면 쉽다. ***즉 하나의 호스트 서버에서 여러 독립적인 user space 인스탄스들을 가상적으로 실행할 수 있게 되는 것***이다. 이러한 가상화 기술의 장점은 hypervisor 가상화 보다 훨씬 가볍기 때문에 빠르고 쉽게 독립적인 가상 환경을 실행시킬수 있다 (또한 hypervisor는 base OS와 가상화 OS 사이에 커널 시스템 호출을 연결 시켜주는 emulation layer가 필요한데 docker는 hypervisor 처럼 emulator가 필요없이 그냥 일반적인 시스템 API interface를 사용한다). 예를 들어 ***docker image만 있으면 어디서든 쉽고 빠르게 test 환경, sandbox 환경 및 production 배포를 할 수 있다.*** 그럼으로 최근에 널리 퍼져있는 MSA(Micro Service Architecture)와 CI/CD에 아주 잘 어울리는 가상화 기술로서 각광을 받고 있다. 단점은 독립적인 OS가 아닌 user space 가상화를 하는 형태이다 보니 운영체제가 전혀 틀린 호스트에서는 실행을 시킬수가 없다. 예를 들어 Windows 를 linux 호스트에서 실행시킬수 없다 (하지만 사실 개발자라면 windows는 어차피 대부분 사용 안할테니 크게 상관없다). 그리고 완전히 독립적인 운영체제 가상화가 아니다 보니 보안적인 측면에서 hypervisor 보다 약할수 밖에 없다.

### Docker 구조
1. Docker client 와 server  
Docker는 클라이언트 와 서버 구조로 이루어저 있다. 클라이언트가 서버에 명령을 전달하고 서버가 실행시키는 구조이다. docker binrary 커맨드가 docker 클라이언트 이고 dockerd 가 docker daemon 혹은 docker engine이다. Docker engine과 interact하기 위한 Restful API도 제공된다. 클라이언트와 서버는 동일한 호스트 안에서 운영 될수도 있으며 서로 다른 호스트에서 운영 될수도 있다.

2. Docker 이미지  
Docker의 life cycle에서 docker 이미지는 “build”의 부분에 해당된다. Docker container(API가 실행되는 환경)에서 실행시키고 싶은 application을 docker 이미지로 빌드해서 실행시키게 된다.

3. Docker registries  
Docker registires는 docker 이미지를 저장하는 repository라고 보면 된다. Source code를 github에 저장하여 관리하듯 ***docker 이미지는 dockerhub 같은 docker registries에 저장한다***고 생각하면 된다. Github가 마찬가지로 public registry 가 있고 private registry가 있다.

4. Docker containers  
Docker container에서 docker 이미지가 실행된다. 즉 docker 이미지를 실행시키는 가상화 공간 이다. Docker container는 하나 혹은 그 이상의 프로세스를 실행 시킬수 있다 (하지만 하나의 프로세스만 실행시키는 것을 권장).

### Docker Compose And Swarm
Docker에서는 여러 docker container들로 이루어진 stack이나 cluster를 관리 하는 서비스도 제공하는데 바로 docker compose 와 docker swarm이다.  
Docker compose는 복수의 docker container들을 모아서 종합적인 application stack을 정의 하고 운영할수 있도록 해주는 서비스이다. Compose 파일을 사용하여 전체적인 application 서비스를 설정한후, application을 이루고 있는 각각의 컨테이너들 (예를 들어, web 서버 컨테이너, api 서버 컨테이너 등등)을 따로 실행시킬 필요 없이 한번에 생성하고 실행할 수 있도록 해준다. Docker swarm은 docker containers 들로 이러우진 cluster를 관리할수 있도록 해주는 서비스이다. 즉 docker container를 위한 clustering tool 이다.


### Docker VS Configuration Management Tools
Docker가 Chef나 Puppet, Ansible 같은 CM tool들을 대체하는가 에 대해서 물어보시는 분들이 많다. Docker를 사용함으로서 많은 CM(configuration management) 부분이 해결되는것은 맞지만 그래도 여전히 CM의 역활이 남아있다. 예를 들어, docker를 사용하기 위해서는 docker가 호스트에 설치되어 있어야 한다. 이러한 부분은 CM tool을 사용하여 관리할 수 있다.

### 그 외 Docker의 특징들
Docker는 modern한 리눅스 커널이 설치되어 있는 x64 호스트에서는 다 실행 가능하다 하지만 커널 버젼 3.10 이상에서 실행되는 것이 overhead가 적기 때문에 권장된다. 최근에는 docker를 Windows 나 Mac에서도 사용 가능하다. 하지만 Windows 및 Mac에서 docker를 직접 실행시키면 내부적으로 가상머신이 실행되어 그 안에서 docker가 실행되기 때문에 개발용 및 테스트 용으로는 괜찮지만 production 용도로는 적합하지 않다. Docker 컨테이너는 독립된 root 파일 시스템이기도 하다 그러므로 파일 시스템 분리가 이루어져 있다. 또한 docker 컨테이너는 호스트와 분리된 프로세스 환경을 가지고 있으므로 독립적인 프로세스를 실행할수 있다. 파일 시스템과 프로세스 환경과 마찬가지로 네트워크 또한 분리 되어 있어서 독립적인 가상 네트워크 인터페이스 와 IP 주소를 가질 수 있다. Resource isolation and grouping - 커널의 cgroups 기능을 통해서 docker container마다 독립적인 CPU와 메모리 가 할당 될수 있다. Docker 파일 시스템은 copy-on-write 을 사용하여서 효율적이며 빠른 디스크 IO를 실행한다. Docker의 copy-on-write은 중요한 특징중 하나이므로 다음에 더 자세히 설명하겠다. Docker 컨테이너에서 STDOUT, STDERR, STDIN을 통해서 생성되는 로그들은 전부 수집되어서 분석하거나 trouble-shooting을 가능하게 해준다. 그뿐만 아니라, 실행되고 있는 docker container의 shell에 접속해서 interact할 수도 있다.

### Docker 실습

1. 로컬 DB의 data를 RDS에 넣어준다.

**mysqldump** 명령어를 이용해서 sql 파일 추출

RDS server에 접속하는 명령어
```python
   $ mysql -h {RDS End-point 주소} -u root -p
```
RDS에 local DB의 data를 넣어주는 명령어
```python
    $ mysql -h {AWS의 RDS 엔드포인트} -u root -p {database 이름} < {database 이름}.sql  
```
만약에 no database selected 오류 메세지가 뜬다면 아래와 같이 직접 RDS server에 접근해서 local database와 같은 database를 생성해준다.
```python
    $ mysql -h {AWS의 RDS 엔드포인트} -u root -p

    mysql> show databases;
    mysql> create database {database 이름} character set utf8mb4 collate utf8mb4_general_ci;  
```
2. docker 설치 방법
   + mac os(local) : https://www.docker.com/에서 다운로드 받아서 설치하면 된다.
   + ubuntu : 
    아래 명령어 모두 실행
    ```python
        sudo apt update
        sudo apt install apt-transport-https ca-certificates curl software-properties-common
        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
        sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
        sudo apt update
        apt-cache policy docker-ce
    ```
    설치 및 정상적으로 실행 중인지 확인하는 명령어
    ```python
    sudo apt install docker-ce
    sudo systemctl status docker
    ```
3. (local) Project docker 설치 및 설정

   + requirements.txt 작성  
    이때, **gunicorn**을 추가해야 한다.
    ```python 
    bcrypt==3.1.7
    beautifulsoup4==4.8.1
    bs4==0.0.1
    certifi==2019.9.11
    cffi==1.13.1
    chardet==3.0.4
    Django==2.2.6
    django-cors-headers==3.1.1
    idna==2.8
    mysql-connector-python==8.0.18
    protobuf==3.10.0
    pycparser==2.19
    PyJWT==1.7.1
    pytz==2019.3
    requests==2.22.0
    selenium==3.141.0
    six==1.12.0
    soupsieve==1.9.4
    sqlparse==0.3.0
    urllib3==1.25.6
    gunicorn==20.0.0
    ```
   + Dockerfile 생성
        ```python
        FROM python:3

        WORKDIR /usr/src/app

        ## Install packages
        COPY requirements.txt ./
        RUN pip install -r requirements.txt

        ## Copy all src files
        COPY . .

        ## Run the application on the port 8080
        EXPOSE 8000

        #CMD ["python", "./setup.py", "runserver", "--host=0.0.0.0", "-p 8080"]
        CMD ["gunicorn", "--bind", "0.0.0.0:8000", "weWanted_u.wsgi:application"]
        ```
    weWanted_u는 wsgi.py가 있는 directory 이름을 써줘야된다.

   + docker image 생성
   ```python
      #docker 로그인
      docker login
      #user : lordmyshepherd, image 이름 : wewantedu, TAG : 0.1 
      docker build -t lordmyshepherd/wewantedu:0.1 .
   ```
   ```python
        # 아래 message가 뜨면 성공!!
        Sending build context to Docker daemon  695.8kB
        Step 1/7 : FROM python:3
        ---> a2aeea3dfc32
        Step 2/7 : WORKDIR /usr/src/app
        ---> Using cache
        ---> b8394f59f056
        Step 3/7 : COPY requirements.txt ./
        ---> 993c4ba3f183
        Step 4/7 : RUN pip install -r requirements.txt
        ---> Running in c083de141870
        Collecting bcrypt==3.1.7
        Downloading https://files.pythonhosted.org/packages/8b/1d/82826443777dd4a624e38a08957b975e75df859b381ae302cfd7a30783ed/bcrypt-3.1.7-cp34-abi3-manylinux1_x86_64.whl (56kB)
        Collecting beautifulsoup4==4.8.1
        Downloading https://files.pythonhosted.org/packages/3b/c8/a55eb6ea11cd7e5ac4bacdf92bac4693b90d3ba79268be16527555e186f0/beautifulsoup4-4.8.1-py3-none-any.whl (101kB)
        Collecting bs4==0.0.1
        Downloading https://files.pythonhosted.org/packages/10/ed/7e8b97591f6f456174139ec089c769f89a94a1a4025fe967691de971f314/bs4-0.0.1.tar.gz
        Collecting certifi==2019.9.11
        Downloading https://files.pythonhosted.org/packages/18/b0/8146a4f8dd402f60744fa380bc73ca47303cccf8b9190fd16a827281eac2/certifi-2019.9.11-py2.py3-none-any.whl (154kB)
        Collecting cffi==1.13.1
        Downloading https://files.pythonhosted.org/packages/0d/85/fa637061cc51edfa61ef688099ef1e4ccffdb0be2063006b31a78232345c/cffi-1.13.1-cp38-cp38-manylinux1_x86_64.whl (439kB)
        Collecting chardet==3.0.4
        Downloading https://files.pythonhosted.org/packages/bc/a9/01ffebfb562e4274b6487b4bb1ddec7ca55ec7510b22e4c51f14098443b8/chardet-3.0.4-py2.py3-none-any.whl (133kB)
        Collecting Django==2.2.6
        Downloading https://files.pythonhosted.org/packages/b2/79/df0ffea7bf1e02c073c2633702c90f4384645c40a1dd09a308e02ef0c817/Django-2.2.6-py3-none-any.whl (7.5MB)
        Collecting django-cors-headers==3.1.1
        Downloading https://files.pythonhosted.org/packages/47/0c/13b4435fbbcdc39e3aec21774dd41a37ee2d38750dbdab455a14bd4ccc23/django_cors_headers-3.1.1-py3-none-any.whl
        Collecting idna==2.8
        Downloading https://files.pythonhosted.org/packages/14/2c/cd551d81dbe15200be1cf41cd03869a46fe7226e7450af7a6545bfc474c9/idna-2.8-py2.py3-none-any.whl (58kB)
        Collecting mysql-connector-python==8.0.18
        Downloading https://files.pythonhosted.org/packages/bc/5c/abd95d9d978982e669d5caa7b40d86efce01e9729800043e40581b6e55f5/mysql_connector_python-8.0.18-py2.py3-none-any.whl (346kB)
        Collecting protobuf==3.10.0
        Downloading https://files.pythonhosted.org/packages/ad/c2/86c65136e280607ddb2e5dda19e2953c1174f9919b557d1d154574481de4/protobuf-3.10.0-py2.py3-none-any.whl (434kB)
        Collecting pycparser==2.19
        Downloading https://files.pythonhosted.org/packages/68/9e/49196946aee219aead1290e00d1e7fdeab8567783e83e1b9ab5585e6206a/pycparser-2.19.tar.gz (158kB)
        Collecting PyJWT==1.7.1
        Downloading https://files.pythonhosted.org/packages/87/8b/6a9f14b5f781697e51259d81657e6048fd31a113229cf346880bb7545565/PyJWT-1.7.1-py2.py3-none-any.whl
        Collecting pytz==2019.3
        Downloading https://files.pythonhosted.org/packages/e7/f9/f0b53f88060247251bf481fa6ea62cd0d25bf1b11a87888e53ce5b7c8ad2/pytz-2019.3-py2.py3-none-any.whl (509kB)
        Collecting requests==2.22.0
        Downloading https://files.pythonhosted.org/packages/51/bd/23c926cd341ea6b7dd0b2a00aba99ae0f828be89d72b2190f27c11d4b7fb/requests-2.22.0-py2.py3-none-any.whl (57kB)
        Collecting selenium==3.141.0
        Downloading https://files.pythonhosted.org/packages/80/d6/4294f0b4bce4de0abf13e17190289f9d0613b0a44e5dd6a7f5ca98459853/selenium-3.141.0-py2.py3-none-any.whl (904kB)
        Collecting six==1.12.0
        Downloading https://files.pythonhosted.org/packages/73/fb/00a976f728d0d1fecfe898238ce23f502a721c0ac0ecfedb80e0d88c64e9/six-1.12.0-py2.py3-none-any.whl
        Collecting soupsieve==1.9.4
        Downloading https://files.pythonhosted.org/packages/5d/42/d821581cf568e9b7dfc5b415aa61952b0f5e3dede4f3cbd650e3a1082992/soupsieve-1.9.4-py2.py3-none-any.whl
        Collecting sqlparse==0.3.0
        Downloading https://files.pythonhosted.org/packages/ef/53/900f7d2a54557c6a37886585a91336520e5539e3ae2423ff1102daf4f3a7/sqlparse-0.3.0-py2.py3-none-any.whl
        Collecting urllib3==1.25.6
        Downloading https://files.pythonhosted.org/packages/e0/da/55f51ea951e1b7c63a579c09dd7db825bb730ec1fe9c0180fc77bfb31448/urllib3-1.25.6-py2.py3-none-any.whl (125kB)
        Collecting gunicorn==20.0.0
        Downloading https://files.pythonhosted.org/packages/60/0d/3dbda0324f5bf007f3274e5ea09f0f3bcbf0ca01a75b80ff4f1ff9f8ecfd/gunicorn-20.0.0-py2.py3-none-any.whl (77kB)
        Requirement already satisfied: setuptools in /usr/local/lib/python3.8/site-packages (from protobuf==3.10.0->-r requirements.txt (line 11)) (41.6.0)
        Building wheels for collected packages: bs4, pycparser
        Building wheel for bs4 (setup.py): started
        Building wheel for bs4 (setup.py): finished with status 'done'
        Created wheel for bs4: filename=bs4-0.0.1-cp38-none-any.whl size=1274 sha256=6c94da63cd5d986b38427ca2e83c75695ee0db798a6d61077fff5e76f6853bff
        Stored in directory: /root/.cache/pip/wheels/a0/b0/b2/4f80b9456b87abedbc0bf2d52235414c3467d8889be38dd472
        Building wheel for pycparser (setup.py): started
        Building wheel for pycparser (setup.py): finished with status 'done'
        Created wheel for pycparser: filename=pycparser-2.19-py2.py3-none-any.whl size=111029 sha256=613badd3ef516540c3183857a65e817f85a92b5d85baaf81787e4b82874e8946
        Stored in directory: /root/.cache/pip/wheels/f2/9a/90/de94f8556265ddc9d9c8b271b0f63e57b26fb1d67a45564511
        Successfully built bs4 pycparser
        Installing collected packages: pycparser, cffi, six, bcrypt, soupsieve, beautifulsoup4, bs4, certifi, chardet, pytz, sqlparse, Django, django-cors-headers, idna, protobuf, mysql-connector-python, PyJWT, urllib3, requests, selenium, gunicorn
        Successfully installed Django-2.2.6 PyJWT-1.7.1 bcrypt-3.1.7 beautifulsoup4-4.8.1 bs4-0.0.1 certifi-2019.9.11 cffi-1.13.1 chardet-3.0.4 django-cors-headers-3.1.1 gunicorn-20.0.0 idna-2.8 mysql-connector-python-8.0.18 protobuf-3.10.0 pycparser-2.19 pytz-2019.3 requests-2.22.0 selenium-3.141.0 six-1.12.0 soupsieve-1.9.4 sqlparse-0.3.0 urllib3-1.25.6
        Removing intermediate container c083de141870
        ---> 88cf34c3cd8a
        Step 5/7 : COPY . .
        ---> af2199242c84
        Step 6/7 : EXPOSE 8000
        ---> Running in b12dc5993456
        Removing intermediate container b12dc5993456
        ---> 4bb2c44c6755
        Step 7/7 : CMD ["gunicorn", "--bind", "0.0.0.0:8000", "weWanted_u.wsgi:application"]
        ---> Running in 0fc099b28815
        Removing intermediate container 0fc099b28815
        ---> 118941b39255
        Successfully built 118941b39255
        Successfully tagged lordmyshepherd/wewantedu:0.1
    ```
    + docker image 확인 명령어
    ```python
       $ docker images
    ```
    ![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/Django/docker1.png)

    + container 만들고 실행하기 (생성-->실행)
    ```python
       $ docker run --name wewantedu(컨테이너 이름) -d -p 8000:8000 lordmyshepherd/wewantedu:0.1
    ```
    ![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/Django/docker2.png)
    status가 Up으로 되어 있으면 실행되고 있는 중이다.

    + 도커 컨테이너 내부 쉘에 들어가기 
    ```python
        $ docker exec -it wewantedu /bin/bash
    ``` 
    ![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/Django/docker3.png)
    ![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/Django/docker4.png)
    1행,2행을 보면 gunicon이 실행되고 있음을 알 수 있다.

    + docker registries에 push 
    ```python
        $ docker push lordmyshepherd/wewantedu:0.1
    ``` 
    ```python
        #아래 메세지 나오면 성공!
        The push refers to repository [docker.io/lordmyshepherd/wewantedu]
        85b12411f49b: Layer already exists
        96b0854d4cf6: Layer already exists
        bbc4a232e4a6: Layer already exists
        e0e431279aba: Layer already exists
        182852108eaf: Layer already exists
        bacd49aa77e7: Layer already exists
        1e7031c7fe7e: Layer already exists
        48ebd1638acd: Layer already exists
        31f78d833a92: Layer already exists
        2ea751c0f96c: Layer already exists
        7a435d49206f: Layer already exists
        9674e3075904: Layer already exists
        831b66a484dc: Pushed
        0.1: digest: sha256:f774a846708bd96debbc0bef100b69aede08f5f145cb255995d831f38ea49fc4 size: 3053
    ```

    + AWS EC2 server shell에 들어가기
    ```python
       $ ssh -i ~/Downloads/lims-key.pem ubuntu@13.209.11.45      
    ```
    + docker registries pull해서 받아오기
    ```python
        (base) ubuntu@ip-172-31-6-119:~$ docker pull lordmyshepherd/wewantedu:0.1
    ```

    + httpie를 이용해서 EC2 server에서 API가 구동되고 있는지 확인
    ![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/Django/docker5.png)

    지금까지 docker를 이용해서 AWS상에 하나의 EC2 Server에서 현재까지는 하나의 image(wewantedu project)를 구동시키는 과정을 정리해 보았다. 
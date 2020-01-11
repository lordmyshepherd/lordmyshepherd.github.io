---
title: "Data Structure(자료 구조)"
date: "2020-01-08T22:42:32.169Z"
template: "post"
draft: false
slug: "/posts/data_structure"
category: ""
tags:
description: "#자료구조란?, #List, #Tuple, #Set, #Dictionary, #Stack & Queue, #Tree"
socialImage: "/media/42-line-bible.jpg"
---
*The Lord is my shepherd, I lack nothing. PSLAM 23:1*

### 목표
**자료구조 이해하기**

+ Datastructure란?
+ List
+ Set
+ Dictionary
+ Stack & Queue
+ Tree


### 자료 구조란?
**자료구조란 데이터를 메모리에 배치하고 저장하는 방식이다.**  

백앤드 API의 핵심은 데이터 처리이다.  
데이터를 효율적으로 저장, 관리하여 메모리를 효율적으로 사용하기 위한 적절한 자료구조의 사용은 메모리의 용량을 절약해주고, 실행시간을 단축시켜줄 수 있다.  
예를 들면, 특정 원소를 Search하거나 add할 때(..등), 자료구조마다 그 방식이 다르고, 따라서 사용하는 메모리의 양과 실행 소요 시간도 다르기 때문에 **data 마다(또는 수행하는 일마다) 적절한 자료구조를 사용해야 한다.**

+ 자료구조 란 데이터에 편리하게 접근하고, 변경하기 위해서 데이터를 저장하거나 조직하는 방법 을 말합니다. 모든 목적에 맞는 자료구조는 없습니다.  
+ 따라서 각 자료구조가 갖는 장점과 한계를 잘 아는 것이 중요합니다.
+ 자료구조는 언어별로 지원하는 양상이 다릅니다. 따라서 각각의 언어가 가진 자료구조의 사용방법도 중요하지만, 무엇보다 각 자료구조의 본질과 컨셉을 이해하는 것 이 중요합니다.
+ 개념을 이해 한다면 해당 언어에 맞추어서 사용하기만 하면 됩니다.

**자료구조의 분류**
+ Primitive Data Structure(단순구조): 프로그래밍에서 사용되는 기본 데이터 타입
+ Non-Primtive Data Sturcture(비단순 구조): 단순한 데이터를 저장하는 구조가 아니라 여러 데이터를 목적에 맞게 효과적으로 저장하는 자료 구조.
    + Linear Data Structure(선형구조): 저장되는 자료의 전후관계가 1:1 (리스트, 스택, 큐 등)
    + Non-Linear Data Structure(비선형구조): 데이터 항목 사이의 관계가 1:n 또는 n:m (트리, 그래프 등)

### Array(List in python)
Array는 가장 기초적이고 단순하면서도 가장 자주 사용되는 자료구조 (data structure)이다.
```python
#참고
파이썬 에서는 array보다 list를 더 사용합니다. 사실 파이썬에서는 list가 array라고 생각하고 써도 무방합니다. 다만 엄밀히 말하자면 array와 list는 다릅니다. 
기능적으로는 거의 동일하지만 메모리 효율면에서는 array가 유리합니다. 
다만 사용하기는 list가 훨씬 편합니다.
파이썬에서 array를 사용할려면 import array 모듈을 import 해서 사용해야 합니다.
하지만 일반적으로 파이썬에서는 array 보다 일반 list가 더 많이 사용되고 대부분의 경우 큰 차이가 없음으로 그냥 list를 사용하면 됩니다.
```  
**1.Array의 특징**  
Array의 가장 큰 특징은 **순차적(ordered)으로 데이터를 저장한다**는 점입니다. 자료구조에 저장하는 데이터는 일반적으로 요소(element)라고 합니다). Array는 주로 서로 연결된 데이터들을 순차적 으로 저장할때 사용합니다. 순서가 상관 없더라도 서로 연결된 데이터들을 저장할때 일반적으로 사용됩니다. 그래서 array가 가장 자주 사용되는 자료구조중 하나가 되는 것입니다.

순차적으로 데이터 저장 --> index가 있다. --> index로 search, slice 가능

**2.Array는 왜 순서가 있는가?**  
Array가 순차적으로 데이터를 저장할 수 밖에 없는 이유가 있습니다. 그건 바로 실제 메모리 상에서 순차적으로 저장되기 때문입니다.
![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/array_index.png)
이렇게 순서가 있다보니 당연히 순차적으로 번호를 지정할 수 있습니다. 마치 학교에서 이름을 부르지 않고 번호를 불르는 것과 동일한 개념입니다. 이 번호를 index 라고 합니다.

Index는 0부터 시작됩니다. Index는 마이너스 부호를 가질 수 도 있습니다. 마이너스 index는 맨 마지막 요소 부터 시작합니다. 예를 들어, -1 은 맨 마지막 요소 입니다.

multi dimensional array
메모리 효과적이다.  —> 따닥따닥 저장되므로..
제일 빠르다. —> indexing을 통해서 변수 읽어 들이는 것과 같이 직접 찾아가서 읽을 수 있다.


메모리의 장점 : 빠르다, 휘발성이다, 




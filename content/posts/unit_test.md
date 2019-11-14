---
title: "Unit test"
date: "2019-11-13T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/unit_test/"
category: "Django"
tags:
description: "#DoesNotExist vs ObjectsDoesNotExist"
socialImage: ""
---
*The Lord is my shepherd, I lack nothing. PSLAM 23:1*

### unit test
+ unit test라는 이름에서 unit이라는 단어가 들어가 있는 이유는 테스트가 가능한 코드의 unit들을 테스트한다는 의미이다.  
unit들은 일반적으로 함수그리고 메소드이다. 그러므로 함수나 메소드를 호출한 후 결과값을 확인하는 코드를 실행하므로써 unit test를 실행하는 것이다.
+ unittest module : unit test를 가능하게 해주는 모듈이 python에 이미 포함되어 있다.
+ pytest라는 페키지도 있지만 django에서도 기본적으로 unittest가 사용된다.

### python unit test 개념 및 용어
+ TestCase  : unittest 프레임 워크의 테스트 조직의 기본 단위
+ Fixture   : 테스트를 진행할 때 필요한 테스트용 데이터 혹은 설정 등을 말한다.(주로 테스트가 실행되기 전이나 후에 생긴다.)
+ assertion : unittest에서 테스트 하는 부분이 제대로 됬는지를 확인하는 부분.(Assertion이 실패하면 테스트도 실패한다.)

### 장점
+  코드로 코드를 테스트하므로 자동화는 100% 가능하며, 언제든지 반복적으로 빠른 속도로 실행시킬 수 있다.
+  디버깅이 비교적 쉽다(이유 : 함수 단위로 테스트를 함으로써 문제가 있을 경우 파악이 비교적 쉬울 수 밖에 없다.)

### 단점
+  함수 단위로 테스트를 하다 보니 전체적인 부분을 테스트하기에는 제한적일 수 밖에 없다. (integration test와 UI test를 통해 보완해야 한다.)

### Unit Test 개발 가이드
+ Python의 unittest 모듈을 사용할 경우
    1. unittest 모듈을 import 한다.
        ```python 
        import unittest 
        ```
    2. unittest.TestCase 클래스를 상속하는 테스트 클래스를 만든다.
        ```python
        class MyCalcTest(unittest.TestCase):
        ```
    3. 테스트 클래스 안에 test_로 시작하는 테스트 메서드를 생성한다.
        ```python
        def test_add(self):
        ```
    4. 테스트 메서드에서는 테스트하고자 하는 함수나 메서드를 호출하고 그 결과 값을 **self.assert*()** 메서드를 사용하여 확인한다.
    (assertEqual, assertTrue, assertRaises, assertRegex 등 다양한 assert 메서드들을 사용할 수 있다.)
    ```python
        def test_add(self):
            c = myCalc.add(20, 10)
            self.assertEqual(c, 30)
    ```
    5. 테스트 클래스가 완성되었으면, unittest.main()을 호출하여 테스트를 실행시킨다.

### Unit Test 예제
```python
# mycalc.py
def add(a, b):
    return a + b

def substract(a, b):
    return a - b
```

```python
# tests.py
import unittest
import mycalc

class MyCalcTest(unittest.TestCase):

    def test_add(self):
        c = mycalc.add(20, 10)
        self.assertEqual(c, 30)

    def test_substract(self):
        c = mycalc.substract(20, 10)
        self.assertEqual(c, 10)

if __name__ == '__main__':
    unittest.main()
```

```python 
> python -m unittest --v
test_add (test_my_calc.MyCalcTest) ... ok
test_substract (test_my_calc.MyCalcTest) ... ok

----------------------------------------------------------------------
Ran 2 tests in 0.000s

OK
```

### Test Fixture
테스트 시나리오에 따라 어떤 경우는 테스트 전에 테스트를 위한 사전 준비 작업을 할 필요가 있다.  
또한 테스트가 끝난 후 테스트를 하기 위해 만든 리소스들을 정리(clean up)를 해야하는 경우도 있을 수 있다.  
unittest는 이러한 사전 준비 작업을 위해 ***setUp()** 이라는 메서드를, clean up 처리를 위해 tearDown() 이라는 메서드를 제공합니다.   
이러한 기능들을 Test Fixture라 하며, Fixture는 각각의 테스트 메서드가 실행되기 전과 후에 매번 실행된다.  

```python
import unittest

class MyCalcTest(unittest.TestCase):
    def setUp(self) :
    print("1. Executing the setUp method")
    self.fixture = { 'a' : 1 }

    def tearDown(self) :
    print("3. Executing the tearDown method")
    self.fixture = None

    def test_fixture(self) :
    print("2. Executing the test_fixture method")
    self.assertEqual(self.fixture['a'], 1)

```

```python
> python -m unittest --v
test_fixture (test_my_calc.MyCalcTest) ... 
1. Executing the setUp method
2. Executing the text_fixture method
3. Executing the tearDown method
ok
```

### Django에서 유닛 테스트 하는 방법
python의 unittest 라이브러리를 사용해서 테스트 하면 된다.
+ https://docs.djangoproject.com/en/dev/topics/testing/overview/
+ https://docs.djangoproject.com/en/dev/topics/testing/tools/

### Test의 일반 원칙
+ 테스트 유닛은 각 기능의 가장 작은 단위에 집중하여, 해당 기능이 정확히 동작하는지를 증명해야 합니다.

+ 각 테스트 유닛은 반드시 독립적이어야 합니다. 각 테스트는 혼자서도 실행 가능해야하고, 테스트 슈트로도 실행 가능해야 합니다. 이 때, 호출되는 순서와 무관하게 잘 동작해야 합니다. 이 규칙이 뜻하는 바, 새로운 데이터셋으로 각각의 테스트를 로딩해야 하고, 그 실행 결과는 꼭 삭제해야합니다. 보통 setUp() 과 tearDown() 메소드로 이런 작업을 합니다.

+ 테스트가 빠르게 돌 수 있도록 만들기 위해 노력해야 합니다. 테스트 하나가 실행하는데 몇 밀리세컨드 이상의 시간이 걸린다면, 개발 속도가 느려지거나 테스트가 충분히 자주 수행되지 못할 것입니다. 테스트에 필요한 데이터 구조가 너무 복잡하고, 테스트를 하려면 매번 이 복잡한 데이터를 불러와야 해서 테스트를 빠르게 만들 수 없는 경우도 있습니다. 이럴 때는 무거운 테스트는 따로 분리하여 별도의 테스트 슈트를 만들어 두고 스케쥴 작업을 걸어두면 됩니다. 그리고 그 외의 다른 모든 테스트는 필요한 만큼 자주 수행하면 됩니다.

+ 지금 사용하고 있는 툴이 개별 테스트나 테스트 케이스를 어떻게 수행하는지 배우셔야 합니다. 모듈 안에 들어있는 함수를 개발하고 있다면, 그 함수의 테스트를 자주, 가능하다면 코드를 저장할 때마다 자동으로 돌려야 합니다.
그날의 코딩을 시작하기 전에 항상 풀 테스트 슈트를 돌려야 합니다. 끝난 후에도 마찬가지입니다. 이 작업은 당신이 다른 코드를 망가뜨리지 않았다는 더 큰 자신감을 심어줄 것입니다.

+ 모두가 공유하는 저장소에다가 코드를 집어넣기 전에 자동으로 모든 테스트를 수행하도록 하는 훅을 구현하는 하는 것이 좋습니다.
+ 지금 한창 개발 중인데 그만두고 잠시 다른 일을 해야한다면, 다음에 개발할 부분에다가 일부러 고장난 유닛 테스트를 작성하는 것도 좋은 생각입니다.
+ 코드를 디버깅할 때 가장 먼저 시작할 일은 버그를 찝어내는 새로운 테스트를 작성하는 것입니다. 이런 일이 언제나 가능한 것은 아니지만, 이런 버그 잡이 테스트들이야말로 당신의 프로젝트에서 가장 가치있는 코드 조각이 될 것입니다.
+ 테스트 함수에는 길고 서술적인 이름을 사용하셔야 합니다. 테스트에서의 스타일 안내서는 짧은 이름을 보다 선호하는 다른 일반적인 코드와는 조금 다릅니다. 테스트 함수는 절대 직접 호출되지 않기 때문입니다. 실제로 돌아가는 코드에서는 square() 라든가 심지어 sqr() 조차도 괜찮습니다. 하지만 테스트 코드에서는 test_square_of_number_2(), test_square_negative_number() 같은 이름을 붙여야 합니다. 이런 함수명들은 테스트가 실패할 때나 보입니다. 그러니 반드시 가능한 한 서술적인 이름을 붙여야 합니다.
+ 무언가 잘못되었거나 뜯어고쳐야만 할 경우, 괜찮은 코드에 테스트 셋이 있다면 당신이나 다른 유지보수 담당자들은 오류를 수정하거나 프로그램의 동작을 수정할 때 필시 그 테스트 슈트에 전적으로 의지할 것입니다.
+ 테스트 코드의 또다른 사용 방법은 새로운 개발자들을 위한 안내서로 쓰는 방법입니다. 이미 만들어져 있는 코드에서 작업해야할 경우, 관련 테스트 코드를 돌려보고 읽어보는 것이야말로 가장 좋은 시작점일 경우가 많습니다. 이렇게 테스트 코드를 돌려보면 어느 지점이 문제인지, 수정하기 어려운 곳은 어디일지, 막다른 골목은 어디일지를 발견하게 됩니다. 몇 가지 기능을 추가해야 한다면 가장 먼저 해야할 일은, 그 새로운 기능이 아직 돌아가지 않음을 확인할 수 있는 테스트를 붙여넣는 것입니다.
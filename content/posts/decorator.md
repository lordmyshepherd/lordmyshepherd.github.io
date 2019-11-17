---
title: "Decorator"
date: "2019-11-14T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/decorator/"
category: "Django"
tags:
description: "#DoesNotExist vs ObjectsDoesNotExist"
socialImage: ""
---
*The Lord is my shepherd, I lack nothing. PSLAM 23:1*

### Nested Function(중첩함수)
+ 함수도 함수 안에서 중첩되어 선언될 수 있다.
+ 중첩함수 혹은 내부함수는 상위 부모 함수 안에서만 호출할 수 있다. 즉 부모 함수를 벗어나서 실행될 수 없다.
+ 아래 코드를 보면 child 함수는 parent 함수 밖에서 호출될 수 없다.
    ```python 
    def parent_function():
        def child_function():
            print("this is a child function")

        child_function()

    parent_function()
    > "this is a child function"
    ```

### Nested Function을 왜 사용할까?

1. 가독성  
함수를 사용하는 이유중 하나는 반복되는 코드블럭을 함수로 정의해서 효과적으로 코드를 관리하고 가독성을 높이기 위함입니다.  
중첩함수를 사용하는 이유도 동일합니다. 함수 안의 코드 중 반복되는 코드가 있다면 중첩함수로 선언하면 부모함수의 코드를 효과적으로 관리하고 가독성을 높일 수 있습니다. 

2. Closure (중요)
    + 중첩 함수가 부모 함수의 변수나 정보를 중첩 함수 내에서 사용한다.
    + 부모 함수는 리턴값으로 중첩 함수를 리턴한다.
    + 부모 함수에서 리턴 했으므로 부모 함수의 변수는 직접적인 접근이 불가능 하지만 부모 함수가 리턴한 중첩 함수를 통해서 사용될 수 있다.

3. 그렇다면 closure는 언제 사용하는 것일까?  
주로 factory 패턴을 구현할때 사용된다. 주로 함수나 오브젝트를 생성해내는데 사용된다.  
Factory에서 뭔가를 생성해 내기 위해서는 설정값이 필요할 것 입니다.  
그 설정값을 노출하지 않아서 수정이 불가능하게 하면서 해당 설정값을 기반으로한 연산을 할 수 있는 함수를 만들때 closure를 사용할 수 있다.   
<br>공장처럼 원하는 설정값을 적용해서 무엇가를 만들어내는 예제를 살펴보자.

   + 만일 어떤 ***주어진 수***의 제곱을 구하는 함수는 다음과 같을 것이다.
        ```python
        def calculate_power(number, power):
            return number ** power

        calculate_power(2, 7)
        > 128
        ```
   + ***주어진 수***의 제곱을 구하는 것이 아니라 특정 숫자의 제곱을 구하는 함수를 구현하면 다음과 같을 것이다.
        ```python 
        def calculate_power_of_two(power):
            return 2 ** power

        calculate_power_of_two(7)
        > 128
        calculate_power_of_two(10)
        > 1024
        ```
        하지만 위 함수는 2의 제곱 밖에 구할 수 없는 함수가 되버린다.
        이때 closure를 사용하면 된다!!!

        ```python
        def generate_power(base_number):
            def nth_power(power):
                return base_number ** power

            return nth_power
        
        calculate_power_of_two = generate_power(2)
        calculate_power_of_two(7)
        > 128
        calculate_power_of_two(10)
        > 1024

        calculate_power_of_seven = generate_power(7)
        calculate_power_of_seven(3)
        > 343
        calculate_power_of_seven(5)
        > 16907
        ```
        ![Nulla faucibus vestibulum eros in tempus. Vestibulum tempor imperdiet velit nec dapibus](/media/Django/closure.png)          


### *args, *kwargs

자동차를 구입할때 기본 사양의 자동차를 살 수 도 있고, 원하면 여러 옵션을 추가 해서 구입할 수 있습니다. 예를 들어, 가죽 의자 옵션을 추가 한다던가 해서 자동차를 구입할 수 있습니다.  

예를 들어, A라는 자동차는 옵션 사항이 40개 정도 된다고 가정해 보겠습니다.  
A라는 자동차를 구입하는 함수를 구현한다고 했을때, 모든 옵션 사항을 parameter로 받을려면 parameter수가 40개나 되어야 하므로 함수의 정의 부분이 너무 길어질것입니다.  
게다가 대부분의 옵션 사항은 설정이 안될 확률이 높습니다. 그렇다면 40개의 parameter가 사용되지 않는데도 불구하고 정의되어야 하는 상황이 됩니다.   
또한, A라는 자동차는 계속해서 업그레이드 되므로 전에는 없던 옵션이 생길 수 있습니다. 혹은 있던 옵션이 사라질 수 도 있습니다. 하지만 이미 정의된 함수를 바꾸려면 코드를 수정하고 다시 재배포 해야 하기 때문에 옵션 사항들을 그때 그때 바꿀수 있는 유동성(flexibility)가 떨어집니다.  

그렇다면 이렇게 사전에 정확히 필요한 parameter 수와 구조를 알수 없는 경우는 어떻게 해야할까요?

+ Dictionary를 paramter로 받아서 사용하는 방법
```python
def buy_A_car(options):
    print(f"다음 사양의 자동차를 구입하십니다:")

    for option in options:
        print(f"{option} : {options[option]}")

options = {"seat" : "가죽", "blackbox" : "최신"}
buy_A_car(options)

> 다음 사양의 자동차를 구입하십니다:
seat : 가죽
blackbox : 최신
```
이렇게 하면 원하는 옵션만 간단하게 설정 할 수 있다는 장점은 있지만 옵션을 dictionary로 받아야만 한다는 제약사항이 있습니다. 만일 옵션을 하나도 추가 안하고 기본사양으로 사는 경우에도 비어있는 dictionary를 넘겨줘야 하는것이죠. 게다가 dictionary가 아닌 다른 타입의 값 (예를 들어 string)을 넘겨주는 오류가 생길 확률도 있습니다.

1. Keyworded variable length of arguments
Keyworded variable length of arguments는 이름 그대로 keyword arguments 인데 그 수가 정해지지 않고 유동적으로 변할 수 있는 keyword arguments 이다.
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


        
    
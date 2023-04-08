### 목차

- [OOP의 정의와 그 특징](#oop의-정의와-그-특징)
  - [OOP란?](#oop란)
  - [OOP의 4가지 특징](#oop의-4가지-특징)
- [Class와 Instance(Object) 알아보기](#class와-instanceobject-알아보기)
  - [Class](#class)
  - [Instance(object)](#instanceobject)
  - [Python의 `self` 과 `cls` ?](#python의-self-과-cls-)
- [실제 코드에 Class 도입하기](#실제-코드에-class-도입하기)
  - [언제 도입?](#언제-도입)
  - [Class 사용 예시](#class-사용-예시)
  - [캡슐화 (encapsulation)](#캡슐화-encapsulation)
  - [다형성 (polymorphism)](#다형성-polymorphism)
  - [컴포지션 (composition)](#컴포지션-composition)
- [Namespace의 특징과 관련 함수 알아보기 + Magic method](#namespace의-특징과-관련-함수-알아보기--magic-method)
  - [Namespace란?](#namespace란)
  - [Namespace의 종류](#namespace의-종류)
  - [Namespace의 특징](#namespace의-특징)
  - [Namespace와 관련된 함수들](#namespace와-관련된-함수들)
  - [Magic method](#magic-method)
- [Python에서 Class 상속 적용하기](#python에서-class-상속-적용하기)
  - [class 상속](#class-상속)

---
## OOP의 정의와 그 특징

### OOP란?

- **필요**한 **데이터**를 **추상화**하여 **속성**(Attributes)와 **행위**(methods)를 가진 **객체**를 만들고, 그 **객체들 간**의 **유기적**인 **상호 작용**을 통해 **로직**을 **구성**하는 프로그래밍 방법입니다.
- ********장점********
    - 높은 **코드 재 사용**성 : 상속, 코드 부분 수정
    - **생산성** 향상 : 독립적인 객체를 구성하면 되기 때문에
    - **자연적 모델링** 가능 : 일상 생활의 모습과 유사하기 때문에 생각을 코드로 옮기기 쉬움
    - **유지 보수**의 **우수**성
- ********단점********
    - **개발 속도**가 느림
    - **절차 지향**에 **비해, 실행 속도**가 **느림**
    - **객체**가 **많아짐**에 따라 **용량**이 커짐

<br>

### OOP의 4가지 특징

- **캡슐화** : Encapsulation → **********************************Tell, Don’t Ask.**********************************
    - **객체(object)**의 **속성(attributes)과 행위(methods)**를 하나로 **묶**고, 구현 **코드를 외부에 감춰 은닉** 하는 것을 뜻합니다. 한마디로 **데이터**와 **처리 행위**를 묶고, **외부에는 그 행위를 보여주지 않는 것**입니다**.**
    - 이를 통해 **객체**의 **응집도**와 **독립성**을 **높**여 **객체**의 **모듈화**를 **지향**할 수 있게 됩니다. **객체**의 **모듈화**는 곧, **모듈 단위 재 사용**이 **용이**해진다는 **장점**이 있기 때문에 **간편**한 **코드 유지 보수**에 **도움**을 줍니다.

    - [캡슐화란 무엇인가? 어떤 이점이 있는가?](https://bperhaps.tistory.com/entry/%EC%BA%A1%EC%8A%90%ED%99%94%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-%EC%96%B4%EB%96%A4-%EC%9D%B4%EC%A0%90%EC%9D%B4-%EC%9E%88%EB%8A%94%EA%B0%80)

- ********************추상화 :******************** Abstraction
    - 중요하고 **필요**한 **정보 만**을 **표현**하기 **위해서,** 객체의 공통적인 속성(attributes)와 행위(methods)를 하나로 묶는 것을 의미합니다.
- **상속** : Inheritance
    - 기존 **상위 클래스**(class)에서 **정의된 기능**을 가져와 **재 사용**하거나, **새로운 기능**을 추가하는 식으로 **코드**의 **중복**을 **줄이**고, **재 사용성**을 **늘릴 수** 있는 **방법**을 의미합니다.
        - 파이썬은 다중 상속이 가능한 언어! → **MRO**(Method Resolution Order)를 통해 **순서 확인**
    - [파이썬(python) - MRO(Method Resolution Order)](https://tibetsandfox.tistory.com/26)
    - [점프 투 파이썬](https://wikidocs.net/16073)

```python
class 부모클래스1:
        ...내용...

class 부모클래스2:
        ...내용...

class 자식클래스(부모클래스1, 부모클래스2):
        ...내용...
```
- ******************다형성 :****************** Polymorphism
    - **객체**가 **상속**을 통해 **기능**을 **확장, 변경**하여 **여러 형태**의 **객체**로 **재구성** 되는 것을 의미합니다. **Overriding**과 **Overloading**을 통해 다형성을 확보할 수 있습니다.
    - Overriding은 **상위 클래스**에 **동일한 이름**을 가진 함수가 있을 때, **하위 클래스**에서 **기능**을 **재정의** 하는 것을 의미합니다.
    - Overloading는 **하나**의 **클래스** 안에 **같은 이름**을 가진 **함수**를 **여러 개** 만들 수 있게 해줍니다. 이때, 구분하기 위해서 **함수**의 **인자**들은 **달라야 합**니다.
  - [https://brunch.co.kr/@kd4/4](https://brunch.co.kr/@kd4/4)
---

## Class와 Instance(Object) 알아보기

### Class

- 데이터로 어떤 문제를 해결하기 위해서, **OOP 원칙에 따라** 코드로 구현한 것이다.
- 다시 말하면 어떤 집단(현실 세계)에 속하는 속성(attributes)과 행위(methods)를 코드로 구현해, 변수와 메소드로 정의한 것이다.
- **Class**는 **설계도** 역할을 하기 때문에, **실제 메모리** 상에 올라가진 **않**는다.
- 실제 Class가 메모리에 할당되면, 그것을 instance라고 부른다.

### Instance(object)

- **Class**에서 **정의한 것(설계도)를 토대**로 **실제 메모리 상에 할당**된 것(실제 사물, object)이다.
- 프로그램의 **데이터**로 만들어진 **실체**이다.
- Class로 만들어진 여러 instance(object)는 각각 **독립적**이다.
- Class를 설계도라고 비유 했으니, Instance는 **생산된 실제 물건**이라고 할 수 있다.

### Python의 `self` 과 `cls` ?

- `self`는 **instance object** 그 자체이다.
    - 따라서 Class의 `self`의 주소와 생성된 `instance`의 `메모리 id`는 같다.
- `cls`는 Class 그 자체이다.
    - `cls` 는 Class이기에, `메모리 id` 정보가 출력 되지 않는다.

```python
class SelfTest:
    
    # 클래스 변수
    name = "amamov"
    
    def __init__(self, x):
        self.x = x
    
    # 클래스 메소드
    @classmethod
    def func1(cls):
        print(f"cls: {cls}")
        print("func1")
    
    # 인스턴스 메소드
    def func2(self):
        print(f"self : {self}")
        print("class 안의 self 주소 : ", id(self))
        print("func2")

test_obj = SelfTest(17)
test_obj.func2()
SelfTest.func1()

print("인스턴스의 주소:", id(test_obj))
```

- 실행 결과

```python
self : <__main__.SelfTest object at 0x0000018EB1351F10> 
class 안의 self 주소 :  **1712370032400**
func2
cls: <class '__main__.SelfTest'>
func1
인스턴스의 주소: **1712370032400**
```

- `<__main__.SelfTest object at 0x0000018EB1351F10>`
    - SelfTest의 object 즉 instance임을 의미
- `<class '__main__.SelfTest'>`→ cls가 class 자체를 의미
    - cls는 메모리에 할당되지 않기 때문에, object 주소 값이 없다!

----

## 실제 코드에 Class 도입하기

### 언제 도입?

- 우선 절차 지향적으로 코드 구성  → Class 도입 고민
    - 하나의 집단에서 반복되는 것이 있고 행위를 나눌 수 있을 경우
    - 상태, 속성을 나눌 수 있을 경우

```python
__init__ : 생성자 class가 memory에 allocation됐을 때
즉 instance가 생성됐을 때 실행

def __init__(self)
- 이때, self는 생성된 instance를 가리킴
	self.a = a
	self.b = b
 - _없을 경우 외부에서 접근 가능

def add(self):
	return self.a + self.b
- 이때, self를 통해 생성된 instance를 가리켜 해당 instance의 
namespace에 접근할 수 있게 됨
```

### Class 사용 예시

- `siri`, `jarvis`, `biby`의 공통된 속성, 행위를 묶어 하나의 Class로 표현
    - 추상화 과정
        - 공통의 속성 - 행위를 하나로 묶었기 때문에
        - 사용 시 method에서 **어떤 동작이 이뤄지는 지 알 필요 없이, 기능 수행이 가능하기 때문에**

```python
Class Robot: 
	# 클래스 변수 : 인스턴스들이 공유하는 변수
	population = 0
	# 생성자 함수 -> 인스턴스 생성 시 실행 됨 
	def __init__(self, name, code): 
			self.name = name # 인스턴스 변수
			self.code = code # 인스턴스 변수
			Robot.population += 1 # Robot 클래스의 변수이므로 Robot으로 접근
	# 인스턴스 메소드
	def say_hi(self):
		pass
	def cal_add(self, a, b)
		pass
	def die(self):
		pass
	# 클래스 메소드
	@classmethod
	def how_many(cls): # cls는 클래스를 지칭
		print(f'We have {cls.population} robots.')
	# static method
	# class나 instance와 참조 필요 없을 때 사용
	# 명시적 선언이 필요 없지만 쓰는 것이 좋음
	@staticmethod
	def this_is_robot_class():
		print("yes!!")

siri = Robot("siri", 1234)
jarvis = Robot("jarvis", 2312)
bixby = Robot("bixby", 4567)
```

### 캡슐화 (encapsulation)

- `__` : private → 상속되지 않음
    - 엄밀히 따지면 접근이 되지만 상속 X인 특징 보유
- `_` : protected → 상속됨.
    - 접근 가능하지만 명시적으로 접근하지 말라고 알려줌.

### 다형성 (polymorphism)

- 같은 형태의 코드가 다른 동작을 하도록 하는 것
- 재 사용성 늘릴 수 있음 → 하나의 디자인 패턴

```python
class Siri(Robot):
    def say_apple(self):
        print("hello my apple")

class SiriKo(Robot):
    def say_apple(self):
        print("안녕하세요")

class Bixby(Robot):
    def say_samsung(self):
        print("안녕하세요ㅕ")
```

### 컴포지션 (composition)

- 다른 class의 일부 method를 사용하고 싶지만 상속은 하고 싶지 않을 경우
- 왜 상속을 안하고 싶은데??
    1. 상속되면 **부모 클래스가 변하면 자식 클래스는 계속 수정**되기 때문에 → 일부만 반영하고 싶을 수도 있음
        1. 결국 유지 보수가 어려워 짐
    2. 부모 클래스의 메서드를 오버라이딩 하는 경우 **내부 구현 방식의 얕은 이해로 오류가 생길 가능성이 증가**
        1. method가 여러 상황에서 사용될 경우 모두 확인하는 것은 힘들다.
    
    ```python
    def __say_hi(self):
            self.cal_add(1, 3) # 요런 상황
            print(f"Greetings, my masters call me {self.__name}.")
    
        def cal_add(self, a, b):
            return a + b + 1
    
        @classmethod
        def how_many(cls):
            return f"We have {cls.__population} robots."
    ```
    
- Robot 클래스를 호출 후
    - 별도로 Robot의 method만 사용

    ```python
    class BixbyCal:
        def __init__(self, name, age):
            self.Robot = Robot(name, age)

        def cal_add(self, a, b):
            return self.Robot.cal_add(a, b)
    ```
---

## Namespace의 특징과 관련 함수 알아보기 + Magic method

### Namespace란?

- 프로그래밍 언어에서 특정한 **객체(object)를 이름(name)에 따라 구분할 수 있는 범위**
- **파이썬 모든 것은 객체**로 구성되며 이들 각각은 **특정 이름과의 매핑 관계**를 갖게 되는데 이 **매핑을 포함하고 있는 공간을 namespace**라고 한다.

### Namespace의 종류

- global namespace : **모듈 별로 존재**하며, 모듈 전체에서 통용될 수 있는 이름들이 소속
- local namespace : 함수 및 method 별로 존재하며, 함수 내의 지역 변수들의 이름들이 소속
- built-in namespace : **기본 내장 함수** 및 **기본 예외들의 이름들이 소속**. 파이썬으로 작성된 모든 코드 범위가 포함

![11](https://user-images.githubusercontent.com/86637320/230710526-eab7ea3c-eb91-4f87-9eb1-40a6ce953f8c.png)
- [https://www.programiz.com/python-programming/namespace](https://www.programiz.com/python-programming/namespace)

### Namespace의 특징

- namespace는 딕셔너리 형태로 구현
- 모든 이름 자체는 문자열로 돼있고 각각은 **해당 namespace의 범위**에서 **실제 object**를 가리킨다.
- **이름**과 실제 **객체 사이의 매핑은 Mutable**이므로 런타임 동안 새로운 이름이 추가될 수 있다.
- bulit-in namespace는 함부로 추가하거나 삭제할 수 없다.

![22](https://user-images.githubusercontent.com/86637320/230710550-5d3ab612-8f2e-486d-a7d7-c925bedc91b3.png)
- [[Python] 네임스페이스 개념 정리](https://hcnoh.github.io/2019-01-30-python-namespace)

### Namespace와 관련된 함수들

- `__dict__`
    - namespace를 확인할 수 있도록 dict 형태로 반환
    - 이때, 인스턴스 method의 경우 instance.__dict__에서 조회되지 않고
    클래스.__dict__에서 조회됨
    - Why? 변수는 재정의할 일이 많지만 method의 경우 재정의할 일이 별로
    없음.
    - 따라서 memory 효율을 위해 class namespace에 instance method
    저장함.
- Q. instance namespace에 없는 instance method가 어떻게 조회 가능?
    - python에선 **instance namespace에 존재하지 않을 경우 class namespace**를 확인함!
    - 따라서 instance에서도 class method와 class instance에 접근할 수 있다.

```python
siri.population - 가능
siri.how_many() - 가능

Robot.say_hi() -> say_hi가 참조할 instance가 없어서 불가능
# 동일 Namespace라서 접근은 가능하지만 참조할 instance가 없는 상황
Robot.say_hi(siri) -> say_hi가 참조할 instance인 siri가 지정돼 가능 # self가 무엇인지 전달!
```

- `dir()`
    - siri로 접근 가능한 namespace에서 사용할 수 있는 모든 **key 값** 출력
    - 이때, python은 namespace의 key로 str 사용하므로 이름들이 다 나옴
- `__doc__`
    - 작성한 doc-string이 출력된다.
    - 추상화 전략에 따라서 doc-string만 보고도 함수 사용법을 확인할 수 있어야 함
- `__class__`
    - 해당 instance가 어떤 class로 생성됐는지 확인
    - instance는 참조 class 정보를 알 수 있지만 class는 instance 정보를 알 수 없음!

### Magic method

- aka DUNDER method
- `__init__` : 생성자
- `__str__` : 사용자에게 instance 정보를 알려줌
    - class에서 `__str__` 재정의(over-riding)를 통해서 변경 가능

```python
print(instance) 시에 실행
<__main__.Robot object at 0x~~~>
```

- `__call__` : callable하도록 변경
    - `__call__`이 object에 있다면 instance()로 호출 가능!

---

## Python에서 Class 상속 적용하기

### class 상속

1. 부모 class가 갖는 모든 methods와 attributes가 자식 class에 그대로 전달
2. 자식 class에서 별도의 methods나 attributes를 추가할 수 있다.
3. method over-riding
    1. 동일 이름의 method가 재정의 → 덮어 씌움
4. super()
    1. 부모 class를 접근할 수 있음.
    2. super(자기자신, self) : python 2에선 필수였지만 python3에서는 안써도 됨

```python
super().say_hi() : 부모 class의 say_hi() method 실행
```

1. Python의 모든 class는 object class를 상속한다. → 모든 것은 object 즉 객체이다.
    - class.mro()를 통해서 확인 가능
    - class Robot? → class Robot(object)가 생략된 것
2. 의미 없는 다중 상속은 anti-pattern으로 지양하자!

```python
class Robot:

    """
    [Robot Class]
    Date : ??:??:??
    Author : Amaco
    """

    population = 0

    def __init__(self, name):
        self.name = name
        Robot.population += 1

    def die(self):
        print(f"{self.name} is being destroyed!")
        Robot.population -= 1
        if Robot.population == 0:
            print(f"{self.name} was the last one.")
        else:
            print(f"There are still {Robot.population} robots working.")

    def say_hi(self):
        print(f"Greetings, my masters call me {self.name}.")

    def cal_add(self, a, b):
        return a + b

    @classmethod
    def how_many(cls):
        return f"We have {cls.population} robots."

    @staticmethod
    def are_you_robot():
        print(f"{Robot.population} num!")
        print("yes!!")

    def __str__(self):
        return f"{self.name} robot!!"

    def __call__(self):
        print("call!")
        return f"{self.name} call!!"

class Siri(Robot):
    def call_me(self):
        print("네?")

    def cal_mul(self, a, b):
        self.a = a
        return a * b

    @classmethod
    def hello_apple(cls):
        print(f"{cls} hello apple!!")
```

- object?
    - 모든 object가 상속하는 class
    - 파이썬의 모든 것은 객체다!
        - [https://gist.github.com/shoark7/fb388e6494350442a2d649a154f69a3a](https://gist.github.com/shoark7/fb388e6494350442a2d649a154f69a3a)

```python
data_types = [int, float, bool, str, list, tuple, set]

for tp in data_types:
    print("Is {} a subclass of object?: {}".format(tp, (issubclass(tp, object))))

Is <class 'int'> a subclass of object?: True
Is <class 'float'> a subclass of object?: True
Is <class 'bool'> a subclass of object?: True
Is <class 'str'> a subclass of object?: True
Is <class 'list'> a subclass of object?: True
Is <class 'tuple'> a subclass of object?: True
Is <class 'set'> a subclass of object?: True
```

```python
print(object)
<class 'object'>
print(dir(object))
['__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__']
print(object.__name__)
object
```

---
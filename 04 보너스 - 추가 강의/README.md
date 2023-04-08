### 목차

- [Decorate(데코레이터)](#decorate데코레이터)
  - [파이썬 기본 데코레이터 함수](#파이썬-기본-데코레이터-함수)
- [dict 사용 시 병목 현상 줄이기 : slot 활용](#dict-사용-시-병목-현상-줄이기--slot-활용)
  - [slot](#slot)
## Decorate(데코레이터)

- 기존 함수에 기능을 첨가하는 역할
    - 재 사용성을 고려해 불필요한 반복을 줄일 수 있음

```python
def copyright(func):
	# copyright안에서 func가 재정의됨
	def new_func():
		print("@ dfsklfdjflkjskdjfldsjf")
		func()
	
return new_func

def smile():
	print("!")

def angry():
	print("#")

# 재정의된 smile 함수
# smile이 재정의됨 -> 아직 호출은 X
smile = copyright(smile)
# 재정의된 smile 함수가 호출됨
smile()

# @를 도입하면 재정의 과정 생략 가능
**smile = copyright(smile) -> 재정의 과정**

def smile()
-> 데코레이터 도입
@copyright
def smile()
```

### 파이썬 기본 데코레이터 함수

- `@staticmethod`
    - Class나 instance에 대한 참조 없이 독립적인 기능을 수행하고 싶을 때 선언
- `@classmethod`
    - Class를 참조해 다른 클래스의 속성, 메소드를 참고하고 싶을 때 선언

[static메소드 @classmethod, @staticmethod](https://blog.naver.com/parkjy76/30167615254)

- `@property`  ~= `getter`
    - 접근하여 값을 확인하고 싶을 때, 적절한 값으로 보내고 싶을 때
    - 접근은 가능하도록 → but 수정은 못하게 제한
    - attributes를 get할 수는 있지만 set은 불가

```python
@property
    def age(self):
        return self.__age

@property
    def name(self):
        return f"yoon {self.__name}"
```

- `@함수명.setter`
    - 접근은 불가능하고 수정은 가능하도록
    - 유효성 검사 및 수정

```python
@age.setter # 앞서 정의한 property를 setter로 변경
    def age(self, new_age):
        if new_age - self.__age == 1:
            self.__age = new_age
        else:
            raise ValueError()
```
---
## dict 사용 시 병목 현상 줄이기 : slot 활용

### slot

- 객체 내 변수들은 `__dict__`를 통해서 관리됨.
- `__dict__`를 통한 동적 관리 대신 미리 slots을 통해서 지정 후 관리하면 더 빠른 속도 가능

```python
class WithoutSlotClass:
    def __init__(self, name, age):
        self.name = name
        self.age = age

wos = WithoutSlotClass("amamov", 12)

print(wos.__dict__)

wos.__dict__["hello"] = "world"

# dict를 통해서 접근했기 때문에 변수로 추가됨

print(wos.__dict__)
```

- `__slot__` : 변수들을 list로 미리 지정해서 관리
    - memory 효율성 증가
    - 반복 횟수가 커질 수록 차이 증가

```python
class WithSlotClass:
    __slots__ = ["name", "age"]

    def __init__(self, name, age):
        self.name = name
        self.age = age

ws = WithSlotClass("amamov", 12)

print(ws.__slots__)

# * 메모리 사용량 비교

def repeat(obj):
    def inner():
        obj.name = "amamov"
        obj.age = 222
        del obj.name
        del obj.age

    return inner

use_slot_time = timeit.repeat(repeat(ws), number=9999999)
no_slot_time = timeit.repeat(repeat(wos), number=9999999)

print("use slot : ", min(use_slot_time))
print("no slot : ", min(no_slot_time))
```
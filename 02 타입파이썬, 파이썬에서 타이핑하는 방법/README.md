### 목차

- [Type Hint 사용법 알아보기](#type-hint-사용법-알아보기)
  - [Type Hints \& Type Checking?](#type-hints--type-checking)
  - [유형 별 사용법](#유형-별-사용법)
    - [Callable Types](#callable-types)
    - [Class Types](#class-types)
    - [Union Types](#union-types)
    - [Optional Types](#optional-types)
    - [Final Types](#final-types)
    - [Type Alias](#type-alias)
    - [TypedDict](#typeddict)
    - [Generic Type](#generic-type)
- [`mypy` , `pyright`로 type checking 하기](#mypy--pyright로-type-checking-하기)

---
## Type Hint 사용법 알아보기

### Type Hints & Type Checking?

- Type Hints : Type 확인은 가능하지만 강제할 수가 없음.
- Type Checking : Type 확인 및 Type 강제 가능
    - type_check 예시

```python
def type_check(obj, typer) -> None:
    if isinstance(obj, typer):
        pass
    else:
        raise TypeError(f"Type Error : {typer}")

def cal_add(x: int, y: int) -> int:
    # code
    type_check(x, int)
    type_check(y, int)
    return x + y
```
### 유형 별 사용법

#### Callable Types

- 함수에 대한 type hint
    - 함수를 입력으로 받을 경우 처리

```python
from typing import Callable

def add(a: int, b: int) -> int:
    return a + b

print(add(1, 33))

def test():
    pass

def foo(func: Callable[[int, int], int]) -> int:
    return func(2, 3)

# 에러 발생
print(foo(test))
```

#### Class Types

- class에 대한 type hint

```python
class Hello:
    def world(self) -> int:
        return 7

class World:
    pass

hello: Hello = Hello()
# hello: "Hello" = Hello()와 동일
world: World = World()
# world: "world" = World()와 동일

def foo(ins: Hello) -> int:
    return ins.world()

print(foo(hello))

```

- class에서 자기 자신을 type hint로 지정 시 “class 이름으로 지정”
- python 3.10 이상에선 “class 이름”으로 지정할 필요 X

```python
# * class type 보충
from typing import Optional

class Node:
    def __init__(self, data: int, node: Optional["Node"] = None):
        self.data = data
        self.node = node

node2 = Node(12) # node: Optional["Node"] = None
# default 값이 없을 경우 error 발생

node2 = Node(12, None) # error 발생 X

node1 = Node(27, node2)

node0 = Node(30, node1)
```

#### Union Types

- 여러 data type을 처리하고 싶을 때 사용

```python
Union[int, str] : int가 올 수도 있고 str이 올 수도 있음
```

#### Optional Types

- Union[str, None] 대신 사용할 수 있다.

```python
Union[str, None] -> Optional[str]로 정의 가능

from typing import Union, Optional

def foo(name: str) -> Optional[str]:
    if name == "amamov":
        return None
    else:
        return name

xxx: Optional[str] = foo("amamov")
```

#### Final Types

- **상수**의 개념을 사용하기 위해서 도입
- Final type의 경우 **변경되면 안 된**다.

```python
from typing_extenstions import final

RATE : Final = 300

RATE = 255 # error 발생!
```

#### Type Alias

- type에 대해서 alias(별칭)을 지음
- 만약 **type이 너무 길어**서 **관리**하기 **힘들 경우**에 사용

```python
# * type alias

Value = Union[
    int, bool, Union[List[str], List[int], Tuple[int, ...]], Optional[Dict[str, float]]
]

X = int

x: X = 8

value: Value = 17

def cal(v: Value) -> Value:
    # ddmasda
    return v
```

#### TypedDict

- Dict에 각 성분에 어떤 type이 와야 하는지 지정 가능
    - json 파일이나, dict 다룰 때 활용

```python
ddd: Dict[str, Union[str, int]] = {"hello": "world", "world": "wow!!", "hee": 17}

class Point(TypedDict):
    x: int
    y: float
    z: str
    hello: int

point: Point = {"x": 8, "y": 8.4, "z": "hello", "hello": 12} 
```

#### Generic Type

- **데이터 형식에 의존하지 않**고, **하나의 값이 여러 다른 데이터 타입들을 가질 수 있는 기술**
- Type hint를 쓸 때 도움 됨.

```python
from typing import Union, Optional

class Robot:
	def __init__(self, arm : Union[int, str], head : int):
		self.arm = arm
		self.head = head

	def decode(self):
		bbs: Optional[Union[int, str] = None
 # 이 경우 arm의 type이 이미 결정 됐음에도 bbs에서 type을 
 # 결정할 수 없다는 단점 존재.

from typing import Union, Optional, TypeVar, Generic

T = TypeVar("T", int, float, str) # 어떤 type이 와야할 지 명시  
K = TypeVar("K", int, float, str)

class Robot(Generic[T, K]):
    def __init__(self, arm: T, head: K):
        self.arm = arm
        self.head = head

    def decode(self):
        # 암호를 해독하는 코드
        # 복잡
        pass

robot1 = Robot[int, int](12231413, 23908409)
robot2 = Robot[str, int]("12890309123", 79878789)
robot3 = Robot[float, str](1239.01823, "3243245")

# 상속하는 경우 generic type과 상속할 class를 모두 적음
class Siri(Generic[T, K], Robot[T, K]):
    pass

siri1 = Siri[int, int](12231413, 23908409)
siri2 = Siri[str, int]("12890309123", 79878789)
siri3 = Siri[float, str](1239.01823, "3243245")

print(siri1.arm)

# * function

def test(x: T) -> T:
    print(x)
    print(type(x))
    return x

test(898)
```
---
## `mypy` , `pyright`로 type checking 하기

- 이때, python 공식에서 지원하는 기능은 아니기 때문에
- type checking이 필요한 경우만 뽑아서 확인하는 것을 추천
    - **중요한 부분만 따로 빼서 확인**
- `mypy` 사용법
    - 파일의 type checking 진행

```python
mypy 파일명.py
```

- 결과 값까지 확인하고 싶을 때

```python
mypy 파일명.py && python 파일명.py
```

- `pyright` 사용법
    - 설치하면 mypy처럼 사용 가능

```python
mypy 파일명.py
```

- 결과값까지 확인

```python
mypy 파일명.py && python 파일명.py
```

- pyright의 장점?
    - c로 compile 후 check → 빠른 속도
    - pylance에 설치돼 있음

```python
settings -> python.type -> Python > Analysis: Type Checking Mode
```
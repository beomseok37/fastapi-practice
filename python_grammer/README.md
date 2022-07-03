### 매개변수 `*`

매개변수에서 default value를 가지지 않는 변수보다 가지는 변수가 앞에 오면 에러를 일으킨다.
이럴 경우 함수의 매개변수의 시작을 `*`로 시작하면 에러를 방지할 수 있다.<br>
`*` 뒤에 오는 변수들은 key-value로 인자를 넣어줘야 한다.

### typing 모듈

```py
from typing import List,Dict,Tuple,Final,Union,Optional

nums: List[int] = [1, 2, 3]
countries: Dict[str, str] = {"KR": "South Korea", "US": "United States", "CN": "China"}
user: Tuple[int, str, bool] = (3, "Dale", True)

#상수
TIME_OUT: Final[int] = 10

#여러 개의 타입 허용
def toString(num: Union[int, float]) -> str:
    return str(num)

#None이 허용되는 함수의 매개변수에 대한 타입을 명시할 때 유용
def repeat(message: str, times: Optional[int] = None) -> list:
    if times:
        return [message] * times
    else:
        return [message]
```

### Callable

파이썬에서는 함수를 일반 값처럼 변수에 저장하거나 함수의 인자로 넘기거나 함수의 반환값으로 사용할 수 있다. `Callable`을 이러한 함수에 대한 타입 어노테이션을 추가할 때 사용한다.<br>
예시

```py
from typing from Callable
def repeat(greet:[[str], str], name:str, times: int=2) -> None:
  for _ in range(times):
    print(greet(name))
```

위와 같은 경우 greet함수는 str형의 인자를 하나 받고 str형 변수 하나를 반환하는 함수임을 나타내고 있다.

### 타입 추상화

함수의 매개변수에 대한 타입 어노테이션을 추가해줄 경우, 타입을 추상적으로 명시해줄 수 있게 해준다.<br>
리스트와 같은 경우 list, tuple 등 여러 리스트가 인자로 들어올 수 있기 때문에 `Iterable`을 통해 추상적으로 명시해줄 수 있다.

```py
from typing import Iterable, List

def toStrings(nums: Iterable[int]) -> List[str]:
    return [str(x) for x in nums]

#출력
>>> toStrings([1, 2, 3])
['1', '2', '3']
>>> toStrings((1, 2, 3))
['1', '2', '3']
>>> toStrings({1, 2, 3})
['1', '2', '3']
```

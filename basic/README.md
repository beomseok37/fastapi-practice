### 라이브 서버 실행

```sh
uvicorn main:app --reload
```

main - 파일 main.py 를 의미
app - main.py 내부의 app=FastAPI() 줄에서 생성한 오브젝트
--reload - 코드 변경 후 서버 재시작. 개발 시에만 사용

### 유용한 기능

'/doc'에 접속할 경우 자동으로 api문서를 만들어준다.<br>
'/redoc'에 접속할 경우 대안 자동 문서를 만들어준다.(api관련)<br>
FastAPI는 API를 정의하기 위한 OpenAPI표준을 사용하여 모든 API를 이용해 스키마를 생성한다.<br>
스키마란 무언가를 정의 또는 설명하는 것으로, 구현하는 코드가 아니라 추상적인 설명일 뿐이다.

밑의 내용은 [FastAPI Docs](https://fastapi.tiangolo.com/ko/tutorial/first-steps/#api)의 내용이다.
API "스키마"<br>
이 경우, OpenAPI는 API의 스키마를 어떻게 정의하는지 지시하는 규격입니다.
이 스키마 정의는 API 경로, 가능한 매개변수 등을 포함합니다.

데이터 "스키마"<br>
"스키마"라는 용어는 JSON처럼 어떤 데이터의 형태를 나타낼 수도 있습니다.
이러한 경우 JSON 속성, 가지고 있는 데이터 타입 등을 뜻합니다.

OpenAPI와 JSON 스키마<br>
OpenAPI는 API에 대한 API 스키마를 정의합니다. 또한 이 스키마에는 JSON 데이터 스키마의 표준인 JSON 스키마를 사용하여 API에서 보내고 받은 데이터의 정의(또는 "스키마")를 포함합니다.

openapi.json 확인<br>
가공되지 않은 OpenAPI 스키마가 어떻게 생겼는지 궁금하다면, FastAPI는 자동으로 API의 설명과 함께 JSON (스키마)를 생성합니다.

여기에서 직접 볼 수 있습니다: http://127.0.0.1:8000/openapi.json

다음과 같이 시작하는 JSON을 확인할 수 있습니다

```json
{
    "openapi": "3.0.2",
    "info": {
        "title": "FastAPI",
        "version": "0.1.0"
    },
    "paths": {
        "/items/": {
            "get": {
                "responses": {
                    "200": {
                        "description": "Successful Response",
                        "content": {
                            "application/json": {
...

```

### http 메소드 데코레이터

- @app.post()
- @app.put()
- @app.delete()
-

### 경로 매개 변수

```py
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/{item_id}")
async def read_item(item_id: int):
    return {"item_id": item_id}
```

- '/'를 통해 경로를 정할 수 있다.
- '{}'를 통해 url path로 들어온 값을 변수로 사용할 수 있다.
- 함수의 인자 타입을 지정해서 '{}'를 통해 들어오는 변수의 타입을 지정해줄 수 있다.(해당 타입이 아닐 경우 오류 발생)
- '/docs'를 통해 문서화된 api들을 확인할 수 있다.

### 순서 문제

'/users/me'와 'users/{user_id}'와 같은 경우 user_id는 int형이므로 me가 먼저 판별이 되야지만 오류가 발생하지 않는다. 경로 동작은 순차적으로 평가되기 때문에 '/users/me'가 먼저 선언되어야 한다.

### 사전 정의된 값

**열거형 클래스(Enum)**가 path url로 어떤 타입의 변수를 받을 경우 해당 변수를 특정할 수 있게 해준다.

```py
from enum import Enum

from fastapi import FastAPI

class ModelName(str, Enum):
    alexnet = "alexnet"
    resnet = "resnet"
    lenet = "lenet"

app = FastAPI()

@app.get("/models/{model_name}")
async def get_model(model_name: ModelName):
    if model_name == ModelName.alexnet:
        return {"model_name": model_name, "message": "Deep Learning FTW!"}

    if model_name.value == "lenet":
        return {"model_name": model_name, "message": "LeCNN all the images"}

    return {"model_name": model_name, "message": "Have some residuals"}
```

'/models/{model_name}'에 접속할 경우 model_name에 들어갈 수 있는 내용이 'alexnet', 'resnet', 'lenet'으로 한정되게 된다. (타입 어노테이션)

### 쿼리 매개변수

쿼리 매개변수는 url상에서 '?'뒤에 나오는 key와 value의 쌍들이다.(/item?name=beomseok)

```py
from fastapi import FastAPI

app = FastAPI()

fake_items_db = [{"item_name": "Foo"}, {"item_name": "Bar"}, {"item_name": "Baz"}]

@app.get("/items/")
async def read_item(skip: int = 0, limit: int = 10):
    return fake_items_db[skip : skip + limit]
```

'/items?skip=0'로 접속할 경우 `fake_items_db`의 0~2까지 접근하게 됩니다.

- 쿼리 매개변수로 들어온 값들은 모두 string이지만 인자로 타입과 함계 선언할 경우, 해당 타입으로 변환됩니다.

### 여러 경로/쿼리 매개변수

- 매개변수 선언 시 ` = None`을 넣을 경우 선택적으로 들어올 수 있음을 명시할 수 있다.
- 쿼리 매개변수에 타입을 지정할 경우 해당 타입으로 자동 변환이 된다.
- 경로 매개변수와 쿼리 매개변수를 동시에 선언할 수 있으며 특정 순서로 선언하지 않아도 된다.
- 필수적인 쿼리 매개변수를 선언할 경우 뒤에 ` = None`을 넣어주지 않으면 된다.

### 추가 검증

매개변수로 받아올 경우 설정을 할 수 있다.

```py
from typing import Union

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Union[str, None] = Query(default=None, max_length=50)):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```

q는 필수로 들어오는 매개변수가 아니고 최대길이는 50이다.

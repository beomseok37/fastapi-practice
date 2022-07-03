# Fastapi Practice

시작하기에 앞 서 여러 python 프로젝트의 버전 충돌 문제를 없애기 위해서 virtualenv를 사용한다.

### virtual env 설치

**mac OS 혹은 Linux일 경우**

```sh
sudo easy_install virtualenv
```

혹은

```sh
sudo pip install virtualenv
```

로 설치 가능하다.

**자신만의 실행환경 만들기**

```sh
mkdir myproject
cd myproject
virtualenv venv
```

### 실행환경 활성화

```sh
. venv/bin/activate
```

### Fastapi 설치

```sh
pip install fastapi
pip install uvicorn
```

fastapi[all] 모듈이 없어서 관계로 따로 설치
`uvicorn`이 서버역할을 한다.

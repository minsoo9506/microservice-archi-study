# Dependency Injection ?
- 참고 자료
  - [Dependency Injection Demystified - James Shore](https://www.jamesshore.com/v2/blog/2006/dependency-injection-demystified)
  - [Dependency Injection & Inversion of Control (youtube)](https://www.youtube.com/watch?v=EPv9-cHEmQw)

## DI 설명
- Dependency injection
  - giving an object its instance variables
  - 아래 코드 예시처럼 constructor parameter 에 object(dependency) 를 inject 하는 것이다.
- 구현 (예시 코드)
  - 아래 코드에서 처럼 `DataBase` 라는 interface 를 구현해서 사용한다.
    - 다양한 종류의 DB 를 쉽게 사용할 수 있고 test 하기도 용이하다.

```java
public class Example {
  private DataBase myDatabase;

  public Example() {
    myDatabase = new DataBase();
  }

  public Example(DataBase specificDataBase) {
    myDatabase = specificDataBase;
  }

  public void DoStuff() {
    ...
    myDatabase.GetData();
    ...
  }
}

public interface DataBase {
    void GetData();
}

public class MySqlDataBase implements DataBase {
    ...
}
```
- 이를 고려하지 않는 코드는 아래와 같이 `Example` 클래스에서 바로 특정 db  객체를 생성해서 사용한다.
  - `Example` 클래스는 `SqlDataBase`의 object를 내부에서 생성하기에 depend 한 상태 (`Example` -> `SqlDataBase`)

```java
public class Example {
  private SqlDataBase myDatabase;

  public Example() {
    myDatabase = new SqlDataBase();
  }

  public void DoStuff() {
    ...
    myDatabase.GetData();
    ...
  }
}
```

## 개인적인 경험
- 실제로 최근에 개발한 클래스에서 DI 를 적용할만한 부분이 기억남
  - 특정 클래스 내부에서 다른 클래스의 instance 를 하드코딩으로 만들어서 사용함
- 만약 여기에 DI 의 개념을 적용한다면 장점?
  - 테스트: 현재 unit test 를 클래스마다 진행하는데 instance 가 abstractclass 가 아니라 하드코딩 되어 있어서 테스트하려는 클래스만 테스트하는게 아니라 하드코딩된 instance 의 기능까지 테스트가 되어버림
  - 확장성: 하드코딩 된 클래스를 다른 클래스로 바꾸는 개발을 진행할때 훨씬 편해짐, 또는 특정 유저들은 A클래스를 또 다른 유저들에 대해 B클래스를 적용하고 싶으면 하드코딩하지 않고 instance 를 parameter 로 넣기만 하면 쉽게 할 수 있음


# [IoC and DI - Martin Fowler](https://martinfowler.com/articles/injection.html)
## Components and Service
- 파울러가 해당 글에서 사용할 용어에 대한 본인의 생각
  - component: glob of software that's intended to be used, without change, locally (eg. jar file, source import)
  - service: component 와 비슷하나 외부 app 에서 사용되는 것, not locally (eg. web service, socket, messaging system)

## Inversion of Control
- 파울러는 IoC 가 너무 generic term 이라서 DI 라는 이름으로 정착했다고 설명함

## Forms of Dependency Injection
- 3가지 DI 스타일 존재
  - constructor injection, setter injection, interface injection

### Constructor injection
- interface `MovieFinder` 의 implementation 을 `MovieLister` 의 constructor 를 통해 inject 하는 container(`pico`) 가 있음

```java
class MovieLister...
  public MovieLister(MovieFinder finder) {
      this.finder = finder;       
  }

class ColonMovieFinder...
  public ColonMovieFinder(String filename) {
      this.filename = filename;
  }

private MutablePicoContainer configureContainer() {
    MutablePicoContainer pico = new DefaultPicoContainer();
    Parameter[] finderParams =  {new ConstantParameter("movies1.txt")};
    pico.registerComponentImplementation(MovieFinder.class, ColonMovieFinder.class, finderParams);
    pico.registerComponentImplementation(MovieLister.class);
    return pico;
}

public void testWithPico() {
    MutablePicoContainer pico = configureContainer();
    MovieLister lister = (MovieLister) pico.getComponentInstance(MovieLister.class);
    Movie[] movies = lister.moviesDirectedBy("Sergio Leone");
    assertEquals("Once Upon a Time in the West", movies[0].getTitle());
}
```
### Setter Injection with Spring
- developers tend to prefer setter injection
- Spring 을 이용 (`@Bean`)
```java
class MovieLister...
  private MovieFinder finder;
  public void setFinder(MovieFinder finder) {
  this.finder = finder;
}
```

### Interface injection
- define and use interfaces for the injection

```java
public interface InjectFinder {
    void injectFinder(MovieFinder finder);
}

class MovieLister implements InjectFinder
  public void injectFinder(MovieFinder finder) {
      this.finder = finder;
  }
```

## Using a Service Locator

## Deciding which option to user
### constructor vs setter injection
- general issue with object-oriented programming - should you fill fields in a constructor or with setters ?
- constuctor 를 이용할 때 장점 (파울러는 이걸 좀 더 선호)
  - 명확하게 어떤 object 들이 해당 클래스에 필요한지 파악하기 용이함
  - 또한 setter 를 제공하지 않음으로 해서 immutable 한 field 를 만들 수 있음
- 물론 단점도 존재
  - 너무 field 들이 많아지는 경우 존재 가능
  - 각 field 가 언제 어디에 쓰이는지 파악하기 쉽지 않을 수 있음
  - 상속시 더 복잡해질수있음
- 하지만 상황에 따라 다르게 사용, 언제든지 바꿀 수 있으면 됨

### code vs configuration file
- always provide a way to do all configuration easily with a programmatic interface
- and then treat a separate configuration file as an optional feature

# [A. stackoverflow discussion on python and DI](https://stackoverflow.com/questions/2461702/why-is-ioc-di-not-common-in-python)
- DI/IoC 가 Python 에 uncommon 하지 않음
- DI/IoC framework/container 가 uncommon 하다고 할 수 있음

# APP
## Ch 13 Dependency Injection (and Bootstrapping) (Part1. Building an Architecture to Support Domain Modeling)
- 코드가 많아서 요약정리는 생략

# DI in FastAPI: 1, 2, 3
- Note that FastAPI's dependency injection is not a general DI framework. It is very specific to FastAPI's path operations.
## [Dependencies](https://fastapi.tiangolo.com/tutorial/dependencies/)
- FastAPI 에서 dependency 생성 예시
  - can take all the same parameters that a path operation function can take
```python
from typing import Annotated
from fastapi import Depends, FastAPI

app = FastAPI()

async def common_parameters(q: str | None = None, skip: int = 0, limit: int = 100):
    return {"q": q, "skip": skip, "limit": limit}

@app.get("/items/")
async def read_items(commons: Annotated[dict, Depends(common_parameters)]):
    return commons

@app.get("/users/")
async def read_users(commons: Annotated[dict, Depends(common_parameters)]):
    return commons
```
## [Advanced Dependencies](https://fastapi.tiangolo.com/advanced/advanced-dependencies/)
- use the instance as a dependency
  - fixed function, class 만들 dependency 로 사용할 필요는 없음
```python
from typing import Annotated
from fastapi import Depends, FastAPI

app = FastAPI()

class FixedContentQueryChecker:
    def __init__(self, fixed_content: str):
        self.fixed_content = fixed_content

    def __call__(self, q: str = ""):
        if q:
            return self.fixed_content in q
        return False

checker = FixedContentQueryChecker("bar")

@app.get("/query-checker/")
async def read_query_check(fixed_content_included: Annotated[bool, Depends(checker)]):
    return {"fixed_content_in_query": fixed_content_included}
```

## [Testing Dependencies with Overrides](https://fastapi.tiangolo.com/advanced/testing-dependencies/)
- test 시에만 사용하고 싶은 dependency 가 있는 경우가 존재
  - 예를 들어, 외부의 authentication system 을 이용하는 경우 테스트 할 때 이를 이용하고 싶지 않을 수 있음
- FastAPI 에서는 dependency 를 override 하는 기능을 제공
```python
from typing import Annotated

from fastapi import Depends, FastAPI
from fastapi.testclient import TestClient

app = FastAPI()

async def common_parameters(q: str | None = None, skip: int = 0, limit: int = 100):
    return {"q": q, "skip": skip, "limit": limit}

@app.get("/items/")
async def read_items(commons: Annotated[dict, Depends(common_parameters)]):
    return {"message": "Hello Items!", "params": commons}

@app.get("/users/")
async def read_users(commons: Annotated[dict, Depends(common_parameters)]):
    return {"message": "Hello Users!", "params": commons}

client = TestClient(app)

async def override_dependency(q: str | None = None):
    return {"q": q, "skip": 5, "limit": 10}

app.dependency_overrides[common_parameters] = override_dependency

def test_override_in_items():
    response = client.get("/items/")
    assert response.status_code == 200
    assert response.json() == {
        "message": "Hello Items!",
        "params": {"q": None, "skip": 5, "limit": 10},
    }

def test_override_in_items_with_q():
    response = client.get("/items/?q=foo")
    assert response.status_code == 200
    assert response.json() == {
        "message": "Hello Items!",
        "params": {"q": "foo", "skip": 5, "limit": 10},
    }

def test_override_in_items_with_params():
    response = client.get("/items/?q=foo&skip=100&limit=200")
    assert response.status_code == 200
    assert response.json() == {
        "message": "Hello Items!",
        "params": {"q": "foo", "skip": 5, "limit": 10},
    }
```

# [(Optional) python-dependency-injector]
-  This is a general DI framework. See this [example](https://python-dependency-injector.ets-labs.org/examples/fastapi-sqlalchemy.html) using it with FastAPI's DI and SQLAlchemy.
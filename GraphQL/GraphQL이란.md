![image](https://user-images.githubusercontent.com/62709718/147321342-5391baa2-583f-4570-aa4b-580edf1ee4f3.png)

## GraphQL이란?
Facebook이 만든 쿼리 언어 (서버로부터 데이터를 효율적으로 가져오기 위해, 주체: 웹클라이언트)
- 필요한 것을 구체적으로 요청 (필요한 것만 정확히 얻음)
- 단일 요청으로 많은 데이터를 얻음 (모든 데이터를 한번에)
- 타입 시스템 (엔드포인트 x, 타입과 필드)

***쿼리 언어?***

SQL(Structured Query Language): 구조화된 질의어
RDBMS(관계형 데이터베이스 관리 시스템)의 데이터 관리를 위해 설계된 언어
-> DB로부터 데이터를 효율적으로 가져오기 위해

## REST API와 GraphQL의 차이점

1. 리소스

    REST API에서 리소스 모양과 크기는 서버에 의해 결정됨. 클라이언트는 수동적인 입장

    GraphQL은 클라이언트가 필요한 리소스를 요청함. 프론트 입장에서 좀 더 적극적이고 주도적임

2. 엔드포인트

    REST API에서는 다중 엔드포인트, URL과 Method에 따라 접근할 수 있는 데이터가 다름

    GraphQL은 보통 단일 엔드포인트, GraphQL 스키마에 따라 데이터가 다름
    
3. 라우트 핸들러와 리졸버

    REST API에서 각 요청은 정확히 하나의 경로 처리 함수를 호출
    
    GraphQL에서는 하나의 쿼리가 여러 리졸버를 호출하여 여러 리소스가 포함된 중첩 응답 구성
    
REST API를 사용하면 일반적으로 ***여러 엔드포인트***에 엑세스해서 데이터를 수집해야함

ex) 사용자/게시물/팔로워 데이터를 받아오려면

    /users/<id>
    /users/<id>/posts
    /users/<id>/followers

GraphQL은 구체적인 데이터 요구 사항이 포함된 ***단일 쿼리***로 요청가능

ex) 사용자/게시물/팔로워 데이터를 받아오려면
```
query{
  User(idL "er3tg439frjw"){
    name
    posts{title}
    followers{name}
  }
}
```

## 그래서 GraphQL이 REST API보다 나은점은?
##### REST API는 ***overfetching***과 ***underfetching***을 유발시킴
사용자 이름만 알고 싶은데, ```/users/<id>``` 호출하면 서버가 정해둔 모든 데이터를 받아오게됨. 사용자 정보와 팔로워를 알고싶은데 ```/users/<id>``` 호출하고 ```/users/<id>/followers``` 호출해야함

##### GraphQL은 ***overfetching X, underfetching X***
필요한 데이터만 요청할 수 있고 원하는 중첩 데이터 요청을 할 수 있음.

## GraphQL의 장점 정리
- 프론트엔드에서 신속한 제품 이터레이션을 돌릴 수 있음 (서버에 매번 API 요청을 하지 않아도됨)
- 백엔드에서 분석이 가능함 (프론트엔드에서 어떤 데이터를 가져다 쓰는지 알 수 있게 되므로)
- 스키마 및 타입 시스템의 이점이 있음 (프론트엔드와 백엔드가 사용하는 데이터 구조를 맞출 수 있음)

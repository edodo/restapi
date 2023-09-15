# REST API 작성 가이드
<br/>
<br/>

## 목차
<br/>

[1. REST API URL 구조](#1-rest-api-url-구조)<br/>
[2. 명명 규칙](#2-명명-규칙)<br/>
[3. Request/Response 본문](#3-requestresponse-본문)<br/>
​	[3-1. GET All, GET 목록 정보 조회에 대한 Response 본문 표시](#3-1-get-all-get-목록-정보-조회에-대한-response-본문-표시)<br/>
​	[3-2. GET  개별 정보 조회에 대한 Response 본문 표시](#3-2-get-개별-정보-조회에-대한-response-본문-표시)<br/>
​	[3-3. POST / PUT / PATCH 추가 / 수정에 대한 Request 본문 표시(단일)](#3-3-post--put--patch-추가--수정에-대한-request-본문-표시단일)<br/>
​	[3-4. POST / PUT / PATCH 추가 / 수정에 대한 Request 본문 표시(복수)](#3-4-post--put--patch-추가--수정에-대한-request-본문-표시복수)<br/>
​	[3-5. 요청 및 처리 실패 시 응답](#3-5-요청-및-처리-실패-시-응답)<br/>
[4. REST API의 3요소](#4-rest-api의-3요소)<br/>
[5. 프로그램 리소스 이름 표기 규칙](#5-프로그램-리소스-이름-표기-규칙)<br/>
​	[5-1. PascalCase](#5-1-pascalcase)<br/>
​	[5-2. camelCase](#5-2-camelcase)<br/>
​	[5-3. snake_case](#5-3-snake_case)<br/>
​	[5-4. spinal-case](#5-4-spinal-case)<br/>
[6. Model Object](#6-model-object)<br/>
[7. HTTP method](#7-http-method)<br/>
​	[7-1. 멱등성(idempotent)](#7-1-멱등성idempotent)<br/>
​	[7-2. PUT과 PATCH의 차이](#7-2-put과-patch의-차이)<br/>
[8. HTTP 상태 코드](#8-http-상태-코드)<br/>
[9. REST API Rule](#9-rest-api-rule)<br/>
[10. Reference](#10-reference)<br/>
<br/>

---
<br/>

## 1. REST API URL 구조
<br/>

```java
{domain}:{port}/api/{version}/{object}?{query params}
ex) 127.0.0.1:8080/api/v1/users?id=test
```
<br/>

---
<br/>

## 2. 명명 규칙
<br/>

|Type|Rule|
|:---:|:---|
|URI|_spianl-case_|
|Path Variable|_camelCase_|
|Query Parameter Key|_camelCase_|
|JSON|_camelCase_|

<br/>

---
<br/>

## 3. Request/Response 본문
<br/>

* Text 포맷은 `JSON`으로 정의한다.
* 표기법은 `camelCase`로 정의한다.
* 클라이언트의 요청을 성공적으로 처리할 때 응답한다.
<br/>

### 3-1. GET All, GET 목록 정보 조회에 대한 Response 본문 표시
<br/>

* data에 조회 결과를 `Array`로 정의한다 
```java
[
    {
        "id": 1406264199,
        "connectedAt": "2020-07-14T06:15:36Z",
        "synchedAt": "2020-07-14T06:15:36Z"
    },
    {
        "id": 1399634384,
        "connectedAt": "2020-07-06T09:55:51Z",
        "synchedAt": "2020-07-06T09:55:51Z"
    }
    ...
]
```
<br/>

### 3-2. GET 개별 정보 조회에 대한 Response 본문 표시
<br/>

* data에 조회 결과를 `JSON`으로 정의한다.
```java
{
    "id": 1406264199,
    "connectedAt": "2020-07-14T06:15:36Z",
    "synchedAt": "2020-07-14T06:15:36Z"
}
```
<br/>

### 3-3. POST / PUT / PATCH 추가 / 수정에 대한 Request 본문 표시(단일)
<br/>

* request body에 담을 data를 `JSON`으로 정의한다.
```java
{
    "id": 1406264199,
    "connectedAt": "2020-07-14T06:15:36Z",
    "synchedAt": "2020-07-14T06:15:36Z"
    "sample1": {
        "id": 1399634384,
        "name": "sample",
        "createdAt": "2020-07-06T09:55:51Z"
    }
    "sample2": {
    "id": 1399634384,
    "name": "sample",
    "createdAt": "2020-07-06T09:55:51Z"
    }
    "sample3": {
    "id": 1399634384,
    "name": "sample",
    "createdAt": "2020-07-06T09:55:51Z"
    }
}
```
<br/>

### 3-4. POST / PUT / PATCH 추가 / 수정에 대한 Request 본문 표시(복수)
<br/>

* request body에 담을 data를 `Array`로 정의한다.
```java
[
    {
        "id": 1406264199,
        "connectedAt": "2020-07-14T06:15:36Z",
        "synchedAt": "2020-07-14T06:15:36Z",
        "sample1": {
            "id": 1399634384,
            "name": "sample",
            "createdAt": "2020-07-06T09:55:51Z"
        },
        "sample2": {
            "id": 1399634384,
            "name": "sample",
            "createdAt": "2020-07-06T09:55:51Z"
        },
        "sample3": {
            "id": 1399634384,
            "name": "sample",
            "createdAt": "2020-07-06T09:55:51Z"
        }
    },
    {
        "id": 1406264199,
        "connectedAt": "2020-07-14T06:15:36Z",
        "synchedAt": "2020-07-14T06:15:36Z",
        "sample1": {
            "id": 1399634384,
            "name": "sample",
            "createdAt": "2020-07-06T09:55:51Z"
        },
        "sample2": {
            "id": 1399634384,
            "name": "sample",
            "createdAt": "2020-07-06T09:55:51Z"
        },
        "sample3": {
            "id": 1399634384,
            "name": "sample",
            "createdAt": "2020-07-06T09:55:51Z"
        }
    },
    ..........
]
```
<br/>

### 3-5. 요청 및 처리 실패 시 응답
<br/>

```java
{
    "timestamp": "2023-04-05T02:13:51.640+00:00",
    "status": 405,
    "error": "Method Not Allowed",
    "path": "/api/v1/users/health_check"
}
```
<br/>

---
<br/>

## 4. REST API의 3요소
<br/>

| 구성 요소 | 내용 | 표현 방법 | 예 |
| ------ | ------ | ------ | ------ |
| 자원(Resource) | 자원 | HTTP URI | /users/{1}, /users |
| 행위(Verb) | 자원에 대한 행위 | HTTP Method | POST, GET, DELETE, PUT |
| 표현(Representations) | 자원에 대한 행위의 내용 (즉, 요청에 대한 Body) | HTTP Message Payload (JSON, XML, TEXT, RSS 등) | {  userId: ”test”,  userName: ”홍길동“ } |

<br/>

---
<br/>

## 5. 프로그램 리소스 이름 표기 규칙
<br/>

### 5-1. PascalCase
<br/>

*  각 단어의 첫 문자를 대문자로 작성하는 표기법
```java
PascalCase, ClassName, UserName
```
<br/>

### 5-2. camelCase
<br/>

*  각 단어의 첫 문자를 대문자로 표기하되 첫 문자는 소문자로 작성하는 표기법
```java
camelCase, varialbleName, userName
```
<br/>

### 5-3. snake_case
<br/>

*  전체 문자를 소문자로 표시하고, 각 단어를 언더바(\_)로 구분해주는 표기법
```java
snake_case, table_name, user_name
```
<br/>

### 5-4. spinal-case
<br/>

*  Spinal Case, Kebab Case, Train Case, Lisp Case 라고도 불리며, 전체 문자를 소문자로 표시하고, 각 단어를 하이픈(-)로 구분해주는 표기법
```java
kebab-case, spinal-case, train-case
```
<br/>

---
<br/>

## 6. Model Object
<br/>

* Controller Layer에서 Request body 또는 Request Param, 그리고 Respone body에 DTO 객체를 사용한다.
* Controller Layer와 Service Layer 계층 간에는 DTO 객체를 사용하여 데이터를 주고 받아야 한다.
* Service Layer와 DAO 계층에서는 Entity 객체를 사용한다.
* Request DTO 객체 내부에 `toEntity `함수를 구현하여 Entity 객체로 변환할 때 사용한다.
* `UserReq`, `UserRes`와 같은 패턴으로 DTO 객체를 생성한다.
<br/>

---
<br/>

## 7. HTTP method
<br/>

| METHOD | CRUD   | IDEMPOTENT |
| ------ | ------ | ---------- |
| POST   | Create | N          |
| GET    | Read   | Y          |
| PUT    | Update | Y          |
| PATCH  | Patch  | Y          |
| DELETE | Delete | Y          |

<br/>

### 7-1. 멱등성(idempotent)
<br/>

멱등성(idempotent)란, 연산을 여러 번 적용하더라도 결과가 달라지지 않는 성질을 의미한다. 즉, 연산을 여러 번 반복하여도 한 번만 수행된 것과 같은 성질을 의미한다.
<br/>

HTTP 메서드를 예로 들자면, `GET`, `PUT`, `PATCH`, `DELETE`메서드는 여러번 호출해도 결과가 동일하지만, `POST`메서드는 매 호출마다 새로운 데이터가 추가되기 때문에 멱등성을 가진다고 할 수 없다.
<br/>

### 7-2. PUT과 PATCH의 차이
<br/>

`PUT` 과 `PATCH`는 모두 데이터 수정을 위한 HTTP Method이다.

* `PUT`요청은 속성값을 일부만 전달하면 나머지는 디폴트 값으로 수정되는 것이 원칙이므로 **수정을 원치않는 속성값도 포함하여 요청**해야 한다.
  * 만약 속성값 일부만 전달하는 경우 전달한 필드 외에는 모두 `NULL` 또는 디폴트 값으로 처리되어야 한다.

<br/>

`PUT`요청으로 `홍길동`이라는 사람의 나이(age)를 15로 변경하고자할 때 수정할 속성값만 전달할 경우 전달하지 않은 속성값은 `NULL`로 변경된다.
```JAVA
PUT /users/1
{
    "age": 15 
}


HTTP/1.1 200 OK {
    "name" : null,
    "age" : 15
}
```
<br/>

따라서, `PUT`요청 시에는 변경하지 않은 속성값도 모두 전달해야 한다.
```JAVA
PUT /users/1
{
    "name" : "홍길동"
    "age" : 15
}


HTTP/1.1 200 OK {
    "name" : "홍길동",
    "age" : 15
}
```
<br/>

그러나 `PATCH`를 이용하여 나이(age)만 변경하는 요청을 보내면, 새롭게 바뀐 부분만 반영되며 나머지는 기존의 데이터가 유지되어야 한다.
```JAVA
PATCH /users/1
{
    "age" : 15
}


HTTP/1.1 200 OK {
    "name" : "홍길동",
    "age" : 15
}
```
<br/>

따라서, 자원의 일부를 수정할 때는 `PATCH`를, 전체적인 수정이 필요할 때는 `PUT`을 이용해야 한다.
<br/>

---
<br/>

## 8. HTTP 상태 코드
<br/>

<table>
 <tr>
  <th>구분</th>
  <th>Status Code</th>
  <th>사용 경우</th>
  <th>비고</th>
 </tr>
 <tr>
  <td rowspan="2">Success</td>
  <td>200 OK</td>
  <td>서버가 클라이언트의 요청을 성공적으로 수행하고 응답 함.</td>
  <td>대부분 요청 성공 시 적용</td>
 </tr>
 <tr>
  <td>201 Created</td>
  <td>요청의 결과 새로운 리소스가 <b>바로</b> 만들어진 경우 Location 헤더를 이용해서 만들어진 리소스의 URI를 포함해야 함.</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="9">Failure</td>
  <td>301 Moved Permanently</td>
  <td>접속한 주소가 영영 다른 위치로 옮겨진 경우 Location 헤더로 다른 위치를 알려줘야 함</td>
  <td></td>
 </tr>
 <tr>
  <td>304 Not Modified</td>
  <td>200 을 받아간 뒤로 바뀐 부분이 없는데 또 요청이 온 경우</td>
  <td></td>
 </tr>
 <tr>
  <td>400 Bad Request</td>
  <td>요청 URI, Header 또는 Body 정보에 오류가 있는 경우</td>
  <td>예) 필수 파라메터가 없거나, 문법 오류가 있거나, 잘못된 형식의 요청이거나, 전달 인자가 잘못되어 구문을 인식하지 못하는 경우 등</td>
 </tr>
 <tr>
  <td>401 Unauthorized</td>
  <td>로그인이 안된 경우</td>
  <td>예) 로그인조차 하지 않아서 접근할 수 없는 상태, 아이디나 비밀번호가 틀려서 로그인 실패 시</td>
 </tr>
 <tr>
  <td>403 Forbidden</td>
  <td>로그인을 했지만, 권한이 없는 경우</td>
  <td>예) 로그인을 해서 인증은 됐지만(User에 대한 정보는 확실하게 알고 있지만), 관리자의 권한이 없어서 접근할 수 없는 상태</td>
 </tr>
 <tr>
  <td>404 Not Found</td>
  <td>요청한 리소스 정보가 없는 경우</td>
  <td>예) 요청한 API URL이 존재하지 않는 경우</td>
 </tr>
 <tr>
  <td>405 Method Not Allowed</td>
  <td>클라이언트의 요청이 허용되지 않는 메소드 인 경우</td>
  <td>예) GET 메소드인 API를 POST로 호출하는 경우</td>
 </tr>
 <tr>
  <td>409 Conflict</td>
  <td>클라이언트의 요청이 서버의 상태와 충돌이 발생한 경우</td>
  <td>예) 주로 POST 요청 시 이미 존재하는 자원이 있는 경우에 대한 응답으로 사용. 이미 회원가입이 되어있는데, 또 회원가입을 시도하는 경우 (= 중복된 아이디 또는 이메일인 경우)</td>
 </tr>
 <tr>
  <td>409 Conflict</td>
  <td>클라이언트의 요청이 서버의 상태와 충돌이 발생한 경우</td>
  <td>예) 주로 POST 요청 시 이미 존재하는 자원이 있는 경우에 대한 응답으로 사용. 이미 회원가입이 되어있는데, 또 회원가입을 시도하는 경우 (= 중복된 아이디 또는 이메일인 경우)</td>
 </tr>
 <tr>
  <td>500 Internal Server Error</td>
  <td colspan="3">서버 오류를 총칭하는 오류 코드로, 요청을 처리하는 과정에서 서버가 예상하지 못한 상황에 놓인 상태인 경우</td>
 </tr>
 <tr>
  <td>503 Service Unavailable</td>
  <td colspan="3">서비스 점검 중 서버가 요청을 처리할 준비가 되지 않은 경우</td>
 </tr>
</table>
<br/>

---
<br/>

## 9. REST API Rule
<br/>

### 9-1. URL에 spinal-case를 사용하자
<br/>

**Bad:**
```Java
  [GET] /systemOrders 
  [GET] /system_orders
```
**Good:**
```Java
  [GET] /system-orders
```
<br/>

### 9-2. Path Variable에는 camelCase를 사용하자
<br/>

주문 내역 조회 URL을 만든다고 가정한다. 주문 내역이라는 Orders 하위에 orderId가 있을 것이다.<br/>
Path Variable에 해당하는 orderId는 camelCase를 사용한다.
<br/>

**Bad:**
```Java
  [GET] /orders/{order_id} 
  [GET] /orders/{OrderId}
```
**Good:**
```Java
  [GET] /orders/{orderId}
```
<br/>

### 9-3. Collection에는 단수가 아닌 복수를 사용하자
<br/>

사용자 전체 조회하는 URL에 User의 집합이므로 복수를 사용한다.
<br/>

**Bad:**
```Java
  [GET] /user
```
**Good:**
```Java
  [GET] /users
```
<br/>

### 9-4. Collection으로 시작해서 Identifier로 끝나자
<br/>

**Bad:**
```Java
  [GET] /shops/{shopId}/category/{categoryId}/price
```
리소스 대신에 `price`라는 속성을 가리키기 때문에 권장하지 않는다.
**Good:**
```Java
  [GET] /shops/{shopId}
  [GET] /categories/{categoryId}
```
상점 목록이라는 Collection으로 시작해서 하위에 속한 하나의 `shopId`라는 Identifier로 끝난다.
<br/>

### 9-5. 동사보다 명사를 사용하자
<br/>

HTTP Method를 이용해서 CRUD(생성, 읽기, 수정, 삭제)를 수행한다는 것을 알 수 있어야 한다.
<br/>

**Bad:**
```Java
  [POST] /updateuser/{userId} 
  [GET]  /getusers
```
**Good:**
```Java
  [PUT]  /users/{userId}
```
<br/>

### 9-6. Non-Resource URL에는 동사를 사용하자
<br/>

특정 작업을 하고 응답으로 아무것도 하지 않는 경우 URL에 동사를 사용할 수 있다.
사용자에게 알림을 보내는 URL을 다음과 같이 쓸 수 있다.
<br/>

**Good:**
```Java
  [POST] /alerts/245743/resend
```
<br/>

### 9-7. 소문자를 쓰자
<br/>

RFC 3986(URI 문법 형식)은 URI 스키마와 호스트를 제외하고는 대소문자를 구별하도록 규정하고 있다.
대소문자를 섞어 쓰는 것은 가능하지만 URI를 기억하기 어려우며 실수할 가능성이 높아진다. 
URI는 소문자로 작성 하자.
<br/>

**Bad:**
```Java
  [GET] /users/1/PROFILE
```
**Good:**
```Java
  [GET] /users/1/profile
```
<br/>

### 9-8. HTTP 메서드로 표현하자
<br/>

HTTP 메서드를 통해서 CRUD 중 무엇을 수행하는지 알 수 있어야 한다.
* `GET` 리소스 정보를 조회한다.
* `POST` 새로운 리소스를 생성한다.
* `PUT` 존재하는 리소스를 수정한다.
* `PATCH` 제공된 필드를 수정한다. 제공된 필드만 수정하고 나머지 필드는 유지한다.
* `DELETE` 존재하는 리소스를 삭제한다.
<br/>

**Bad:**
```Java
  [GET] /posts/update/1
  [GET] /users?method=update&id=covenant
```
**Good:**
```Java
  [PUT] /posts/1
```
**예시:**
* **`GET` /shops/2/products** shop 2에서 판매하는 모든 상품을 조회합니다.
* **`GET` /shops/2/products/31** shop 2에서 판매하는 31번 상품을 조회합니다.
* **`DELETE` /shops/2/products/31** shop 2에서 판매하는 31번 상품을 삭제합니다.
* **`PUT` /shops/2/products/31** shop 2에서 판매하는 31번 상품의 정보를 수정합니다.
* **`POST` /shops** 새로운 shop을 생성합니다.
<br/>

### 9-9. 적절한 상태 값을 응답하자
<br/>

잘 설계된 REST API는 URI만 잘 설계된 것이 아니라 리소스에 대한 응답을 잘 전달해야한다.
정확한 응답의 상태코드는 많은 정보를 전달할 수 있다.
<br/>

또한 에러 내용에 대한 상세 내용을 response body에 정의해서 상세한 에러 원인을 전달하는 것이 디버깅에 좋다.
<br/>

다음은 Kakao Open API에서 카카오 로그인 응답 예시다.
상태 메시지를 통하여 400 Status Code를 응답한 이유를 담고 있다.
(참고. "developers.kakao": https://developers.kakao.com/docs/latest/ko/kakaologin/rest-api#request-code)
<br/>

```Java
HTTP/1.1 400 Bad Request
{
    "msg":"user property not found ([gender, age] for appId={APP_ID})",
    "code":-201
}
```
<br/>

### 9-10. JSON property에는 camelCase 를 사용하자
<br/>

**Bad:**
```Java
{
    "user_id": "1"   
    "user_name": "Covenant" 
}
```
**Good:**
```Java
{
   "userId": "1" 
   "userName": "Covenant" 
}
```
<br/>

### 9-11. URI에 파일 확장자를 포함하지 않는다.
<br/>

URL에 파일 확장자를 포함하지 않는 대신에 클라이언트가 허용할 수 있는 파일 형식 정보가 있는 Accept header를 사용한다.
<br/>

**Bad:**
```Java
  [GET] /users/1/profile.png
```
**Good:**
```Java
  [GET] /users/1/profile-img
  Accept: image/jpg
```
<br/>

### 9-12. API 버전을 위해서 서수를 사용하자
<br/>

하위 호한성을 위하여 API에 버전을 넣어야 한다.
새로운 기능이 들어간 API를 제공할 때 이전 API와 구분하기 위해서 서수를 사용하여 URL의 상단의 경로에 표기한다.
(참고.  "developers.kakao": https://developers.kakao.com/docs/latest/ko/kakaologin/rest-api)
<br/>

**Good:**
```Java
  https://kapi.kakao.com/v2/user/me
```
<br/>

### 9-13. 리스트 응답의 경우 파라미터로 limit과 offset 정보를 받자
<br/>

다수의 정보를 조회하는 경우 limit, offset의 정보를 파라미터로 받아서 응답해야한다.
API를 응답 받기 전까지 몇 개의 리스트가 내려올지 알지 못하기 때문에 하나의 API 호출로 1만개(640kb)를 응답한 적도 있다. 
이 경우, 이용자가 많다면 서비스 응답 시간이 오래 걸리거나 언제 죽더라도 이상하지 않을 것이다.
`limit`, `offset` 값을 이용하여 프론트에서 페이징을 구현할 수 있습니다.
<br/>

**Good:**
```Java
  [GET] /shops?offset=5&limit=5
```
<br/>

### 9-14. 인증 토큰을 URL에 노출하지 말자
<br/>

인증 토큰은 요청 Header에 포함시키는 것이 좋다. 또한, 토큰에 짧은 유효 기간을 부여하여 탈취에 대비해야 한다.
<br/>

**Bad:**
```Java
  [GET] /shops/123?token=some_kind_of_authenticaiton_token
```
**Good:**
```Java
  Authorization: Bearer xxxxxx, Extra yyyyy
```
<br/>

### 9-15. URI의 마지막 문자로 슬래시(/)를 포함하지 말자
<br/>

url의 마지막에 슬래시(Trailing slash)가 붙어 있다면 이는 리소스가 디렉토리라는 의미다.
<br/>
**Bad:**
```Java
  [GET] /users/1/profile/
```
**Good:**
```Java
  [GET] /users/1/profile
```
<br/>

---
<br/>

## 10. Reference
<br/>

* "22 Best Practices to Take Your API Design Skills to the Next Level": https://betterprogramming.pub/22-best-practices-to-take-your-api-design-skills-to-the-next-level-65569b200b9
* "REST API Tutorial REST Resource Naming Guide": https://restfulapi.net/resource-naming/
* "RESTful API Designing guidelines — The best practices": https://wayhome25.github.io/etc/2017/11/26/restful-api-designing-guidelines/
* "REST API 디자인 가이드": https://bcho.tistory.com/914
* "REST API에서의 HTTP 상태 코드, 상태 메시지": https://jaeseongdev.github.io/development/2021/04/22/REST_API%EC%97%90%EC%84%9C%EC%9D%98_HTTP_%EC%83%81%ED%83%9C%EC%BD%94%EB%93%9C_%EC%83%81%ED%83%9C%EB%A9%94%EC%8B%9C%EC%A7%80.md

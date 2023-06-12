# [CORS]에 대처하는 방법과 우회하는 방법

외부 서버로 ajax 요청이 안될 경우 아래와 같은 단계로 처리를 생각해 볼 수 있다.

<br>

1. 개발자가 테스트 혹은 개발단계에서 쉽게 요청하기: [웹 브라우저 실행 옵션](#웹브라우저실행옵션)이나 [플러그인](#플러그인)을 통한 [동일 출처 정책](#동일출처정책)회피
2. CORS구현이 안되어 있는 서버로 ajax요청을 해야하지만 서버 쪽 컨트롤이 불가능할 경우:[json방식](#jsonp방식)으로 요청
3. Ajax요청을 해야 하는 다른 도메인의 서버를 클라이언트와 같이 개발하거나 서버 개발 쪽 수정 요청이 가능한 경우:[서버에서](#서버에서) CORS 요청이 허용되도록 구현

<br>

---

# 용어

### CORS

- CORS(Cross-Origin Resource Sharing)란? 웹 브라우저에서 외부 도메인 서버와 통신하기 위한 방식을 표준화한 스펙이다. 서버와 클라이언트가 정해진 헤더를 통해 서로 요청이나 응답에 반응할지 결정하는 방식으로 교차 출처 자원 공유(cross-origin resource sharing)라는 이름으로 표준화 되어있다.

### 웹브라우저실행옵션

- (용어 설명 x) 웹브라우저 실행 시 외부 요청을 허용하는 옵션을 사용, same orgin policy는 결국 클라이언트인 웹 브라우저가 요청을 해도 되즈닌지 판단해서 결정하는 것으로 이 과정만 무시한다면 어디든 요청하지 못할 이유는 없다. 크롬같은 웹 브라우저들은 실행 시 커맨드 라인 옵션을 통해서 외부 도메인 요청가능 여부를 확인하는 동작을 무시하게 할 수 있다.

> ### 크롬의 경우
>
> `--disable-web-security`옵션을 추가하여 크롬 실행

### 플러그인

- (용어 설명 x) 외부 요청을 가능하게 해주는 플러그인 설치, 서버에서 받은 요청의 응답에 특정 header(`Access-Control-Allow-Origin: *`)만 추가하면 웹 브라우저가 요청이 가능한 사이트로 인식하여 요청이 가능하다. 크롬의 경우 웹스토어에 요청을 가로채서 응답에 위 header를 추가해주는 플러그인이 있다. 웹스토어에서 cors로 검색하면 확장 프로그램 검색 결과에서 찾을 수 있다.

### 동일출처정책

- 동일 출처 정책(Same-Origin Policy)란? 어떤 출처에서 불러온 문서나 스크립트가 다른 출처에서 가져온 리소스와 상호작용하는 것을 제한하는 중요한 보안 방식이다. 동일 출처 정책은 잠재적으로 해로울 수 있는 문서를 분리함으로써 공격받을 수 있는 경로를 줄여준다.

### jsonp방식

- 웹 브라우저에서 css나 js 같은 리소스 파일들은 동일 출처 정책에 영향을 받지 않고 로딩이 가능하다. 이런 점을 응용해서 외부 서버에서 읽어온 js파일을 json으로 바꿔주는 일종의 편법적인 방식이디. 단점은 리소스 파일을 GET 메서드로 읽어오기 때문에 GET 방식의 API만 요청이 가능하다.

### 서버에서

- (용어)X 서버에서 CORS 요청 핸들링하기, 서버로 날아온 preflight 요청을 처리하면 웹 브라우저에서 실제 요청을 날릴 수 있도록 해준다.

### 모든 외부 도메인에서 모든 요청을 허용할 경우 처리

가장 쉬운 방법으로 모든 요청을 허용하는 방식이다.
preflight 요청을 받기 위해 OPTIONS 메서드의 요청을 받아서 컨트롤해야 한다. 모든 요청의 응답에 아래 header를 추가한다.

```js
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET,POST,PUT,DELETE,OPTIONS
Access-Control-Max_Age: 3600
Access-Control-Allow-Headers: Origin,Accept,X-requested-With,Content-Type,Access-Control-Request-Method,Access-
Control-Request-Headers,Authorization
```

`Access-Control-Allow-Origin`: 요청을 허용하는 출처, `*`이면 모든 곳에 공개되어 있음을 의미한다.
`Access-Control-Allow-Methods`: 요청을 허용하는 메서드, 기본값은 `GET`,`POST`라고 보면 된다. 이 헤더가 없으면 `GET`과 `POST`요청만 가능하다. 만역 이 헤더가 지정되어 있으면, 클라이언트는 헤더값에 해당하는 메서드일 경우에만 실제 요청을 시도하게 된다.
`Access-Control-Max_Age`: 클라이언트에서 preflight의 요청 결과를 저장할 기간을 지정. 클라이언트에서 preflight 요청의 결과를 저장하고 있을 시간이며 해당 시간 동안은 preflight 요청을 다시 하지 않게 된다.
`Access-Control-Allow_Headers`: 요청을 허용하는 헤더

웹 브라우저의 스크립트 엔진에서 preflight 요청 응답으로 `Access-Control-Allow-Origin` header에 `"*"`값이 있으면 모든 도메인에서의 요청을 허용하는 것으로 판단하다. ajax요청이 실패하면서 발생하는 메시지는 바로 preflight요청을 날린 응답 메시지에 `Access-Control-Allow-Origin` 헤더가 없어서 요청이 허용되지 않는다는 뜻이다.

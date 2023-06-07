# null, undefined, undeclared, NaN

## null

--

- 빈 값
- null이라는 빈 값을 할당했을 때, 생기는 타입이다.

## undefined

--

- 정의되지 않음
- `var` 선언문의 경우, 호이스팅 되었을 때, 변수 선언과 초기화가 동시에 일어나기 때문에, 변수가 `undefined`된다(타입 결정 안됨)

```javascript
console.log(data); // undefined
const data = "data";
```

## undeclared

--

- 선언되지 않음
- `let`,`const`선언문의 경우, [호이스팅](#호이스팅) 되었을 때, 변수 선언과 초기화가 따로 이루어지기에, 변수가 `undecalred`되어 에러가 생깁니다.

```javascript
console.log(data); // error
let data = "data";
```

## NaN

--

- 표현 불가능한 수치형 결과입니다.

```javascript
typeof 1 / 0; //NaN
```

---

# 용어 공부

## 호이스팅

- 호이스팅이란"끌어올린다"라는 뜻으로 **변수 및 선언문이 스코프 내의 최상단으로 끌어올려지는 현상** 을 말한다. 여기서 주의할 점은 **"선언문"** 이라는 것이며 "대입문"은 끌어올려지지 않는다.

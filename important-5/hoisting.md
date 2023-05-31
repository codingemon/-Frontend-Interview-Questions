# 호이스팅(hoistiong)이란?

--
호이스팅이란 "끌어오린다" 라는 뜻으로 **변수 및 함수 선언문이 스코프 내의 최상단으로 끌어올려지는 현상** 을 말한다. 여기서 주의할 점은 **"선언문"**이라는 것이며"대입문"은 끌어올려지지 않는다.

<hr>

> ### 모범 답변([medium 링크 참고](https://medium.com/@limsungmook/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%99%9C-%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85%EC%9D%84-%EC%84%A0%ED%83%9D%ED%96%88%EC%9D%84%EA%B9%8C-997f985adb42))
>
> 실행 컨텍스트 생성 시 렉시컬 스코프 내의 선언이 끌어올려 지는 게 호이스팅이다.

---

```javascript
console.log(a);
var a = 2;
```

컴파일러는 javascript 엔진이 [인터프리팅](#gear-인터프리팅)을 하기전에 컴퍼일을 하는데 이때, `var a = 2;`를 2개의 구문으로 본다.

-`var a` -`a = 2`

`var a `는 변수 선언문으로 컴퍼일을 할 때 처리하고,`a = 2`는 실행할 때까지 내버려둔다.따라서, 변수 a는 호이스팅 되고 콘솔에는 다음과 같은 결과가 나온다.

```javascript
undefined;
```

> var는 선언, 초기화가 동시에 이루어지기 때문에 undefined를 출력하지만 let,const는 선언단계만 호이스팅 되기 때문에 Reference Error를 출력한다.

[함순선언문](#gear-함수선언문)의 경우도 호이스팅 된다.

```javascript
func();
function func() {
  console.log("함수 호이스팅");
}
```

`함수 호이스팅`

> 함수 호이스팅에서 주의할 점: [함수표현식](#gear-함수표현식)은 호이스팅 되지 않는다.

---

## :hammer_and_wrench: 용어 공부

### :gear: 인터프리팅

- 인터프리팅(InterPreting)은 컴파일과 다르게 소스 코드를 한 번에 읽어서 번역하지 않고,런타임 상태의 소스코드를 한 줄 한 줄 번역하면서 프로그램을 구동하는 방식이다.번역한 코드가 바로 기계어가 되는 것이 아니며 중간 코드(intermediate code)로 번역된다.이 중간 코드는 다른 프로그램에 의하여 기계어로 번역되어 실행된다.

### :gear: 함수선언문

- 변수를 선언하는 것(const,let 등)처럼 함수 선언은 function으로 시작한다. 선언 된 함수는 나중 사용을 위해 저장되며, call될 때 실행된다.

### :gear: 함수표현식

- javascript 함수는 표현식을 사용하여 정의 될 수 있으며,함수 표현식은 변수로 저장될 수 있다.

```js
var x = function (a, b) {
  return a * b;
};
```

함수 표현식이 변수에 저장되면, 변수는 함수처럼 사용 가능해진다. 변수에 저장된 함수는 함수명이 필요 없으며, 변수 이름을 통하여 호출된다.

<br>
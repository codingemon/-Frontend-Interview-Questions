# 이벤트 전파

생성된 이벤트 객체는 이벤트를 발생시긴 DOM 요소인 이벤트 타깃을 중심으로 DOM트리를 통해 전파된다.

<br>

> ### [이벤트 버플링]()
>
> 이벤트가 하위 요소에서 상위 요소 방향으로 전파

<br>

> ### [이벤트 캡처링]()
>
> 이벤트가 상위 요소에서 하위 요소 방향으로 전파

<br>

> - 이벤트 버블링과 캡처링을 막기 위해서 [e.stopPropagation()]()을 사용한다. 해당 웹 API를 통해 이벤트가 전파되는 것을 막을 수 있다

<br>

> ### 타깃 단계
>
> 이벤트가 이벤트 타깃에 도달

<br>

### [이벤트 위임](): 이벤트 버블링 활용하기

이벤트 위임을 사용하지 않고, 동일한 이벤트를 일일히 수동으로 달아주기에는 코드 낭비가 너무 심하게된다.

따라서 부모 요소에 이벤트를 부여해 버블링을 통해 하위 요소를 동작시킬때도 해당 이벤트가 발생하도록 만드는 것이 바람직하다.

<br>

아래와 같은 상황에서

```html
<div class="itemList">
  <li>
    <input type="checkbox" id="item1" />
    <label for="item1">1</label>
  </li>
  <li>
    <input type="checkbox" id="item2" />
    <label for="item1">2</label>
  </li>
</div>
```

- case1: 각각 이벤트들을 부여해주기 inputs의 내부 input에 각각 이벤트를 달아주었다.

```javascript
let inputs = document.querySelectorAll("input");
inputs.forEach((input) => {
  input.addEventListener("click", () => {
    alert("clicked");
  });
});
```

이처럼 이벤트 버블링을 통한 이벤트 위임은 하위 요소에 각각의 이벤트를 붙이지 않고도 상위 요소에서 하위 요소의 이벤트들을 제어할 수 있다.

---

# 용어 공부

## 이벤트버블링

- 이벤트 버블링(Event Bubbling)은 특정 화면 요소에서 이벤트가 발생했을 때 해당 이벤트가 더 상위의 화면 요소들로 전달되어 가는 특성을 의미한다. (아래와 같은 그림처럼)
  <img src="/img/event-bubbling.png" width="600px">

> 하위의 클릭 이벤트가 상위로 전달되어 가는 그림

## 이벤트캡처링

- 이벤트 캡처링(Event Capturing)은 이벤트 버블링과 반대 방향으로 진행되는 이벤트 전파 방식이다.
  <img src="/img/event-capturing.png" width="600px">

> 클릭 이벤트가 발생한 지점을 찾아내려 가는 그림

```html
<div class="outside">
  녹색 영역
  <div class="middle">
    하늘색 영역
    <div class="inner">
      핑크색 영역
      <div class="float">회색</div>
    </div>
  </div>
</div>
```

```js
const outside = document.querySelector(".outside");
const middle = document.querySelector(".middle");
const inner = document.querySelector(".inner");
const float = document.querySelector(".float");

function callback() {
  alert(this.className + " is Capturing ");
}

outside.addEventListener("click", callback, true);
middle.addEventListener("click", callback, true);
inner.addEventListener("click", callback, true);
float.addEventListener("click", callback, true);
```

위 코드는 이벤트 캡처링의 예시이다. `float`을 크릭하면 `가장 상위 부모요소`인 `outside`의 이벤트부터 발생한다. 이때 `addEventListener`함수의 `두번째 인자`로 전달된 `true`는 `이벤트를 캡처링해야하는지 여부`를 지정한다.

```js
target.addEventListener(type, listener[, useCapture]);
```

- `type`: 이벤트의 이름을 지정하는 문자열. 대소문자 구별.(click, keypress등...)
- `listener`: 이벤트가 발생할때 호출할 이벤트 리스너 함수
- `useCapture`: 캡쳐링을 사용할지 여부를 지정하는 Boolean. default는 false 이다.(선택사항)

## 이벤트위임

- 캡처링과 버블링을 활용하면 강력한 이벤트 핸들링 패턴인 이벤트 위임(event delegation)을 구현할 수 있다. 이벤트 위임은 비슷한 방식으로 여러 요소를 다뤄야 할 때 사용된다. 이벤트 위임을 사용하면 요소마다 핸들러를 할당하지 않고, 요소의 공통 조상에 이벤트 핸들러를 단 하나만 할당해도 여러 요소를 한꺼번에 다룰 수 있다. 공통 조상에 할당한 핸들러에서 `event.target`을 이용하면 실제 어디서 이벤트가 발생했는지 알 수 있다. 이를 이용해 이벤트를 핸들링한다.

## stopPropagation

- “난 이렇게 복잡한 이벤트 전달 방식 알고 싶지 않고, 그냥 원하는 화면 요소의 이벤트만 신경 쓰고 싶어요.”라고 생각하시는 분들이 충분히 있을 수 있다. 실제로 마감 기한에 쫓기는 상황에서 이런 동작 방식을 정확히 이해하는 시간보다는 구현에 더 많은 시간을 쏟아야 하기 때문이다. 그럴 때는 아래처럼 stopPropagation() 웹 API를 사용한다.

```js
function logEvent(event) {
  event.stopPropagation();
}
```

위 API는 해당 이벤트가 전파되는 것을 막는다. 따라서, 이벤트 버블링의 경우에는 클릭한 요소의 이벤트만 발생시키고 상위 요소로 이벤트를 전달하는 것을 방해한다. 그리고 이벤트 캡쳐의 경우에는 클릭한 요소의 최상위 요소의 이벤트만 동작시키고 하위 요소들로 이벤트를 전달하지 않는다

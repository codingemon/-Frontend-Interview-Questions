# React의 생명 주기(라이프 사이클)

리액트는 컴포넌트 기반의 View를 중심으로 한 라이브러리이다. 그러다보니 각각의 컴포넌트에는 라이프 사이클 즉, 컴포넌트의 생명 주기가 존재한다. 컴포넌트의 생명은 보통 페이지에서 렌더링되기 전인 준비 과정에서 시작하여 페이지에서 사라질 때 끝이 난다.

<br>

<img src="/img/react-lifecycle.png" width="600px">

> 위의 이미지는 리액트의 생명 주기를 나타낸 화면이다.
> 이미지에서 볼 수 있듯이 컴포넌트는 `생성 => 업데이트 => 제거`의 생명 주기를 갖고 있다.
> 그럼 이제 각각의 라이프 사이클이 무엇이고 어떻게 Class와 Hooks를 활용한 함수형 컴포넌트에서 사용하는지 알아보도록 하겠다.
> 아래 목록에서 자주 사용되는 메서드는 `코드블럭`으로 표시하겠다. 나머지는 상대적으로 덜 사용된다.

<br>

## 마운트(생성)

컴포넌트의 [인포넌트]()가 생성되어, DOM에 삽입될 때 순서대로 호출된다.

- `constructor()`
- getDerivedStateFromProps()
- `render()`
- `componenetDidMount()`

<br>

## 업데이트

prop나 state가 변경되면 렌더(갱신)가 진쟁되며 순서대로 호출한다.

- getDerivedStateFromProps()
- shouldComponentUpdate()
- `render()`
- getSnapshotBeforeUpdate()
- `componentDidUpdate()`

<br>

## 마운트 해제(제거)

아래 메서드는 컴포넌트가 DOM에서 제거될 때 호출된다.

- `componentWillUnmount()`

<br>

위에 명시된 자주 사용되는 생명 주기 메서드에 대해 간략한 소개를 하겠다.

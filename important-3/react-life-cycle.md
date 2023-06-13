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

<br>

### `render()`

클래스 컴포넌트에 반드시 구현되어야 하는 유일할 메서드이다.

- 이 메서드가 호출되면 this.props와 this.state의 값을 활용하여 값을 반환한다.
- render()함수는 컴포넌트의 state를 변경하지 않고, 호출될 때마다 동일한 결과를 반환하며 브라우저와 직접적인 상호작용을 하지 않는다.

```js
// Class
class Example extends React.Componect {
  render() {
    return <div>컴포넌트</div>;
  }
}

// Hooks
const example = () => {
  return <div>컴포넌트</div>;
};
```

> 함수형 컴포넌트에서는 render를 안쓰고 컴포넌트를 렌더링할 수 있다.

<br>

### `constructor(props)``

메서드를 바인딩하거나 state를 초기화하는 작업이 없다면 constructor(생성자)가 없어도 된다.

- react 컴포넌트의 생정자는 해당 컴포넌트가 마운트되기 전에 호출된다.
- 생정가를 구현하면 this.props가 생성자 내에서 정의되도록 super(props)를 호출해야 한다.
- state의 값을 변경하고자 한다면 constructor() 외부에서 this.setState()를 통해 수정해야 한다.

```js
// Class
class Example extends React.Componect {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }
}

// Hooks
const Example = () => {
  const [count, setCount] = useState(0);
};
```

> 클래스형에서는 초기 state를 정할 때 constructor를 사용해야 한다. 하지만 혹에서는 useState hook을 사용하면 초기 상태를 쉽게 설정해줄 수 있다.

<br>

### `componentDidMount()`

컴포넌트가 마운트된 직후에 호출된다.

- DOM 노드가 있어야 하는 초기화 작업이 이루어지면 좋다
- 외부에서 데이터를 불러와야 한다면 네트워크 요청을 보내기 좋은 위치이다.

```js
// Class
class Example extends React.Component {
    componentDidMount(){
        ...
    }
}

// Hooks
const Example = () => {
    useEffect(() => {
        ...
    }, [])
}
```

> 함수형 Hooks 에서는 useEffect의 []의존성 배열을 비워야지만 똑같은 메소드를 구현할 수 있다.

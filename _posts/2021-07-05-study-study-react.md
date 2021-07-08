---
layout: post
title:  "[React] React tic-tac-toe 연습"
subtitle:   "혼자 공부해보는 React"
categories: study
tags: study
comments: true

---

> 혼자 공부해보는 React



# React 입문

## 시작하기 전에...

* [Completed_Code](https://codepen.io/gaearon/pen/gWWZgR?editors=0010)

* [React_Tutorial](https://ko.reactjs.org/tutorial/tutorial.html#before-we-start-the-tutorial)



## 준비

* react-app 생성 후 실행

```bash
$ npx create-react-app my-app
$ cd my-app
$ npm start
```

> 주의! Node 10.16 혹은 상위 버전 및 npm 5.6 혹은 상위 버전이 필요합니다!
>
> 첫 번째 줄의 ‘npx’는 실수가 아니며 [npm 5.2+ 버전의 패키지 실행 도구입니다](https://medium.com/@maybekatz/introducing-npx-an-npm-package-runner-55f7d4bd282b).



* src에 있는 모든 파일을 삭제



* src/index.css 생성 후 아래 내용 입력

```css
body {
  font: 14px "Century Gothic", Futura, sans-serif;
  margin: 20px;
}

ol, ul {
  padding-left: 30px;
}

.board-row:after {
  clear: both;
  content: "";
  display: table;
}

.status {
  margin-bottom: 10px;
}

.square {
  background: #fff;
  border: 1px solid #999;
  float: left;
  font-size: 24px;
  font-weight: bold;
  line-height: 34px;
  height: 34px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 34px;
}

.square:focus {
  outline: none;
}

.kbd-navigation .square:focus {
  background: #ddd;
}

.game {
  display: flex;
  flex-direction: row;
}

.game-info {
  margin-left: 20px;
}
```



* src/index.js 생성 후 아래 내용 입력

```react
import React from 'react';
import ReactDom from 'react-dom';
import './index.css'

class Square extends React.Component {
  render() {
    return (
      <button className="square">
        {/* TODO */}
      </button>
    );
  }
}

class Board extends React.Component {
  renderSquare(i) {
    return <Square />;
  }

  render() {
    const status = 'Next player: X';

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}

class Game extends React.Component {
  render() {
    return (
      <div className="game">
        <div className="game-board">
          <Board />
        </div>
        <div className="game-info">
          <div>{/* status */}</div>
          <ol>{/* TODO */}</ol>
        </div>
      </div>
    );
  }
}

// ========================================

ReactDOM.render(
  <Game />,
  document.getElementById('root')
);
```



## Props를 통해 데이터 전달

* Board의 renderSquare 함수 코드 수정

```react
class Board extends React.Component {
  renderSquare(i) {
    return <Square value={i} />;
  }
}
```



* 값을 표시하기 위해 Square의 render 함수에서 `{/* TODO */}`를 `{this.props.value}`로 수정

```react
class Square extends React.Component {
  render() {
    return (
      <button className="square">
        {this.props.value}
      </button>
    );
  }
}
```

<img src="md-images/image-20210702163742650.png" alt="image-20210702163742650" style="zoom:33%;" />



## 사용자와 상호작용하는 컴포넌트 만들기

* Square 컴포넌트를 클릭하면 "X"가 체크되도록 Square 컴포넌트의 render() 함수에서 반환하는 버튼 태그 변경

```react
class Square extends React.Component {
  render() {
    return (
      <button className="square" onClick={function() { alert('click'); }}>
        {this.props.value}
      </button>
    );
  }
}
```

* arrow function으로 변경해보기

```react
class Square extends React.Component {
  render() {
    return (
      <button className="square" onClick={() => alert('click')}>
        {this.props.value}
      </button>
    );
  }
}
```



* Square 컴포넌트를 클릭한 것을 "기억하게" 만들어 "X" 표시를 채워 넣기 위해 `state` 사용

* React 컴포넌트는 생성자에 this.state를 설정하는 것으로 state를 가질 수 있습니다. this.state는 정의된 React 컴포넌트에 대해 비공개로 간주해야 합니다. 이제 Square의 현재 값을 this.state에 저장하고 Square를 클릭하는 경우 변경하겠습니다.
* 우선 클래스에 생성자를 추가하여 state를 초기화합니다.

```react
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button className="square" onClick={() => alert('click')}>
        {this.props.value}
      </button>
    );
  }
}
```

> 주의! [JavaScript 클래스](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)에서 하위 클래스의 생성자를 정의할 때 항상 `super`를 호출해야합니다. 모든 React 컴포넌트 클래스는 `생성자`를 가질 때 `super(props)` 호출 구문부터 작성해야 합니다.



* Square를 클릭할 떄 현재 state 값을 표시하기 위해 render 함수를 변경
  * `<button>` 태그 안 `this.props.value`를 `this.state.value`로 변경해주세요.
  * `onClick={...}` 이벤트 핸들러를 `onClick={() => this.setState({value: 'X'})}`로 변경해주세요.
  * 가독성을 높이기 위해 `className`과 `onClick` props를 별도의 줄에 넣어주세요.

```react
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button
        className="square"
        onClick={() => this.setState({value: 'X'})}
      >
        {this.state.value}
      </button>
    ); 
  }
}
```

> 지금까지 작성한 코드로 어디를 눌러도 'X'가 나오는 것까지 완성되었습니다.



## 게임 완성하기

### State 끌어올리기

> Board가 각 Square에 Square의 state를 요청해야 한다고 생각할 수도 있습니다. 그리고 React에서 이런 접근이 가능하기는 하지만 이 방식은 코드를 이해하기 어렵게 만들고 버그에 취약하며 리팩토링이 어렵기 때문에 추천하지 않습니다. 각 Square가 아닌 부모 Board 컴포넌트에 게임의 상태를 저장하는 것이 가장 좋은 방법입니다

**여러개의 자식으로부터 데이터를 모으거나 두 개의 자식 컴포넌트들이 서로 통신하게 하려면 부모 컴포넌트에 공유 state를 정의해야 합니다. 부모 컴포넌트는 props를 사용하여 자식 컴포넌트에 state를 다시 전달할 수 있습니다. 이것은 자식 컴포넌트들이 서로 또는 부모 컴포넌트와 동기화 하도록 만듭니다.**

state를 부모 컴포넌트로 끌어올리는 것은 React 컴포넌트를 리팩토링할 때 흔히 사용합니다. 이번 기회에 시험해 보겠습니다.

```react
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      /* Board에 생성자를 추가하고 9개의 사각형에 해당하는 9개의 null 배열을 초기 state로 설정 */
      squares: Array(9).fill(null)
    }
  }

  renderSquare(i) {
    return <Square value={i}/>;
  }
```



처음에는 모든 Square에서 0부터 8까지 숫자를 보여주기 위해 Board에서 [`value` prop을 자식으로 전달했습니다](https://ko.reactjs.org/tutorial/tutorial.html#passing-data-through-props). 또 다른 이전 단계에서는 숫자를 [Square의 자체 state에 따라](https://ko.reactjs.org/tutorial/tutorial.html#making-an-interactive-component) “X” 표시로 바꾸었습니다. 그렇기 때문에 현재 Square는 Board에서 전달한 `value` prop을 무시하고 있습니다.

이제 prop을 전달하는 방법을 다시 사용할 것입니다. 각 Square에게 현재 값(`'X'`, `'O'`, 또는 `null`)을 표현하도록 Board를 수정할 것입니다. Board의 생성자에서 `squares` 배열을 이미 선언했으며 `renderSquare` 함수를 아래와 같이 수정할 것입니다.

```react
  renderSquare(i) {
    return <Square value={this.state.squares[i]} />;
  }
```



Square는 이제 빈 사각형에 `'X'`, `'O'`, 또는 `null`인 `value` prop을 받습니다.

다음으로 Square를 클릭할 때 발생하는 변화가 필요합니다. Board 컴포넌트는 어떤 사각형이 채워졌는지를 여부를 관리하므로 Square가 Board를 변경할 방법이 필요합니다. 컴포넌트는 자신이 정의한 state에만 접근할 수 있으므로 Square에서 Board의 state를 직접 변경할 수 없습니다.

대신에 Board에서 Square로 함수를 전달하고 Square는 사각형을 클릭할 때 함수를 호출할 것입니다. 이제 Board의 `renderSquare` 함수를 아래와 같이 변경해주세요.

```react
  renderSquare(i) {
    return (
      <Square 
      value={this.state.squares[i]}
      onClick={() => this.handleClick(i)}
      />
    );
  }
```

> 주의! 반환되는 엘리먼트를 여러 줄로 나누어 가독성을 확보하였고 괄호를 추가하여 JavaScript가 `return` 뒤에 세미콜론을 삽입하지 않아도 코드가 깨지지 않습니다.



* Square의 `render` 함수 내부의 `this.state.value`를 `this.props.value`로 바꾸어주세요.
* Square의 `render` 함수 내부의 `this.setState()`를 `this.props.onClick()`으로 바꾸어주세요.
* Square는 게임의 상태를 유지할 필요가 없기 때문에 `constructor`를 지워주세요.

```react
class Square extends React.Component {
  render() {
    return (
      <button
        className="square"
        onClick={() => this.props.onClick()}
      >
        {this.props.value}
      </button>
    );
  }
}
```

1. 내장된 DOM `<button>` 컴포넌트에 있는 `onClick` prop은 React에게 클릭 이벤트 리스너를 설정하라고 알려줍니다.
2. 버튼을 클릭하면 React는 Square의 `render()` 함수에 정의된 `onClick` 이벤트 핸들러를 호출합니다.
3. 이벤트 핸들러는 `this.props.onClick()`를 호출합니다. Square의 `onClick` prop은 Board에서 정의되었습니다.
4. Board에서 Square로 `onClick={() => this.handleClick(i)}`를 전달했기 때문에 Square를 클릭하면 `this.handleClick(i)`를 호출합니다.
5. 아직 `handleClick()`를 정의하지 않았기 때문에 코드가 깨질 것입니다. 지금은 사각형을 클릭하면 “this.handleClick is not a function”과 같은 내용을 표시하는 붉은 에러 화면을 보게됩니다.

> 주의! DOM `<button>` 엘리먼트의 `onClick` 어트리뷰트는 내장된 컴포넌트라는 점에서 React에게 특별한 의미가 있습니다. Square같은 사용자 정의 컴포넌트의 경우 이름 지정은 자유롭습니다. Square의 `onClick` prop이나 Board의 `handleClick` 함수에는 어떤 이름도 붙일 수 있으며 코드는 동일하게 작동합니다. React에서 이벤트를 나타내는 prop에는 `on[Event]`, 이벤트를 처리하는 함수에는 `handle[Event]`를 사용하는 것이 일반적입니다.



`handleClick`을 아직 정의하지 않았기 때문에 Square를 클릭하려고 할 때 에러가 발생합니다. 이제 Board 클래스에 `handleClick`을 추가하겠습니다.

```react
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      /* Board에 생성자를 추가하고 9개의 사각형에 해당하는 9개의 null 배열을 초기 state로 설정 */
      squares: Array(9).fill(null)
    }
  }

  handleClick(i) {
    const squares = this.state.squares.slice();
    squares[i] = 'X';
    this.setState({squares: squares});
  }

  renderSquare(i) {
    return (
      <Square 
      value={this.state.squares[i]}
      onClick={() => this.handleClick(i)}
      />
    );
  }

  render() {
    const status = 'Next player: X';

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}
```

> 이제 이전과 마찬가지로 Square를 클릭하여 사각형을 채울 수 있습니다. 그러나 이제는 state가 각 Square 컴포넌트 대신에 Board 컴포넌트에 저장됩니다. 



### 함수 컴포넌트

이제 Square를 **함수 컴포넌트**로 바꿔보겠습니다.

React에서 **함수 컴포넌트**는 더 간단하게 컴포넌트를 작성하는 방법이며 state 없이 `render` 함수만을 가집니다. `React.Component`를 확장하는 클래스를 정의하는 대신 `props`를 입력받아서 렌더링할 대상을 반환하는 함수를 작성할 수 있습니다. 함수 컴포넌트는 클래스로 작성하는 것보다 빠르게 작성할 수 있으며 많은 컴포넌트를 함수 컴포넌트로 표현할 수 있습니다.

```react
function Square(props) {
  return (
      <button className="square" onClick={props.onClick}>
        {props.value}
      </button>
  );
}
```



### 순서 만들기

초기 state를 수정해줍니다.

```react
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      /* Board에 생성자를 추가하고 9개의 사각형에 해당하는 9개의 null 배열을 초기 state로 설정 */
      squares: Array(9).fill(null),
      xIsNext: true,
    }
  }
```

카드를 뒤집습니다

```react
  handleClick(i) {
    const squares = this.state.squares.slice();
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      squares: squares,
      xIsNext: !this.state.xIsNext,
    });
  }
```

Board의 `render` 안에 있는 “status” 텍스트도 바꿔서 어느 플레이어가 다음 차례인지 알려주겠습니다.

```react
  render() {
    const status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
```



### 승자 결정하기

* 함수를 붙여넣습니다.

```react
function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```



* board의 render 함수에서 선언한 status를 아래 코드로 변경해주세요.

```react
  render() {
    const winner = calculateWinner(this.state.squares);
    let status;
    if (winner) {
      status = 'Winner: ' + winner;
    } else {
      status = 'Next Player: ' + (this.state.xIsNext ? 'X' : 'O');
    }
```



* 누군가가 승리하거나 Square가 이미 채워졌다면 Board의 `handleClick` 함수가 클릭을 무시하도록 변경하겠습니다.

```react
  handleClick(i) {
    const squares = this.state.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      squares: squares,
      xIsNext: !this.state.xIsNext,
    });
  }
```



## 시간 여행

### 동작에 대한 기록 저장하기

이전 동작에 대한 리스트를 보여주려면 최상위 단계의 Game 컴포넌트가 필요합니다. `history`를 이용해야하기 때문에 최상위 단계 Game 컴포넌트에 `history` state를 두겠습니다.

`history` state를 Game 컴포넌트에 두었기 때문에 자식 Board 컴포넌트에서 `squares` state를 더 이상 사용하지 않아도 됩니다. Square 컴포넌트에서 Board 컴포넌트로 [“state를 끌어올렸던 것”](https://ko.reactjs.org/tutorial/tutorial.html#lifting-state-up) 처럼 이번에는 Board에서 최상위 단계 Game 컴포넌트로 state를 끌어올렸습니다. 이를 통해 Game 컴포넌트는 Board의 데이터를 완벽히 제어할 수 있으며 `history`에 저장된 과거의 차례를 Board가 렌더링 할 수 있게 만듭니다.

우선 Game 컴포넌트의 생성자 안에 초기 state를 설정해주세요.

```react
class Game extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      history: [{
        squares: Array(9).fill(null),
      }],
      xIsNext: true,
    }
  }

  render() {
    return (
      <div className="game">
        <div className="game-board">
          <Board />
        </div>
        <div className="game-info">
          <div>{/* status */}</div>
          <ol>{/* TODO */}</ol>
        </div>
      </div>
    );
  }
}
```



다음으로 Game 컴포넌트에서 Board 컴포넌트로 `squares`와 `onClick` props를 전달하겠습니다. Board에서 여러 개의 Square에 쓰일 단일 클릭 핸들러를 가졌기 때문에 각 Square의 위치를 `onClick` 핸들러에게 넘겨주어 어떤 Square를 클릭했는지 표시할 것입니다. Board 컴포넌트를 변경하는 순서는 아래와 같습니다.

- `constructor`를 Board에서 제거해주세요.
- Board의 `renderSquare` 안의 `this.state.squares[i]`를 `this.props.squares[i]`로 바꿔주세요.
- Board의 `renderSquare` 안의 `this.handleClick(i)`을 `this.props.onClick(i)`으로 바꿔주세요.

이제 Board 컴포넌트는 아래와 같은 형태입니다.

```react
class Board extends React.Component {
  handleClick(i) {
    const squares = this.state.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      squares: squares,
      xIsNext: !this.state.xIsNext,
    });
  }

  renderSquare(i) {
    return (
      <Square 
      value={this.props.squares[i]}
      onClick={() => this.props.onClick(i)}
      />
    );
  }

  render() {
    const winner = calculateWinner(this.state.squares);
    let status;
    if (winner) {
      status = 'Winner: ' + winner;
    } else {
      status = 'Next Player: ' + (this.state.xIsNext ? 'X' : 'O');
    }

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}
```



Game 컴포넌트의 `render` 함수를 가장 최근 기록을 사용하도록 업데이트하여 게임의 상태를 확인하고 표시하겠습니다.

```react
  render() {
    const history = this.state.history;
    const current = history[history.length - 1];
    const winner = calculateWinner(current.squares);
    let status;
    if (winner) {
      status = 'Winner: ' + winner;
    } else {
      status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
    }

    return (
      <div className="game">
        <div className="game-board">
          <Board 
            squares={current.squares}
            onClick={(i) => this.handleClick(i)}
          />
        </div>
        <div className="game-info">
          <div>{status}</div>
          <ol>{/* TODO */}</ol>
        </div>
      </div>
    );
  }
}
```



Game 컴포넌트가 게임의 상태를 렌더링하기 때문에 Board의 `render` 함수에서 중복되는 코드를 제거할 수 있습니다. 리펙토링 이후에 Board의 `render` 함수는 아래와 같습니다.

```react
  render() {
    return (
      <div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}
```



마지막으로 `handleClick` 함수를 Board에서 Game 컴포넌트로 이동하겠습니다. 또한 Game 컴포넌트의 state가 다르게 구성되어있기 때문에 `handleClick`을 수정해야 합니다. Game의 `handleClick` 함수에서 새로운 기록 목록을 `history`로 연결하겠습니다.

```react
  handleClick(i) {
    const history = this.state.history;
    const current = history[history.length - 1];
    const squares = this.state.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      history: history.concat([{
        squares: squares,
      }]),
      xIsNext: !this.state.xIsNext,
    });
  }
```

> 주의! 배열 `push()` 함수와 같이 더 익숙한 방식과 달리 `concat()` 함수는 기존 배열을 변경하지 않기 때문에 이를 더 권장합니다.

현재 시점에서 Board 컴포넌트는 `renderSquare`와 `render` 함수만을 필요로 합니다. 게임의 상태와 `handleClick` 함수는 Game 컴포넌트가 가지고 있어야 합니다.



### 과거의 이동 표시하기

history를 map

```react
class Game extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      history: [{
        squares: Array(9).fill(null),
      }],
      xIsNext: true,
    }
  }

  handleClick(i) {
    const history = this.state.history;
    const current = history[history.length - 1];
    const squares = this.state.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }

    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      history: history.concat([{
        squares: squares,
      }]),
      xIsNext: !this.state.xIsNext,
    });
  }

  render() {
    const history = this.state.history;
    const current = history[history.length - 1];
    const winner = calculateWinner(current.squares);

    const moves = history.map((step, move) => {
      const desc = move ?
        'Go to move #' + move :
        'Go to game start';
      return (
        <li>
          <button onClick={() => this.jumpTo(move)}>{desc}</button>
        </li>
      );
    });
    
    let status;
    if (winner) {
      status = 'Winner: ' + winner;
    } else {
      status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
    }

    return (
      <div className="game">
        <div className="game-board">
          <Board 
            squares={current.squares}
            onClick={(i) => this.handleClick(i)}
          />
        </div> 
        <div className="game-info">
          <div>{status}</div>
          <ol>{moves}</ol>
        </div>
      </div>
    );
  } 
}
```

> 주의! **배열이나 이터레이터의 자식들은 고유의 “key” prop을 가지고 있어야 합니다. “Game”의 render 함수를 확인해주세요.**



### Key 선택하기

React에서 `key`는 심화 기능인 `ref`와 동일하게 특별하고 미리 지정된 prop입니다. 엘리먼트가 생성되면 React는 `key` 속성을 추출하여 반환되는 엘리먼트에 직접 키를 저장합니다. `key`가 `props`에 속하는 것처럼 보이지만 `this.props.key`로 참조할 수 없습니다. React는 자동으로 `key`를 어떤 컴포넌트를 업데이트 할 지 판단하는 데에 사용합니다. 컴포넌트는 `key`를 조회할 수 없습니다.

**동적인 리스트를 만들 때마다 적절한 키를 할당할 것을 강력하게 추천합니다.** 적절한 키가 없는 경우 데이터 재구성을 고려해 볼 수 있습니다.



### 시간 여행 구현하기

```react
    const moves = history.map((step, move) => {
      const desc = move ?
        'Go to move #' + move :
        'Go to game start';
      return (
        <li key={move}>
          <button onClick={() => this.jumpTo(move)}>{desc}</button>
        </li>
      );
    });
```

`JumpTo` 함수가 정의되어있지 않기 때문에 리스트 아이템의 버튼을 클릭하면 에러가 발생합니다. `jumpTo`를 구현하기 전에 Game 컴포넌트의 state에 `stepNumber`를 추가하여 현재 진행 중인 단계를 표시하겠습니다.

먼저 Game의 `constructor` 초기 state로 `stepNumber: 0`을 추가해주세요.

```react
class Game extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      history: [{
        squares: Array(9).fill(null),
      }],
      stepNumber: 0,
      xIsNext: true,
    }
  }
```

다음으로 Game의 `stepNumber`를 업데이트하기 위해 `jumpTo`를 정의해주세요. 또한 `stepNumber`가 짝수일 때 마다 `xIsNext`를 true로 설정하겠습니다.

```react
  jumpTo(step) {
    this.setState({
      stepNumber: step,
      xIsNext: (step % 2) === 0,
    });
  }
```

이제 사각형을 클릭할 때 마다 실행되는 Game의 `handleClick` 함수에 몇 가지 변화를 줄 것입니다.

`stepNumber` state는 현재 사용자에게 표시되는 이동을 반영합니다. 새로운 이동을 만든 후에 `this.setState`의 인자로 `stepNumber: history.length`를 추가하여 `stepNumber`를 업데이트 해아합니다. 이를 통해 새로운 이동이 생성된 후에 이동이 그대로 남아있는 것을 방지합니다.

또한 `this.state.history`를 `this.state.history.slice(0, this.state.stepNumber + 1)`로 교체할 것입니다. 그 결과로 “시간을 거슬러 올라가고” 그 시점에서 새로운 이동을 발생한다면 이제는 맞지 않게 된 모든 “미래”의 기록을 날려버립니다.

```react
  handleClick(i) {
    const history = this.state.history.slice(0, this.state.stepNumber + 1);
    const current = history[history.length - 1];
    const squares = current.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }

    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      history: history.concat([{
        squares: squares,
      }]),
      stepNumber: history.length,
      xIsNext: !this.state.xIsNext,
    });
  }
```

마지막으로 Game 컴포넌트의 `render` 함수를 수정하여 항상 마지막 이동을 렌더링하는 대신 `stepNumber`에 맞는 현재 선택된 이동을 렌더링할 것입니다.

```react
  render() {
    const history = this.state.history;
    const current = history[this.state.stepNumber];
    const winner = calculateWinner(current.squares);
```


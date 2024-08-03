# I Don't Know React

> 저는 리액트에 대해서 잘 모릅니다.

## Introduction

본 저장소는 리액트를 효율적으로 사용하기 위해 만들어졌습니다. 단순히 '이렇게 코딩하면 좋다'는 식의 접근이 아니라, 왜 그렇게 해야 하는지에 대한 근거를 명확히 설명할 수 있어야 한다고 생각합니다. 이를 위해서는 리액트의 내부 동작 원리를 깊이 이해하는 것이 필수적입니다.

_이해한 바탕으로 기록한 내용이기에 한계가 있을 수 있습니다._  
👉 [PR 환영합니다.](https://github.com/ChoiYongWon/I-Dont-Know-React/pulls)

### 이렇게 진행할꺼에요

코드 전체를 다루진 않을겁니다. 다만, 각 함수에서 다루는 변수에 대해서 설명하고 각 함수가 실행되면서 달라지는 리액트 환경을 시각적으로 설명하는걸 목표로 합니다.

> 24.07.30 기준 React [18.3.1](https://github.com/facebook/react/tree/v18.3.1) 버전을 기준으로 진행합니다.

지속적으로 업데이트 할 예정입니다.

## 전체 틀 및 용어

(추후 추가 예정)

## 과정

### 1. Entry

```jsx
const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<App />);
```

React는 여러 호스트(앱, 브라우저)에서 사용할 수 있습니다. 그 중에서 ReactDOM은 React를 브라우저에서 그려주는 역할을 담당합니다. `createRoot` 메소드를 통해 브라우저의 Container라는 공간에 실제 DOM을 연결하고 FiberRootNode와 HostRoot를 생성하고 `render` 메소드를 통해 workInProgress에서 App 컴포넌트를 생성해서 재조정 과정(Render, Commit Phase)를 거쳐 Container에 페인팅합니다.

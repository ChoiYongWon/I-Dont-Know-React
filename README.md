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

#### createRoot

![Group 9](https://github.com/user-attachments/assets/b9f01ff9-2238-4c03-aa20-45e070997a04)


createRoot는 Fiber 전체를 관리하는 FiberRoot 객체를 생성하고 containerInfo 속성에 실제 DOM을 매핑합니다. 그리고 HostRoot(최상위 Fiber) 역할을 하는 Fiber 객체를 생성하여 current 속성에 매핑합니다. 

#### render

![Group 19](https://github.com/user-attachments/assets/fac89df7-5812-4fc8-94a0-848c3d5a1324)


current의 FiberRoot를 복사해서 workInProgress 객체를 생성하고 current랑 서로 alternate 속성으로 참조합니다. 그 HostRoot Fiber를 workInProgress 객체에 매핑하고 재조정 과정을 진행합니다.  

즉, workInProgress 객체는 하나의 Fiber입니다.  



하나의 workInProgress를 재조정할때 다음과 같은 순서로 진행됩니다.

- childFiber가 존재하면 childFiber를 workInProgress로 설정
- childFiber가 없다면 completeWork로 설정하고 매핑되는 HTML Node를 생성해서 stateNode 속성에 매핑
- siblingFiber가 존재하면 siblingFiber를 workInProgress로 설정



### 2. Initial Mount

 이때 HostRoot인 div#root 태그만 생성됨. HostRoot를 대상으로 재조정 과정 진행 (workLoop 과정). workInProgress가 참조하는 값은 하나의 FiberNode이고 child가 있으면 workInProgress가 child로 바뀌면서 dfs형식으로 진행함 (beginwork 과정). 말단 FiberNode가 처리되면 FiberNode에 stateNode 속성으로 대응되는 HTMLNode값이 매핑되고 sibling이 존재하면 workInProgress을 sibling으로 설정한다. sibling이 존재하지 않으면 해당 Fiber는 재조정이 끝나고 return 속성 값인 부모 FiberNode를 workInProgress로 지정하여 재조정 과정을 마무리한다 (completeWork 과정). 재조정 과정이 마무리 되면 workInProgress 관련 값을 null로 초기화하고 FiberRootNode의 속성인 finishedWork에 workInProgress값을 할당한다 (commitRoot 과정). finishedWork를 실제 DOM에 Mount한다 (CommitMutation 과정).

# Day1

## 리액트 기초 문법

### 태그는 항상 닫혀있어야 한다
태그는 열린 개수와 동일한 개수만큼 닫혀있어야 한다.

HTML에서는 input이나 br 태그의 경우 태그를 닫지 않아도 사용 가능하지만 리액트에서는 허용되지 않는다.

태그를 열었으면 그와 매칭되는 태그로 항상 닫아주어야 한다.

 ```
 <div>
</div>
```


단, 컴포넌트와 같이 태그와 태그 사이에 내용이 들어가지 않을 때는 Self Closing 태그를 사용할 수 있다.

이는 태그를 엶과 동시에 닫는다는 의미이다.

```
 <App />
<input />
```

### 두 개 이상의 태그는 항상 하나의 태그 안에 담겨있어야 한다

```
import React from 'react';
import Hello from './Hello';

function App() {
    return (
        <Hello />
        <div>Hello React</div>
    );
}

export default App;
```

위와 같이 코드를 작성한 경우 에러가 난다.

그 이유는 Hello 태그와 div 태그를 감싸는 부모 태그가 없이 때문이다.

아래와 같이 두 태그를 모두 감싸는 태그를 하나 만들어 주면 에러가 나지 않는다.

 
```
import React from 'react';
import Hello from './Hello';

function App() {
    return (
        <div>
            <Hello />
            <div>Hello React</div>
        </div>
    );
}

export default App; 
```

또는 아래와 같이 이름이 없는 태그인 fragment를 이용해서 간편하게 작성할 수도 있다.

(* div 태그를 사용해도 되지만, 이 경우 CSS를 적용할 때 문제가 발생할 수 있다.

빈 태그인 fragment를 사용하면 CSS를 적용할 때 편하고, 관리자도구에서도 뜨지 않는다.) 

```
import React from 'react';
import Hello from './Hello';

function App() {
    return (
        <>
            <Hello />
            <div>Hello React</div>
        </>
    );
}

export default App;
```

### 한 번에 하나의 컴포넌트만 렌더링할 수 있다
```
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(<App />, document.getElementById('root')); 
```
index.js 파일에서 렌더링 할 때 위와 같이 ```<App />``` 하나의 컴포넌트만 렌더링 가능하다.

# Day2

## 리액트 기초 문법

### props 선언 규칙
```
import React from 'react';

function Test() {
    return <h1>Hello React</h1>
}

function App() {
    return (
        <Test greeting="hello" />
    );
}

export default App;
```
HTML에서는 `<div class="hello"></div>` 와 같은 형식으로 작성하지만

JSX에서는 `<Test greeting="hello" />` 와 같이 작성한다.

이를 해석하자면, Test 컴포넌트의 greeting라는 속성을에 hello라는 value를 준 것이다.

이렇게 greeting="hello"와 같은 것을 props라고 부른다.

### render()
작성한 코드로 눈에 보이는 화면을 구성하려면 렌더링이 꼭 필요하다.

리액트에는 이 렌더링을 render() 메서드가 담당한다.

 

리액트는 모든 클래스 컴포넌트의 render() 메서드를 자동적으로 실행한다.

 
```
import React from "react";

class App extends React.Component{  
    render() {    
        return <h1>a class component</h1>;
    }
}

export default App;
```

### props 기본 예시
```
// Header.jsx (자식 컴포넌트)

function Header(props) { // 2번) 매개변수로 props를 넣고
  return (
    <header className="App-header">
      <p>
        React, { props.title } // 3번) { props.상속받은이름 } 으로 사용한다
      </p>
    </header>
  );
}

export default Header;

// App.jsx (부모 컴포넌트)
import './App.css';
import Header from './components/Header';

function App() {
  return (
    <div className="App">
      <Header title={ "React A" } /> // 1번) 자식에게 상속하면
      <Header title={ "React B" } />
      <Header title={ "React C" } />
    </div>
  );
}

export default App;
```
# Day3

## 리액트네이티브 vs 플러터

사용 언어  
RN : js 경험 있으면 쉬움  
FL : Dart - java 경험 있으면 배우기 쉽지만 쓰이는곳이 여기밖에 없음

성능  
RN : 다양한 자료가 쌓여있어 대규모 프로젝트에 적합 / 속도가 느림 but 요즘 폰 좋아서 최적화 덜돼도 잘돌아감  
FL : 소규모 프로젝트에서 빠르게 적용할 수 있음 / 속도가 빠름 / css 인라인 스타일->작업속도 빠름

전망  
RN : 아직도 쓰는사람 많고 선호도 높음  
FL : 지금은 좀 부족하지만 전망 좋음

편의성  
RN : 외부 라이브러리가 많음  
FL : 기본 제공 툴(컴포넌트)이 많음

개발 방식  
RN : 함수형  
FL : OOP

# Day4

## 리액트네이티브 vs 플러터 예시 코드

리액트 네이티브 css 적용 - 내부 스타일

플러터 - 인라인 스타일


리액트 네이티브 - 함수형 컴포넌트 or 클래스형 컴포넌트

함수형

```jsx
import React, {useState} from 'react';
import Styled from 'styled-components/native';
import Button from '~/Components/Button';

const Container = Styled.SafeAreaView`
    flex: 1;
`;

const TitleContainer = Styled.View`
    flex: 1;
    justify-content: center;
    align-items: center;
`;

const TitleLabel = Styled.Text`
    font-size: 24px;
`;

const CounterContainer = Styled.View`
    flex: 2;
    justify-content: center;
    align-items: center;
`;

const CountLabel = Styled.Text`
    font-size: 24px;
    font-weight: bold;
`;

const ButtonContainer = Styled.View`
    flex: 1;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: space-around;
`;

interface Props {
  title?: string;
  initValue: number;
}

const index = ({title, initValue}: Props) => {
  const [count, setCount] = useState<number>(initValue);

  return (
    <Container>
      {title && (
        <TitleContainer>
          <TitleLabel>{title}</TitleLabel>
        </TitleContainer>
      )}
      <CounterContainer>
        <CountLabel>{initValue + count}</CountLabel>
      </CounterContainer>
      <ButtonContainer>
        <Button iconName="plus" onPress={() => setCount(count + 1)} />
        <Button
          iconName="minus"
          onPress={() => setCount(count > 0 ? count - 1 : count)}
        />
      </ButtonContainer>
    </Container>
  );
};

export default index;
```

클래스형

```jsx
...

interface Props {
  title?: string;
  initValue: number;
}

/**
 * 클래스 컴포넌트에서 State를 사용하는 경우, 함수형 컴포넌트와는 다르게
 * State의 타입을 미리 정의하고 컴포넌트 선언 시 해당 타입을 지정한다.
 */
interface State {
  count: number;
  error: Boolean;
}

class Counter extends React.Component<Props, State> {
  constructor(props: Props) {
    /**
     * 생성자 함수에서는 항상 super(props);를 사용하여
     * 부모 컴포넌트(React.Component)의 생성자 함수를 호출해야 한다는 점이다.
     */
    super(props);
    console.log('constructor');

    /**
     * 함수형 컴포넌트에서는 useState로 State를 생성할 때, 초기값을 지정했으나
     * 클래스 컴포넌트에서는 생성자에서 State의 초기값을 지정한다.
     */
    this.state = {
      count: props.initValue,
      error: false,
    };
  }

  render() {
    /**
     * 클래스 컴포넌트에서는 함수형 컴포넌트와는 다르게
     * Props와 State에 접근하기 위해서 this를 함께 사용한다.
     */
    const {title} = this.props;
    const {count, error} = this.state;

    return (
      <Container>
        {!error && (
          <>
            {title && (
              <TitleContainer>
                <TitleLabel>{title}</TitleLabel>
              </TitleContainer>
            )}
            <CounterContainer>
              <CountLabel>{count}</CountLabel>
            </CounterContainer>
            <ButtonContainer>
              <Button
                iconName="plus"
                /**
                 * State는 불변값이므로 변경하고자 할 때는
                 * this.setState 함수를 사용하여 State 값을 변경한다.
                 */
                onPress={() => this.setState({count: count + 1})}
              />
              <Button
                iconName="minus"
                onPress={() =>
                  this.setState({count: count > 0 ? count - 1 : count})
                }
              />
            </ButtonContainer>
          </>
        )}
      </Container>
    );
  }

  /**
   * 클래스 컴포넌트는 함수형 컴포넌트와 다르게 라이프 사이클 함수들을 가지고 있다.
   */
  static getDerivedStateFromProps(nextProps: Props, prevState: State) {
    console.log('getDerivedStateFromProps');
    return null;
  }

  componentDidMount() {
    console.log('componentDidMount');
  }

  getSnapshotBeforeUpdate(prevProps: Props, prevState: State) {
    console.log('getSnapshotBeforeUpdate');
    return {
      testData: true,
    };
  }

  componentDidUpdate(prevProps: Props, prevState: State, snapshot: ISnapshot) {
    console.log('componentDidUpdate');
  }

  shouldComponentUpdate(nextProps: Props, nextState: State) {
    console.log('shouldComponentUpdate');
    return true;
  }

  componentWillUnmount() {
    console.log('componentWillUnmount');
  }

  componentDidCatch(error: Error, info: React.ErrorInfo) {
    this.state({
      error: true,
    });
  }
}

export default Counter;
```

플러터 - 자바같은 객체지향 프로그래밍

```
class Idol {
  String name;
  int membersCount;
  Idol({                          // contructor 생성
    required this.name,
    required this.membersCount,
  });
  void sayName() {
    print('저는 ${this.name}');
  }
  void sayMembersCount() {
    print("${this.name} 은 ${this.membersCount} 명의 멤버가 있습니다.");
  }
}
class BoyGroup extends Idol {
  BoyGroup(
    String name,
    int membersCount,
  ) : super(                         // super를 이용해 부모 컨스트럭터 상속
          name: name,
          membersCount: membersCount,
        );
}
```

```
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: WelcomePage(),
      debugShowCheckedModeBanner: false,  //오른쪽 상단에 배너 띠를 없애줌니다.
    );
  }
}

class WelcomePage extends StatelessWidget {
  const WelcomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(child:Text('Hello'),),
    );
  }
}
```

# Day 5
## 기술 스택 공부

## Recoil
React를 위한 상태 관리 라이브러리

사용 이유
1. 단순한 사용방법
2. React를 만든 Facebook에서 직접 만든 것이라 호환성이 다른 상태관리 라이브러리보다 좋다 

### Atom 설정
Atom은 상태(state)의 일부를 나타낸다. Atoms는 어떤 컴포넌트에서나 읽고 쓸 수 있다. atom의 값을 읽는 컴포넌트들은 암묵적으로 atom을 구독한다. 그래서 atom에 어떤 변화가 있으면 그 atom을 구독하는 모든 컴포넌트들이 재 렌더링 되는 결과가 발생할 것이다. (Recoil 본문 발췌)
```
import { atom } from 'recoil';

export interface IContentTypes {
    name: string;
    status: boolean;
    message: string;
}

//recoil state 생성
export const contentState = atom<IContentTypes>({
    key: 'content',
    default: {
        name: 'test',
        status: false,
        message: ''
    }
});
```
이런식으로 별도 state.ts 파일을 생성하여 관리할 상태를 atom을 사용하여 설정작업을 진행해준다.

(typescript 환경이 아닌경우 타입 내용 빼고 사용)

 

 

위 작업들로 이제 recoil 상태관리를 사용할 준비는 벌써 끝났다. redux와 비교하면 너무도 단순하다.

 

redux : 스토어 생성 > 리듀서 생성 > 액션 설정 > 디스패치(액션) 호출 

recoil : 스토어 생성 > 호출 

 

사용방식에 따라 단계는 좀 바뀔 수 있지만 간단하게는 저런 차이다.

 

### 사용하기
```
import { useState, useEffect } from 'react';

//state, recoil import
import { contentState } from './state';
import { useSetRecoilState, useRecolValue } from 'recoil';

export default function sample() {
	
    const [reqContent, setReqContent] = useState({
        name: "sample",
        status: true,
        message: "테스트 메세지"
    });
    
    //recoil 사용 선언부
    const setContent = useSetRecoilState(contentState);
    const content = useRecolValue(contentState);
  
    useEffect(()=>{
        //recoil setting 
        setContent(reqContent);
    },[])
    
    return (
    	{content && (
        	<div>
            	{content.name},
                {content.status},
                {content.message}
            </div
        )}
    );
}
```
1. 사용할 recoil state를 import

2. 사용할 recoil 기능 import

    ( useRecoilState : react의 useState랑 동일한 기능이라고 생각하면 된다.

      useSetRecoilState : useState에서 setter만 있는것

      useRecolValue : useState에서 value만 있는것

      useResetRecoilState : 기본값으로 초기화 시키는 기능)

3. 위 기능을 사용하여 기능 구현

 

## Tailwind css

Tailwind CSS는 Utility-First 컨셉을 가진 CSS 프레임워크다. 부트스트랩과 비슷하게 m-1, flex와 같이 미리 세팅된 유틸리티 클래스를 활용하는 방식으로 HTML 코드 내에서 스타일링을 할 수 있다

### 예시
```
<div class="p-6 max-w-sm mx-auto bg-white rounded-xl shadow-lg flex items-center space-x-4">
  <div class="shrink-0">
    <img class="h-12 w-12" src="/img/logo.svg" alt="ChitChat Logo">
  </div>
  <div>
    <div class="text-xl font-medium text-black">ChitChat</div>
    <p class="text-slate-500">You have a new message!</p>
  </div>
</div>
```

자주 사용하는 css 등을 간단한 인라인 스타일로 묶어서 표현할 수 있다.
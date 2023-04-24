# 리액트의 생명주기
> 컴포넌트가 브라우저에 렌더링되기 전 <span style="color:#ffbe17">(생성)</span> 부터 사라질때 <span style="color:#ffbe17">(소멸)</span> 까지의 과정을 말한다.

생명주기 메서드를 사용해서 컴포넌트가 생성되고, 업데이트되고, 제거될 때 일어나는 작업을 수행할 수 있다.

# LifeCycle Method (생명주기 메서드) 
리액트의 생명주기는 크게 세 단계로 나눌 수 있다. <br />
👉 마운트, 업데이트, 언마운트

## 1. **마운트** : 컴포넌트가 생성되어 DOM에 추가되는 단계
<br />

- <mark>constructor()</mark> : 컴포넌트의 생성자 메서드. 초기 state를 설정. <br />
  
  ✨ 함수형 컴포넌트에서는 useState 훅을 사용해서 state를 설정 및 관리.
- <mark>static getDerivedStateFromProps(props,state)</mark> : props를 기반으로 상태를 업데이트하는 객체를 반환한다. (또는 null 반환) <br />
마운트될 때와 업데이트될 때 호출되는 함수이다. 즉, 렌더링될 때마다 매번 실행되는 함수이기 때문에 주의해서 사용하는 것이 좋다. 이 함수를 사용해서 state를 바꾸는것을 권장하지 않는다. <br />

  ✨ 함수형 컴포넌트에서는 useState 훅을 사용해서 state를 설정 및 관리.

- <mark>render()</mark> : 컴포넌트의 UI를 렌더링한다.

  ✨ 함수형 컴포넌트에서는 return 문을 사용해서 JSX로 작성된 UI를 반환하여 DOM에 렌더링한다.

- <mark>componentDidMount()</mark> : 컴포넌트가 처음 렌더링된 후 실행된다. 즉, 컴포넌트가 DOM에 추가된 후 실행된다. 비동기 요청을 통해 데이터를 불러오는 작업을 수행할 수 있다.

  ✨ 함수형 컴포넌트에서는 useEffect 훅을 사용해서 렌더링 후 실행될 함수를 정의할 수 있다. useEffect 훅의 두번째 인자에 비어있는 의존성 배열을 추가해주면 컴포넌트가 처음 렌더링될 때 실행할 동작을 정의할 수 있다. <br />
  ex. 
  ```javascript
  useEffect(()=>{ 
    console.log('컴포넌트가 마운트되었습니다.') 
    },[]);
  ```
<br />

## 2. **업데이트** : 상태나 속성이 변경되어 컴포넌트가 업데이트되는 단계
<br />

- <mark>static getDerivedStateFromProps()</mark> : 설명 생략 (위에서 설명했음)
- <mark>shouldComponentUpdate()</mark> : 컴포넌트가 업데이트되어야 하는지 결정한다. (props 또는 state 값이 변경되었을때, 리렌더링할지 결정한다.) 성능 최적화를 위해 사용한다.

  ✨ 함수형 컴포넌트에서는 useMemo 훅을 사용할 수 있다. <br />
- <mark>render()</mark> : 설명 생략 (위에서 설명했음)
- <mark>getSnapshotBeforeUpdate()</mark> : 변경된 컴포넌트를 DOM에 반영하기 전에 호출되는 메서드이다. 업데이트하기 직전의 값을 참고할 때 사용된다. (ex. 스크롤바 위치)
- <mark>componentDidUpdate()</mark> : 컴포넌트가 갱신된 직후 (리렌더링 후) 호출되는 함수이다. 최초 렌더링에서는 호출되지 않는다.

  ✨ 함수형 컴포넌트에서는 useEffect 훅을 사용해서 렌더링 후 실행될 함수를 정의할 수 있다. useEffect 훅의 두번째 인자 배열에 state를 넘겨주면, 해당 상태에 변화가 일어날때마다 함수가 호출된다.  <br />

<br />

## 3. **마운트 해제** : 컴포넌트가 DOM에서 제거될 때 호출된다.
<br />

- <mark>componentWillUnmount()</mark> : 컴포넌트가 마운트 해제되어 제거되기 전에 호출되는 메서드이다. componentDidMount에서 등록한 이벤트, 타이머 등이 있다면 여기서 제거 작업을 해야 한다.

  ✨ 함수형 컴포넌트에서는 useEffect 훅의 clean-up 함수를 사용해서 마운트 해제 시 실행될 함수를 정의할 수 있다.   <br />
  ex. 
  ```javascript
  useEffect(()=>{ 
    
    return ()=>{ //clean-up 함수
      console.log('컴포넌트가 언마운트됩니다..') 
    }
    },[]);
  ```
<br />

## 에러처리 메서드
<br />
컴포넌트 내부에서 발생하는 에러를 처리하는 메서드이다. 

<br />


- <mark>static getDerivedStateFromError()</mark> 
- <mark>componentDidCatch(error,info)</mark> 

  -  error : 발생한 오류
  -  info : 어떤 컴포넌트에서 오류가 발생했는지에 대한 정보룰 포함한 객체


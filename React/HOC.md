## HOC (High Order Component)

HOC(고차 컴포넌트)는 컴포넌트 로직을 재사용하기 위한 기술이다. 고차 컴포넌트는  파라미터로 컴포넌트를 받아서 새 컴포넌트를 반환하는 <mark>함수</mark>이다.

리액트 컴포넌트를 작성하다보면 반복되는 로직이 발생한다. 이때, HOC를 정의하여 해결할 수 있다.

예시를 통해 빠르게 이해해보자.

<span style="color:#3ac8b4; font-size:17px; font-weight:bold">예시 )</span>
```javascript
<Routes>
  <Route exact path="/" element={Home} />
  <Route path="/about" element={About} />
  <Route path="/my-page" element={MyPage} />
  <Route path="/todo-list" element={TodoList} />
</Routes>
```
위와 같이 라우터 컴포넌트들이 있다. 각 라우터 element마다 컴포넌트에 lazy와 Suspense 그리고 ErrorBoundary를 적용해주려고 한다. 우선 고차 컴포넌트를 사용하지 않고 구현해보자.


❌ <span style="font-size:17px; font-weight:bold">HOC</span>
```javascript
const Home = React.lazy(() => import('./page/Home'));
const About = React.lazy(() => import('./page/About'));
const MyPage = React.lazy(() => import('./page/MyPage'));
const TodoList = React.lazy(() => import('./page/TodoList'));

<Routes>
  <Route exact path="/" element={
    <Suspense>
    Boundary>Home} />
  <Route path="/about" element={
    <Suspense>
      <ErrorBoundary>
        About
      <ErrorBoundary/>
    </Suspense>
    } 
  />
  <Route path="/my-page" element={
    <Suspense>
      <ErrorBoundary>
        MyPage
      <ErrorBoundary/>
    </Suspense>
    } 
/>
  <Route path="/todo-list" element={
    <Suspense>
      <ErrorBoundary>
        TodoList
      <ErrorBoundary/>
    </Suspense>
    } 
/>
</Routes>
```
🤯

물론 이렇게 안하고 그냥 한번에 라우터 바깥에서 Suspense와 ErrorBoundary를 감싸줘서 전역적으로 처리할 수도 있다. <br />
하지만 페이지별로 에러를 처리하고 싶다면 위와 같이 해당 페이지 컴포넌트를 감싸줘야하는데 보시다시피 똑같은 코드가 반복적으로 발생하게된다.

HOC를 사용해서 해결해보자! 

✅ <sp7 style="font-size:17px; font-weight:bold">HOC</sp7>

```javascript

// HOC 생성
const LazyComponent = (importFunc) => {
  const Component = lazy(importFunc);
  return (props) => (
    <ErrorBoundary>
      <Suspense fallback={<div>Loading...</div>}>
        <Component {...props} />
      </Suspense>
    </ErrorBoundary>
  );
};


const Home = LazyComponent(() => import('./page/Home'));
const About = LazyComponent(() => import('./page/About'));
const MyPage = LazyComponent(() => import('./page/MyPage'));
const TodoList = LazyComponent(() => import('./page/TodoList'));

<Routes>
  <Route exact path="/" element={Home} />
  <Route path="/about" element={About} />
  <Route path="/my-page" element={MyPage} />
  <Route path="/todo-list" element={TodoList} />
</Routes>
```

import 함수를 받아서 ErrorBoundary와 Suspense로 감싸진 새로운 컴포넌트를 반환하는 Lazy 컴포넌트를 정의했다.
이전 코드의 중복된 로직을 줄여 코드가 간결해졌고 가독성도 향상됐다.

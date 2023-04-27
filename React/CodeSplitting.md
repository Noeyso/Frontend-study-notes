## 코드 스플리팅(Code Splitting)

일반적으로 웹 애플리케이션을 제작할 때 모든 코드를 하나의 코드로 묶어서 배포한다. (✨번들링) <br/>
프로그램이 커짐에 따라 번들 파일의 크기도 커지게 되는데 이로인해 초기 로딩 속도가 느려질 수 있다. 이를 개선하는 방법 중 하나가 바로 **코드 스플리팅**이다. 이름 그대로 코드를 쪼개는 것이다.👊 필요한 파일만 쪼개서 불러오기 때문에 초기 로딩시 모든 파일을 불러올 필요가 없어서 초기 로딩 속도를 개선할 수 있다.



## React에서 코드 스플리팅하기
### lazy , Suspense
리액트에서 코드스플리팅하는 대표적인 방법은 lazy와 Suspense를 사용하는 것이다.
<br />
<br />


우선, lazy에 대해 알아보자.

### ⚙️ lazy
lazy를 사용하면 동적으로 컴포넌트를 로드할 수 있다.

```typescript
const MyPage = lazy(() => import('./MyPage'));
```
위와 같은 형태로 사용할 수 있다.
<br />
<br />

### ⚙️ Suspense
import 함수는 Promise를 반환한다. 즉, lazy가 비동기적으로 페이지를 받아온다는 것이다. 

Suspense를 사용하면 컴포넌트가 로드될때까지 대기하며 그 과정에서 fallback에 지정해준 Loading 컴포넌트를 보여준다.

```javascript
<Suspense fallback={<div>Loading...</div>}>
  <MyPage />
</Suspense>
```

## React Router와 코드 스플리팅
코드스플리팅을 하는 방법 중 하나는 라우터를 기반으로 하는 것이다. 라우터와 함께 사용하면 특정 URL 경로에 해당하는 컴포넌트를 동적으로 로드하도록 하여 초기 로딩 시간을 줄이고, 라우터 별로 컴포넌트를 분리할 수 있다.

🤖 아래는 ChatGPT가 만들어준 예시이다 🤖

```javascript
const Home = lazy(() => import('./components/Home'));
const About = lazy(() => import('./components/About'));
const Contact = lazy(() => import('./components/Contact'));
```

```javascript
<Router>
  <Suspense fallback={<div>Loading...</div>}>
    <Switch>
      <Route exact path="/" component={Home} />
      <Route path="/about" component={About} />
      <Route path="/contact" component={Contact} />
    </Switch>
  </Suspense>
</Router>
```
이런식으로 컴포넌트들을 lazy 로딩하여 라우터와 함께 사용할 수 있다. ✨
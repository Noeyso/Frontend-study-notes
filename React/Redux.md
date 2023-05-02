# Redux

>JavaScript 앱의 상태 관리 라이브러리

순수 js 라이브러리이기 때문에 React 뿐만 아니라 Vue.js, jQuery 등 다른 라이브러리에서도 사용가능하다. 하지만 일반적으로 React에서 사용되어지고 있다.

## 💥 Redux를 사용하는 이유 : props drilling 막기
<img width="734" alt="image" src="https://user-images.githubusercontent.com/48446896/235561444-468c4d7b-7767-4caa-bd1e-29b8ee635958.png">

리액트로 앱을 만들다보면 여러 컴포넌트가 계층 구조를 이루게 된다. 이때 부모 컴포넌트의 상태를 자식 컴포넌트가 사용해야하는 경우 위 그림과 같이 props가 자식 컴포넌트로 계속해서 전달된다.

이렇게되면 상태가 많아질 수록 자식 컴포넌트가 받는 props도 많아지고 전달하는 props가 많아서 상태관리가 어려워진다. 문제가 발생했을때 추적하기도 쉽지 않을 것이다.

이러한 문제를 해결하기 위해 상태 관리 라이브러리가 나왔다. 대표적으로 사용되는 것이 바로 Redux이다.

<img width="883" alt="스크린샷 2023-05-02 오전 10 47 08" src="https://user-images.githubusercontent.com/48446896/235562159-ccec2844-5870-4a53-bbbc-bf094015a496.png">

리덕스를 사용하면 상태를 한 곳에서 관리하기 때문에 props로 넘겨줄 필요 없이 상태를 가져올 수 있다.

<br />


## ⚡️ Redux 기본 원칙
![redux](https://user-images.githubusercontent.com/48446896/235560336-fdff1682-a685-4126-aba9-dd72739b81e2.png)
이미지 출처 : https://hanamon.kr/%ec%83%81%ed%83%9c%ea%b4%80%eb%a6%ac%eb%8f%84%ea%b5%ac-%ed%95%84%ec%9a%94%ec%84%b1/

### 🎯 동일한 데이터는 <mark>항상 같은 곳</mark>(Store)에서 가지고 온다.

**(Store는 단 한 개)**

### 🎯 <mark>액션</mark>(Action) 객체를 통해서만 상태를 변경할 수 있다.

### 🎯 변경은 <mark>순수 함수</mark>(Reducer)로만 가능하다.

<br />

## 👊 Redux 중요 개념들

### **액션 (Action)**
액션은 상태 변화에 필요한 정보 객체이다. 기본적으로 type 값을 가지고 있다.
type값은 액션의 이름이라고 생각하면 된다. <br />
예를들어, TODO를 추가하는 액션이 있다고 하면 
```javascript
{
  type: 'ADD_TODO'
}
```
ADD_TODO라는 액션 이름을 type 속성에 설정해줄 수 있다. <br />
액션은 type 뿐만 아니라 다른 값들도 넘겨줄 수 있다. 일반적으로 payload 속성에 상태 변경과 관련된 정보를 담는다.<br />
```javascript
{
  type: 'ADD_TODO',
  payload:{
    title:'블로그 포스팅 1회',
    done: false
  }
}
```

이런식으로 상태 변화시 참고할 값들을 넣어줄 수 있다. (함수의 매개변수 같은거라고 생각하면 된다.)

<br />

### **리듀서(Reducer)**
이전 상태와 액션을 받아서 새로운 상태를 반환하는 순수함수이다.

<img width="1016" alt="image" src="https://user-images.githubusercontent.com/48446896/235565691-de58b40f-e0b7-478a-a131-7558479dd696.png">
리듀서는 위 그림과 같이 동작한다.<br />
-  액션의 type 정보를 받아서 ADD_TODO에 관한 로직을 수행한다. <br />
-  payload에 넘겨준 값을 이용해서 기존의 상태 값에 변화를 준다.<br />
-  변한 상태값을 return하여 그 값이 상태값으로 업데이트된다.<br />

<br />

### **스토어(Store)**

스토어는 상태(state)를 저장하고 관리하는 객체이다. 스토어 객체는 **애플리케이션에서 유일**해야 하며, action이 발생하면 reducer 호출하여 새로운 상태를 계산하고, 그 상태를 저장한다.


<br />
여기까지가 리덕스 구조와 관련된 기본적인 개념이다.

<br />

### **디스패치(Dispatch)**
디스패치는 action을 store에 보내는 함수이다. 디스패치 함수를 호출하면 리듀서가 실행되어 상태가 변경된다. 디스패치 함수는 **action을 인자로 받는다.** 

### **셀렉터(Selector)**
셀렉터는 상태의 일부를 반환하는 함수이다. 셀렉터를 사용하면 상태의 일부를 쉽게 가져올 수 있다. 
# react typescript 기반 프로젝트 설정

#### create-react-app으로 프로젝트 만들기

`npx create-react-app app-name --template typescript`

### Redux, Redux-Thunk 세팅

`npm i axios redux react-redux @types/react-redux @types/redux redux-devtools-extension redux-thunk @types/redux-thunk`

`/src/store.ts` 스토어 생성

```ts
import { applyMiddleware, createStore } from "redux"
import RootReducer from './reducers/RootReducer'
import { composeWithDevTools} from 'redux-devtools-extension'
import thunk from 'redux-thunk'

const Store = createStore(RootReducer, composeWithDevTools(applyMiddleware(thunk)));

export type RootStore = ReturnType<typeof RootReducer>

export default Store;
```

`/src/reducers` 여기에 root reducer를 포함한 reducer들 생성

root reducer 형태 (개별 reducer를 combine해서 export 한다.)

```ts
import { combineReducers } from 'redux';
import customReducer from './CustomReducer';
// ...

const RootReducer = combineReducers({
  custom: customReducer,
  // ...
})

export default RootReducer
```

개별 reducer 형태

```ts
// action 정의한거 import
import { CustomActionDispatchTypes, CustomType, CUSTOM_ACTION_1, CUSTOM_ACTION_2, CUSTOM_ACTION_3 } from '../actions/CustomActionTypes';

interface 기본상태 {
  키1: 타입1,
  키2?: 타입2
}

const defaultState: 기본상태 = {
  키1: 값1,
  // 키2는 ? 이기 때문에 없어도 됨
}

const customReducer = (state: 기본상태 = defaultState, action: CustomActionDispatchTypes): 기본상태 => {
  switch(action.type) {
    case CUSTOM_ACTION_1:
      return {
        키1: 값1
      }
    case CUSTOM_ACTION_2:
      return {
        키1: 값1,
      }
    case CUSTOM_ACTION_3:
      return {
        키1: 값1,
        키2: 값2 // action.payload
      }
    default:
      return state;
  }
}

export default customReducer
```

`/src/actions` 여기에 action과 action type들 저장

actionTypes 형태

```ts
export const CUSTOM_ACTION_1 = 'CUSTOM_ACTION_1';
export const CUSTOM_ACTION_2 = 'CUSTOM_ACTION_2';
export const CUSTOM_ACTION_3 = 'CUSTOM_ACTION_3';

export interface CustomAction1 {
  type: typeof CUSTOM_ACTION_1
}

// payload를 안가져도 되고
export interface CustomAction2 {
  type: typeof CUSTOM_ACTION_2
}

// 특정 payload를 가져도 된다.
export interface CustomAction3 {
  type: typeof CUSTOM_ACTION_3,
  payload: {
    // 키: 타입...
    // 키: 커스텀타입...
  }
}

export type 커스텀타입 = {
  키: 타입
}

// payload 형태랑 캍은 타입을 하나 만들어준다.
export type payload타입 = {
  // 키: 타입...
  // 키: 커스텀타입...
}

export type CustomActionDispatchTypes = CustomAction1 | CustomAction2 | CustomAction3

```

action 형태

```ts
// action type에 정의한 것들을 import 한다.
import { CustomActionDispatchTypes, CUSTOM_ACTION_1, CUSTOM_ACTION_2, CUSTOM_ACTION_3 } from './CustomActionTypes'
import { Dispatch } from 'redux'

// 그냥 함수
export const 함수이름 = (파라미터: 타입) => (dispatch: Dispatch<CustomActionDispatchTypes>) => {
  // 로직 구현 dispatch({}) 로 리듀서의 해당 액션을 활성화 시킨다.
  dispatch({
    type: CUSTOM_ACTION_1
  })

  dispatch({
    type: CUSTOM_ACTION_3,
    payload: {
      키: 타입,
      키: 커스텀타입
    }
  })
}

// 비동기 함수 예시
export const 함수이름 = (파라미터: 타입) => async (dispatch: Dispatch<CustomActionDispatchTypes>) => {
  try {

    const res = await axios.get('http://~~")

  } catch(e) {

  }
}

```
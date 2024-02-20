# useState

- 리액트 훅이란?

  > useState, useEffect 등 'use'접두사를 사용하는 일련의 함수

  - 컴포넌트 데이터 관리
    - useMemo
    - useCallback
    - useState
    - useReducer
  - 컴포넌트 생명 주기 대응
    - useEffect
    - useLayoutEffect
  - 컴포넌트 메서드 호출
    - useRef
    - useImperativeHandle
  - 컴포넌트 간의 정보 공유
    - useContext

---

#### UseState

- 기본 형태

  ```react
  import {useState} from 'react'
  
  const [값, 값을 변경하는 세터 함수] = useState(초기값)
  
  ```



- 예시 코드

  ```react
  import React, { useState } from 'react';
  
  function Example() {
      // 초기값을 0으로 설정
      const [count, setCount] = useState(0);
      
      return(
          <div>
            <button onClick={() => setCount(count + 1)}>
              Click me
            </button>
          </div>
      );
      
  }
  ```



- Tip. 대괄호가 의미하는 것

  "구조분해 할당"임

  ```react
  const [count, setCount] = useState(0);
  ```

  에서 count, setCount 총 2개의 값을 만들고, useState를 이용하면 두 값을 반환하는 것.

  

---

#### useEffect

- 예시코드

  ```react
  import React, { useState, useEffect } from 'react';
  
  function Example() {
    const [count, setCount] = useState(0);
  
    useEffect(() => {
      document.title = `You clicked ${count} times`;
    });
  
    return (
      <div>
        <p>You clicked {count} times</p>
        <button onClick={() => setCount(count + 1)}>
          Click me
        </button>
      </div>
    );
  }
  ```

  useEffect Hook은 react에게 컴포넌트가 '렌더링 이후' 어떤 일을 수행해야 하는 지를 말한다.

  

- clean-up을 이용하는 Effect

  메모리 누수가 발생하지 않도록 정리.

  ```react
  import React, { useState, useEffect } from 'react';
  
  function FriendStatus(props) {
    const [isOnline, setIsOnline] = useState(null);
  
    useEffect(() => {
      function handleStatusChange(status) {
        setIsOnline(status.isOnline);
      }
      ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
      // effect 이후에 어떻게 정리(clean-up)할 것인지 표시합니다.
      return function cleanup() {
        ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
      };
    });
  
    if (isOnline === null) {
      return 'Loading...';
    }
    return isOnline ? 'Online' : 'Offline';
  }
  ```

  

- 요약

  useEffect가 컴포넌트의 렌더링 이후에 다양한 side effects를 표현할 수 있다.

  effect에 정리가 필요한 경우에는 함수를 반환한다.

  ```react
    useEffect(() => {
      function handleStatusChange(status) {
        setIsOnline(status.isOnline);
      }
  
      ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
      return () => {
        ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
      };
    });
  ```

  

- tip. 관심사를 구분하려 한다면 Multiple Effect를 사용한다.

  즉, effect여러번 사용 가



- tip. effect를 건너뛰어 성능 최적화하기

  ```react
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  }, [count]); // count가 바뀔 때만 effect를 재실행합니다.
  ```

  
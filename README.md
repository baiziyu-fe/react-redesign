# use-hooks

---

## 1.useInterval

interval（定时触发的hooks）

```TypeScript
import React, { ReactEventHandler, useState } from 'react'
import './App.css'
import hooks from '../hooks'

function App() {
  const [count, setCount] = useState(0);
  const [delay, setDelay] = useState(1000);
  const [isRunning, setIsRunning] = useState(true);

  hooks.useInterval(() => {
    setCount(count + 1);
  }, isRunning ? delay : null);

  return (
    <div
      style={{
        padding: 14
      }}
    >
      <div>{count}</div>
      <div>
        <input type="text" value={delay} onChange={(e: React.ChangeEvent) => setDelay(e.target.value || 1000)} />
      </div>
      <div>
        <button onClick={() => setIsRunning(!isRunning)}>暂停</button>
      </div>
    </div>
  );
}

export default App;


```

## 2.useMergeState

处理对象类型的state


```TypeScript
import React, { ReactEventHandler, useState } from 'react'
import './App.css'
import hooks from '../hooks'

function App() {
  const [state, setState] = hooks.useMergeState({
    hello: '',
    count: 0,
  });

  return (
    <div>
      <pre>{JSON.stringify(state, null, 2)}</pre>
      <p>
        <button type="button" onClick={() => setState({ hello: 'world' })}>
          set hello
        </button>
        <button type="button" onClick={() => setState({ foo: 'bar' })} style={{ margin: '0 8px' }}>
          set foo
        </button>
        <button type="button" onClick={() => setState((prev) => ({ count: prev.count + 1 }))}>
          count + 1
        </button>
      </p>
    </div>
  );
}

export default App;

```

## 3.useReservedFunc

保留函数引用的hook

```TypeScript
import React, { useState, useCallback, useRef } from 'react';
import { message } from 'antd';
import hooks from '../hooks'

export default () => {
  const [count, setCount] = useState(0);

  const showCountPersistFn = hooks.useReservedFunc(() => {
    message.info(`Current count is ${count}`);
  });

  const showCountCommon = useCallback(() => {
    message.info(`Current count is ${count}`);
  }, [count]);

  return (
    <>
      <button
        type="button"
        onClick={() => {
          setCount((c) => c + 1);
        }}
      >
        Add Count
      </button>
      <p>You can click the button to see the number of sub-component renderings</p>

      <div style={{ marginTop: 32 }}>
        <h4>Component with persist function:</h4>
        {/* use persist function, ExpensiveTree component will only render once */}
        <ExpensiveTree showCount={showCountPersistFn} />
      </div>
      <div style={{ marginTop: 32 }}>
        <h4>Component without persist function:</h4>
        {/* without persist function, ExpensiveTree component will re-render on state change */}
        <ExpensiveTree showCount={showCountCommon} />
      </div>
    </>
  );
};
```
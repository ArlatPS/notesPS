# React Intermediate

## Hooks
- useRef()
    - it can store a refernce to DOM element
    - it guarantees that the same element is referenced during any re-render cycles
    - combining useRef with memo method will eliminate re-rendering at all and increase performance for expensive elements
    ```
    const UseRefMemo = memo(function UseRef() {
        const renderTarget = useRef();
        ...
        <div ref={renderTarget}></div>
    }
    ```
- useReducer()
    - instead of having many functions to update properties, one reducer that handles that
    - reducer() that takes state and action and has switch cases for those actions
    - const [{objValues}, dispatch] = useReducer(reducer, {initValues})
    - then dipsatch({type: "action"})
 - useMemo()
    - return a memoized **value**
    - const value = useMemo(callback, [])
    - performance optimization
    - it memoizes expensive function calls so they are only re-evaluated when needed, not on every re-render
    ```
    const value = useMemo(() => expensiveMathOperation(count), [count]);
    ```
- useCallback(callback, [])
    - returns a memoized **callback**
    ```
    const memoizedCallback = useCallback(
        () => {
            doSomething(a, b);
        },
        [a, b],
    );
    ```
- useLayoutEffect(callback)
    - similar to useEffect() but it is synchronous to render and useEffect() is sheduled after that
    - it runs at the same time as componentDidMount and componentDidUpdate wheareas euseEffect is sheduled after
    - only needed to measure DOM nodes for animation
- useId()
    - for consistent ids
    ```
    const id = useId();
    <label htmlFor={id}>
    <input id={id}>
    ```
- useImperativeHandle()
- useDebugValue()
- useDefferedValue() and useTransition() - low priority updates

## Code Splitting
- lazy(callback with import)
- Suspense with fallback
```
import { useState, lazy, Suspense } from "react";

const Details = lazy(() => import("./Details"));

<Suspense
  fallback={
    <div className="loading-pane">
      <h2 className="loader">🌀</h2>
    </div>
  }
>
  […]
</Suspense>;
```


## to arrange
- setState(x) is really setState(n => x)
- it can be used to really queue operations on state
  ```
  setVar(n => n + 1)
  setVar(n => n + 1)
  setVar(n => n + 1)
  ```
- another good use `setEnabled(e => !e)`

- updating arrays in state
  - adding - [...arr, newVal] - instead of push or unshift
  - removing - filter or slice - instead of pop, shift or splice
  - replacing - map - instead of splice or assignment
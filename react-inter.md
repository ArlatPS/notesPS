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
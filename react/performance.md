# React Performance

- core concepts
  - not doing something is always faster - component hierarchy and state managment
  - skipping is also less work - memoization
  - putting of doing stuff - suspense
- use both React Profiler and Performance dev tools to spot issues
- issue with useState with a function

  ```
  useState(generateRandomColor()) //  calls that function on every render

  useState(() => generateRandomColor()) //only once
  ```

## Preventing rerenders

- pushing state down to not trigger unnecessary rerenders
- higher order components - add functionality to existing components
- **memo()**
  - `import { memo } from "react"`
  - `export default memo(Component)`
  - on rerender it checks if props for this component are different and if not it won't rerender
  - but if a prop is a function, object, array which is redeclared on rerenders then it's not the same
- another option is to take out a component that doesnt need to rerender from ParentComponent and pass it in children

  ```
  <Game>
  <ExpensiveComponent />
  </Game>

  const Game  = ({children}) => ...

  {children}
  ```

  - because of that it won't be the child (confusing names)
  - Component and its rerender belong to the file it was called in/defined in

## Memoization

- **useMemo()** - if it is expensive to get a value or it could trigger an unwanted rerender
- **useCallback()** - for functions
- **useReducer()** can be an alternative

## Context

- for values accessed by many components
- can be nested and outer with non-changing values (for example dispatch) and inner with changing
- props from context api are also props (but hidden) so memo() can trigger rerenders with them

## Suspensing

- lazy and Suspense
- **useTransition()** - makes passed callback less urgent (so UI responsivness can be prioritized)

  ```
  const [isPending, startTransition] = useTransition();

  //in for ex event handler
  startTrasition(() => {expensiveFunc()})
  {isPending ? LoadingPane : Component}
  ```

- **useDefferedValue** - makes variable less urgent in depednecies arrays

  ```
  const defferedArticle = useDefferedValue(article)

  useEffect(() => {}, [defferedArticle])

  {article !== defferedArticle ? <loading pane /> : null}
  ```

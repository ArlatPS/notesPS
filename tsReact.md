# TypeScript and React

- typing props
  ```
  const MainPage = ({propName} : {propName: type}) => {}
  ```
- for specific React stuff hover on variables ex. from useState variable for setting has type: `React.Dispatch<React.SetStateAction<type>>`
- typing children `{children : React.ReactNode}` or `PropsWithChildren<{other props}>`
  ```
  type ButtonProps = React.PropsWithChildren<{
    onClick: () => void;
  }>;
  const Button = ({ children, onClick }: ButtonProps) => {
    return <button onClick={onClick}>{children}</button>;
  };
  ```
- typing useState `useState<type>()`
- useState with undefined value at start `useState<type | undefined >()`
- use useReducer when too many useStates >5
- in Reducers
  ```
  type Action = {
    type: "increment" | "decrement";
  }
  type ActionWithPayload = {
    type: "fromForm";
    payload: number
  }
  ```
- template literals type ActionTypes = \`update-${ColorFormats}-color\`
- custom type guards
  ```
  const isHexColor = (s: string): s is HexColor => {
    return s.startsWith("#);
  }
  ```
- TS utilities
  - `Partial<Type>` - makes all keys optional
  - `Omit<Type, "id">` - deletes a key

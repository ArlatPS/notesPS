# Complete Intro To React

## General
- install react react-dom react-router-dom
- simple app with older markup
    ```
    const App = () => {
    return React.createElement(
        "div",
        {},
        React.createElement("h1", {}, "Adopt Me!")
        );
    };

    const container = document.getElementById("root");
    const root = ReactDOM.createRoot(container);
    root.render(React.createElement(App));
    ```
- current syntax with jsx
    ```
    import { createRoot } from "react-dom/client";
    const App = () => {
        return (
            <div>
                <h1>Adopt Me!</h1>
            </div>
        );
    };

    const container = document.getElementById("root");
    const root = createRoot(container);
    root.render(<App />);
    ```
- good tools ESLint, Prettier, Vite
- **useState()**
    ```
    const [value, setValue] = useState(defaultValue)
    ```
    - in input `onChange={(e) => setValue(e.target.value)}`
- mapping through values
    ```
    array.map((object) => (
        <div>{object}</div>
    ))
    ```
- **useEffect(callback, arrayOfDependencies)**
    - if array empty it runs once at first render
- function for fetching
    ```
    async function requestSth() {
        const res = await fetch('url');
        const json = await res.json();
        setValue(json);
    }
    ```
- custom hooks created from predefined hooks
- controlled form 
    - for inputs onChange with setValue
    - on submit
        ```
        <form
            onSubmit={e => {
                e.preventDefault();
                function();
            }}
        >
        ```
- uncontrolled form
    - no onChanges
    - onSubmit
        ```
        e.preventDefault();
        const formData = new FormData(e.target);
        const obj = {
            property: formData.get("property") ?? "",
        };
        ```
- providing props
    ```
    <Results result={response}>
    const Results = ({result}) => {}
    
    <Pet animal={animal} name={name}>
    const Pet = (props) => {
        const {animal, name} = props;
    }
    ```
- React Strict Mode
    - wrap app in `<React.StrictMode>`
    - gives additional warnings about future versions
    - downside is that it runs initialization functions twice (it will fetch 2 times)


## React Router
- BrowserRouter, Routes, Route
    ```
    <BrowserRouter>
        <Routes>
            <Route path="/" element={<App />} />
        </Routes>
    </BrowserRouter>;
    ```
- better to use Link than a - better performance
    ```
    import { Link } from "react-router-dom";
    <Link to={'/'} >...</Link>
    ```
- **useParams() to get params (in url ${}) from BrowserRouter context**
- **useNavigate()**
    - to redirect
        ```
        const navigate = useNavigate();
        navigate("/")
        ```

## React Query
- @tanstack/react-query
- creating Client Provider
    ```
    import { QueryClient, QueryClientProvider } from "@tanstack/react-query"
    const queryClient = new QueryClient({
        defaultOptions: {
            queries: {
                staleTime: Infinity,
                cacheTime: Infinity,
            },
        },
    });


    <BrowserRouter>
        <QueryClientProvider client={queryClient}>
            ...
        </QueryClientProvider>
    </BrowserRouter>;
    ```
    - other time can be used
- **useQuery**
    ```
    const result = useQuery(["queryKey", id], requestSth)
    ```
    - if under Name is result for id, requestSth is not used
    - if there is no result it is called with this tuple as prop
        ```
        const requestSth = async({queryKey})=> {
            const id = queryKey[1];
        }
        ```
    - result.isLoading is set and can be used for loading panes
        ```
        if (result.isLoading) return <div className="loading-pane">
        ```
    - there is also isError, isFetching, isPaused ...
- for mutations useMutation hook

## Class Component
- import { Componment } from "react"
    ```
    class ClassName extends Component {
        state = {
            myValue: 0,
        };

        static defaultProps = {
            myProp: 1,
        };

        render() {
            const { myValue } = this.state;
            const { myProp } = this.props;
            return (JSX)
        }
    }
    ```
- this.setState({ key:val })
- lifecycle methods
    - constructor - initial state
    - componentDidMount - after first render (like useEffect with empty array)
    - componentDidUpdate - after updating state
    - componentWillUnmount - in useEffect return is called

## Error Boundary
```
import { Component } from "react";
import { Link } from "react-router-dom";

class ErrorBoundary extends Component {
  state = { hasError: false };
  static getDerivedStateFromError() {
    return { hasError: true };
  }
  componentDidCatch(error, info) {
    console.error("ErrorBoundary caught an error", error, info);
  }
  render() {
    if (this.state.hasError) {
      return (
        <h2>
          There was an error with this listing. <Link to="/">Click here</Link>{" "}
          to back to the home page.
        </h2>
      );
    }

    return this.props.children;
  }
}

export default ErrorBoundary;
```
 - wrap component in it
    ```
    const Results = () => {};
    export default ResultsErrorBoundary(props) {
        return (
            <ErrorBoundary>
                <Results {...props}>
            </ErrorBoundary>
        )
    }
    ```
    - use props when needed

## Portals
- separate mount point
- common use is a modal - above root `<div id="modal"></div>`
- **useRef**
    - container of state outside function closure state
    - always refers to the same thing (ex. DOM element)
- Modal.jsx
    ```
    import { useEffect, useRef } from "react";
    import { createPortal } from "react-dom";

    const Modal = ({ children }) => {
        const elRef = useRef(null);
        if (!elRef.current) {
            elRef.current = document.createElement("div");
        }

        useEffect(() => {
            const modalRoot = document.getElementById("modal");
            modalRoot.appendChild(elRef.current);

            return () => modalRoot.removeChild(elRef.current);
        }, []);

        return createPortal(<div>{children}</div>, elRef.current)
    }
    ```
- using Modal
    ```
    showModal ? (
        <Modal>
            <div>
                <h1>
            </div>
        </Modal>
    ) : null
    ```

## Context
- application level state
- use Context or Redux
- for ex user logged-in information
- creating context
    ```
    import { createContext } from "react";
    const AdoptedPetContext = createContext();
    export default AdoptedPetContext;
    ```
    - createContext returns an object with a Provider and a Consumer
- using provider
    ```
    import AdpoptedPetContext
    const adoptedPet = useState(null);
    <AdoptedPetContext.Provider value={adoptedPet}>
        ...
    </AdoptedPetContext.Provider>
    ```
- using consumer inside context **useContext()**
    ```
    import AdoptedPetContext
    const [adoptedPet, setAdoptedPet] = useContext(AdoptedPetContext)
    ```
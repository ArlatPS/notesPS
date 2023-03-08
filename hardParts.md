# Hard Parts of JS

## Main Components of JS Engine

- thread of execution - executing code line by line
- memory
  - Global Variable Environment
  - Variable Environment - local
- call stack
- Global Execution Context = thread of execution + Global Variable Environment
- Local Execution Context = thread of execution + Variable Environment

## Callbacks and Higher Order Functions

- functions are objects and can be
  - assigned to variables or properties of other objects
  - passed as arguments to other functions
  - returned as values from other functions
- higher order functions – functions that takes in a function or passes out one
- callback function – function taken as an argument and then called in an outer (higher-order) function
- used for simplicity and DRY
- applications
  - map, filter, reduce
  - often at job interviews
  - async JS - promises

## Closure

- hidden property of functions `[[scope]]` - backpack
  - with variables of local memory in which the function was declared/created and which are being used in that function
  - these variables can only be accessed by that function
- backpack is
  - **COVE** - Closed Over Variable Environment
  - **P.L.S.R.D. - Persistent Lexical Scope Reference Data**
  - Closure
- applications
  - helper functions - once and memoize (storing previously done input-output pairs)
  - iterators and generators
  - module pattern - not polluting global scope with generically named variables
  - asyncJS - promises and callbacks rely on closure
- once
  ```
  function once(passedFunc) {
    let used = false;
    function oncePassedFunc(arguments) {
      if (used) return "Can only be used once";
      used = true;
      return passedFunc(arguments);
    }
    return oncePassedFunc;
  }
  ```

### Iterators

- iterator function returns next element on every call - treating data more like flow than arrays
- simple array iterator
  ```
  function returnIterator(arr) {
    let i = -1;
    function iterator() {
      i++;
      return arr[i];
    }
    return iterator;
  }
  ```

## Asynchronous JS

- takes advantage of Web Browser Features (APIs) - JS itself is only synchronous
- An API (Application Programming Interface) is a software intermediary that enables two applications to communicate with each other.

### setTimeout(callback, ms)

- it's a label (interface/API) for Timer feature of Browser (written not in JS)
- when Timer finishes callback is pushed to Callback Queue
- when call stack `global()` is empty (after all synchronous code is done) Event Loop takes callback from Callback Queue to call stack
- callbacks can "wait" for execution more than declared ms - all synchronous code must me complete and Microtask Queue must be empty

### Promises

- **fetch("url")** - does two things: initiate xhr (xls http request) through the browser features and returns a Promise object
- Promise object has three important properties
  - **`.value`** - in which returned will be stored, but firstly it is undefined
  - **`.onFulfillment`** - hidden array of functions (that are pushed by **`.then`**) which will be pushed to MicroTask Queue when value is returned (ex. data fetched) and they automatically take .value as an argument
  - **`.onRejection`** - hidden array of functions pushed by **`.catch`**
- also properties .pending, .resolved, .rejected

### Queues

- when global synchronous code is finished (empty call stack) Event Loop goes to
- MicroTask Queue / Job Queue (specs) - first
- Callback Queue / Macrotask Queue / Task Queue (specs) - second

### Generators

- function generators make it possible to stop execution of a function with **`yield`**
- \* is mandatory
  ```
  function* funcName() {
    yield 4;
    yield 5;
  }
  ```
- calling this function returns an object with **`.next()`**
- calling `.next()` returns an object `{value: currentValue, done: boolean}`
- when next is called with an argument it will replace previous yield (assigning to variable)
  ```
  function* createFlow() {
    const num = 10;
    const newNum = yield num;
    yield newNum;
  }
  const returnNext = createFlow();
  const element1 = returnNext.next().value // 10
  const element2 = returnNext.next(2).value // 2
  ```
- async/await with generators and Promises

  ```
  // generator that will wait for data
  function* createFlow() {
    const data = yield fetch("url");
    console.log(data);
  }

  const returnNextElement = createFlow();

  // creating Promise
  const FutureData = returnNextElement.next();

  //pushing to .onFulfillment
  FutureData.then(doWhenDataReceived)


  // on promise fulfillment it will place value to data variable in generator (into yield)

  function doWhenDataReceived(value) {
    returnNextElement.next(value);
  }
  ```

### Async/Await

- syntactic sugar to generators
- they are still resolved after synchronous (like Promises)

```
async function createFlow() {
    console.log("first")
    const data = await fetch("url");
    console.log(data);
}

createFlow();
console.log("second")

// first
// second
// data
```

## OOP

- `const newObj = Object.create(objName)`
  - creates new empty object
  - with hidden property `__proto__` that links to objName which gives newObj access to objName methods
  - sophisticated and very explicit
  - behaviour delegation
- `.hasOwnProperty()` from `Object.prototype`
- implicit parameter `this`
  - obj.method() - implicit this
  - this is obj
  - arrow function is lexically scoped and has different this behaviour (this refers to lexical scope of where the function was declared)
- ## keyword `new`
  - same behaviour as Object.create() but with some automation
  1. create empty object this
  2. set `this.__proto__` to funcName.prototype
  3. return this
- functions have 2 parts
  - () function part
  - . object part which has object prototype which can be populated with methods
- class = function(constructor) + object - it is really only syntactic sugar

## Functional Programming

-

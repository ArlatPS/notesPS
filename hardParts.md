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
  - **`.onFulfilment`** - hidden array of functions (that are pushed by **`.then`**) which will be pushed to MicroTask Queue when value is returned (ex. data fetched) and they automatically take .value as an argument
  - **`.onRejection`** - hidden array of functions pushed by **`.catch`**
- also properties .pending, .resolved, .rejected

### Queues

- MicroTask Queue / Job Queue (specs) - first
- Callback Queue / Macrotask Queue / Task Queue (specs) - second

### Generators

- function generators make it possible to stop execution of a function with **`yield`**
- \* is mandatory
  ```
  function* funcName() {
    yield 4;
    yeidl 5;
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

  //pushing to .onFulfilment
  FutureData.then(doWhenDataReceived)


  // on promise fulfilment it will place value to data variable in generator (into yield)

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

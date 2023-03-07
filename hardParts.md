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

### setTimeout(callback, ms)

- it's a label (interface/API) for Timer feature of Browser (written not in JS)
- when Timer finishes callback is pushed to Callback Queue
- when call stack `global()` is empty (after all synchronous code is done) Event Loop takes callback from Callback Queue to call stack
- callbacks can "wait" for execution more than declared ms - all synchronous code must me complete and Microtask Queue must be empty

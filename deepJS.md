# Deep JS Foundations

## Additional things

- A polyfill is a piece of code that provides a service that you, the developer, expect it to provide natively

# Types

## Basic types

- primitive types:
  - undefined
  - string
  - number
  - boolean
  - symbol
  - bigint
- variables don't have types, values do
- typeof varName returns a string ex. "number"
  - exception typeof null returns "object" !!
- special values
  - NaN - invalid number
    - only value in JS with no identity property and so it is **not equal to itself!!**
    - isNaN() - does coersion and then checks if number is NaN
    - Number.isNaN() - without coersion
  - -0
    - Object.is(var, -0) to really test for it (=== is not enough)
    - also option with -Infinity
  - fundamental objects
    - use with new: Object(), Array(), Function(), Date(), RegExp(), Error()
    - don't use with new: String(), Number(), Boolean()

## Coercion

- abstract operations - type conversion = **coercion**
  - ToPrimitive(hint)
    - hint == "number" -> valueOf(), toString()
    - hint == "string" -> toString(), valueOf(),
  - ToString() - corner cases
    - -0 -> "0"
    - [] -> ""
    - [null,undefined] -> ","
    - {} -> "[object Object]"
  - ToNumber()
    - **the worst: "" -> 0**
    - null -> 0
    - undefined -> NaN
    - [""] -> 0 , [undefined] -> 0 - all because "" -> 0
  - ToBoolean() - just a lookup, no coercion
    - falsy values: "", 0, -0, null, NaN, false, undefined
    - rest is truthy
- **boxing** - implicit coercion which turns primitives into objects ex. `"myStr".length`
- coercion to number with `+"16"`
- `>` and `<` operators perform coersion to Number

## Equality

- if type of both values is the same then == peforms === (strict comparison)
- **== allows coercion and === doesn't**
- coercive equality 12 == "12", **null == undefined**
- both don't check structure of objects so in their case == performs identity comparison
- == prefers numeric comparison (12 == "12" into 12 == 12)
- if type is non-primitive then == will perform ToPrimitive ex.
  ```
  42 == [42]
  42 == "42"
  42 === 42
  ```
- corner case with [] and boolean
  ```
  //wrong approach
  [] == true
  "" == true
  0 === 1
  //right approach
  if ([]) -> true
  if (Boolean([])) -> true
  ```
- avoid
  - == with 0 or ""
  - == with non-primitives
  - == boolean
- **== is not about comparing unknown types, but about comparing known types where conversion is helpful**
- **_using === is generally not neccesary and == is better_**

# Scope

- JS organizes scopes with functions and blocks `{}`
- **shadowing** - in inner scope the same identifier (varName) as in outer scope
- lexical scopes are determined during compliation (parsing)
- identifier as a **target** (L side) and as a **source** (R side)
- `"use strict"` prevents auto globals (when no identifier found in scope JS automatically creates variable in global) - in babel as default
- ReferenceError
- undefined - variable exists but without a value
- undeclared - it doesn't exist, will throw ReferenceError

## Functions

- function declaration `function funcName() {}`
- named function expression `var varName = function funcName() {}`
- anonymous function expression `var varName = () => {}`
- benefits from named: recursion possible, more debuggable stack trace, self-documentation
- **IIFE** - immediately invoked function expression
  ```
  (function funcName(parameterName) {
    ///
  }) (argument)
  ```
  - IIFEs great with try and catch

## Block Scoping

- with just {} and let/const
  - example
  ```
  {
      let name = "Kyle"
  }
  ```
  - var wouldn't work because in block scopes it binds to outer/global scope - it's better for functions and global scope
- var should be used in global or in functions or for escaping unintented scope like in try catch
- var can be redeclared
- const can't be reassinged but its value can be mutated
- var > const > let
- with let in a block the variable starts as uninitialized not undefined like var and if one tries to source reference it before its declaration **TDZ error (temporal dead zone)** will be thrown
- **hoisting** is a short-hand term for what really happen during parsing with variables and functions (attaching them to scopes and in case of fucntion declarations you can put them after executable code)
- for let loop creates new variable in every iteration, but for var it doesn't and a classic with setTimeout in for (var = 1, i < 3, i++) and 4,4,4

## Closure

- Closure is when a function “remembers” its lexical scope even when the function is executed outside that lexical scope.
- **namespace** - object with properties - vars and functions
  ```
  var workshop = {
    teacher: "Kyle",
    ask(question) {
      console.log(this.teacher, question);
    }
  };
  workshop.ask("I have a question");
  ```
- modules encapsulate data and behavior (methods) together. The state (data) of a module is held by its methods via closure.
- classic/revealing module pattern
  ```
  var workshop = (function Module(teacher){
    var publicAPI = {ask, };
    return publicAPI;
    function ask(question) {
      console.log(teacher, question);
    }
  }) ("Kyle);
  workshop.ask("new question");
  ```
- module factory
  ```
  function Module(teacher){
    var publicAPI = {ask, };
    return publicAPI;
    function ask(question) {
      console.log(teacher, question);
    }
  };
  var workshop = Module("Kyle");
  workshop.ask("new question");
  ```
- ES6 module patter with different files and export

# Objects

## this

- this reference/binding is determined by how the function is called
- 4 ways function can be called
  - **varName.methodName()** - this refers to varName scope (binding can be implicit - function in varName object or dynamic - its outside and only assigned inside varName)
  - **funcName.call(contextObject, arguments)** - explicit binding - this refers to contextObject
  - **varName.methodName.bind(contextObj)** - hard binding - preserves current state
  - **new funcName(args)** - new binding
- steps of new keyword
  - create new empty object
  - link that object to another object
  - call function with this as new object
  - if function doesn't return an object, return this
- **determination of this**
  - is function called by new - `{}`
  - is function called by .call(), .apply(), or .bind() - explicit context
  - is function called on an object - object
  - in non strict mode default global object
- **arrow functions don't define local bindings for this**, so it uses lexical this (outer)

## Prototypes

- call with new makes an object linked to its prototype

  ```
  function Workshop(teacher) {
    this.teacher = teacher
  }
  Workshop.prototype.ask = function() {}

  var deepJS = new Workshop("Kyle")
  deepJS.contructor === Workshop
  deepJS.__proto__ === Workshop.prototype
  Object.getPrototypeOf(depJS) === Workshop.prototype
  ```

- .prototype to assign
- `__proto__` (dunder proto) to get prototype of an object
- objects are linked - behavior delegation
  - `Object.create(Workshop.prototype)`
  - `Object.assign()`
- delegation design pattern

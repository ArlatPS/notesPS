# TypeScript Introduction

## General
- open source syntactic superset of JS
- .ts is compiled to .js and .d.ts with types
- package.json with devDependendcy typescript
- tsonfig.json
    - outDir
    - target - which JS
    - include

## Type Categories
- literal type ex. `const num = 6` - can only be 6
- inference - TS makes as specific as possible /reasonable assumption about type
- types are deined when sth is declared
- type annotation `let num: number`
- in fucntions return types   
`const func = (arg: type): type => {}`
- objects
    ```
    let car: {
        make: string
        year: number
    } = {...}
    ```
    - optional properties `{ propertyName?: number }` - so type is number | undefined (but it is not the same as defining with | undefined)
- type guard - if or switch or throw Error statements to guarante right type
- dictionaries - type with index signature `[k: string]`
    ```
    const dicName: {
        [k: string]: {
            prop: string
        } | undefined
    }
    ```
- arrays `type[]`
- tuple `[string, number, string]` - set amount of values in order
    - `const numPair: readonly [number, number] = [4,5]`
    - **readonly with arrays prevents .push, .pop**
- types of Types
    - TS type system is static - type-checking is performed during compilation
    - dynamic type systems check during runtime - JS
    - nominal system - about names of classes
    - structural system like TS - about shape
    - JS - duck typing (looks like a duck, so duck)
- union types - OR - |
    - then narrowing with type guards
    - discriminated or tagged union (like in a tuple with first position "Success" or "Error")
- intersection types - AND - &
- type aliases
    ```
    type Name = {
        name: string
    }
    ```
    - kind of inheritance - `type Name = Date & {sth}`
- interfaces
    ```
    interface IName {
        name: string
    }
    ```
    - more limited - only describes object type
    - **intefaces are open - can be augmented by calling interface IName {} again - it adds/modifies**
    - inheritance
        - **class Dog implements AnimalLike**
        - **interface Name extends ClassName**
- interfaces and types can be recursvie
    ```
    type nestedNumbers = number | nestedNumbers[]
    ```

## Functions
- **callable types / call signatures**
    - **`interface IFunc {(x: type): type}`**
    - **`type Func = (x: type) => type`**
- example
    ```
    type TwoNumberCalc = (x: number, y: number) => number
    const add: TwoNumberCalculation = (a, b) => a + b
    ```
- void fro functions that dont return
- construct signatures - keyword new
- fucntion overload
    - ex. first argument defines second
    - define possible combinations in fucntion heads
    ```
    function handleMainEvenet(
        elem: HTMLFormElement
        handler: FormSubmitHandler
    )
    function handleMainEvenet(
        elem: HTMLFrameElement
        handler: MessageHandler
    )
    function handleMainEvenet(
        elem: HTMLFormElement | HTMLFrameElement
        handler: FormSubmitHandler | MessageHandler
    ) {}
    ```
- this also needs type `functionName(this: type)`

## Classes
- typing class fields
    ```
    class Name {
        property: type
        constructor(arg: type) {}
    }
    ```
- access modifier keyword
    - public - everything
    - protected - instance of a class and subclasses
    - private - only the instance `#name`
- readonly can be added
- param properties syntax
    - old
    ```
    class Car {
        make: string
        constructor(make: string) {
            this.make = make
        }
    }
    ```
    - new
    ```
    class Car [
        constructor(public make: string) {}
    ]
    ```
- constructor order of operation
    1. super()
    2. param property initialization
    3. other class field inittialization
    4. other code in constructor after super()

## Types
- top types T: any and unknown
    - with unknown variables can't be accessed without type guard
- bottom type never
    - exhaustive conditional
    ```
    if for all possbile types and
    } else {
        const neverValue: never = never
    }
    ```
    - or throw an error there
- type guards
    - instanceof
    - typeof
    - ===
    - !
    - Array.isArray()
- user defined type guards
    - value is Type
    ```
    function returnsBoolean(value: any): value is Type { return... }
    ```
    - assert value is Type
    ```
    function assertSomeType(value: any): assert value is Type { if (!()) throw new Error }
    ```
- nullish values
    - null - there is a value and it's nothing
    - undefined - value isn't avaliable (yet?)
    - void - return from a function should be ignored
- non-null assertion operator !.
- definitive assignment operator !:
    - used to supress TS objections about class field being used, when it cant be proven that it was initialized
    - for example with asynchronistic constructor

## Generics
- type param `<T>`
    - `function Name<T,C>(){}`
    - T and C can be used as types in function
    - can be inferenced by TS when using function
    ```
    function wrapInArray<T>(arg: T): [T] { 
        return [arg]
    }
    ```
- constraint `<T extends Type>`
- type casting
    - sth as sth
    - `5 as string`


# TypeScript Intermediate

## General
- things (intefaces, variables, namespaces) with the same name can stack on top of each other (also when exporting)
- they can be differenciated by trying to use them
- classes are 2 things stacked (**declaration merging**)
    - a value - Class itself - can invoke static methods on that
    - a type - interface that describes an instance of a class
- Namespace Import
    ```
    import * as myModule from ""
    ```
    - import all exports
- most of commonJS conversion
    ```
    const fs = require("fs")
    into
    import * as fs from "fs"
    ```
- alternative way if namespace import fails
    ```
    export = sth
    import sth = require("")
    ```
- to accomplish this in TS
    ```
    import img from "./file.png"
    
    // @filename: global.d.ts
    declare module "*.png" {
    const imgUrl: string
    export default imgUrl
    }
    ```

## Conditional Types
- keyof obtains type representing all property keys of a given interface
    ```
    type DatePropertyNames = keyof Date
    ```
- to get only keys that are strings
    ```
    type DatePropertyNames = keyof Date & string
    ```
- typeof - extracting type from a valuer
    - typeof ClassName extracts static site of class
- ternary TS expression
    ```
    type CookingDevice<T> = T extends "grill" ? Grill : Oven
    ```
    - extends means that T is at least of this type (sort of >=), but really the right side is bigger
    - never extends any - true
- `type ExtractedType = Extract<TypeFrom, type>`
    - its not an exact match - minimum requirement
    - extracting a subset of TypeFrom which is assignable to type
- Exclude<> - opposite
- infer can be used to extract a type
    ```
    type ConstructorArg<C> = C extends {new (arg: infer A): any } ? A : never
    ```
- access indexed type `[""]`  
    `let CarColor: Car["color"]`  
    `["key"|"key"]`


## Mapped Types
- index signature parameter `[key: type]` can't be a union type, so:
- `{ [NamedKey: in "some"|"some2"] }`
    ```
    type MyRecord<KeyType extends string, ValueType> = {
        [key in KeyType]: ValueType
    }
    ```
- build in Record and Pick
- modifiers
    - `[P in keyof T]?:` optional
    - `[P in keyof T]-:` requierd
    - `readonly [P in keyof T]`
- template literals
    ```
    type ArtMethodNames = `paint_${Colors}_${ArtFeatures}`
    ```
- special types to use in template literals
    - UpperCase
    - LowerCase
    - Capitalize
    - Uncapitalzie
    - ex. `${Capitalize<Color>}`
- key remapping
    ```
    [K in keyof DataState as `set${Capitalize<K>}`]
    ```
- filtering
    ```
    type FilteredKeys<T, U> = {[P in keyof T]: T[P] extends U ? P : never}[keyof T] & keyof T
    ```
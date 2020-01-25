# JavaScript - Interpreted

## Script tag
- inline JavaScript
- obselete attributes:
  - `type` (e.g. text/javascript)
    - modern HTML redefined the definition
  - `language`: JavaScript is default now
- useful attributes:
  - `src = "path_to_js_file"`
    - useful because it can be [cached](#cache)
    - if `src` is set, the inline code will be ignored

## Semicolons
- There is implicit semicolons (automatic semicolon insertion)
- based on lexical analysis and terminating token such as ')' or '}'
- **should always write semicolons - not dependable**

## Comments
- cannot be nested :P

## Modern JS
- ES5 modified some of the existing functionality and are off by default
- `use strict` enables the ES5 changes
- always have it on top of the scope (globally or function body)
- cannot be cancelled part way

## Variables
- `var`:
  - old
  - always processed at the beginning of script (regardless of actual declaration position or if conditions are met) (hoisting)
  - *assignments* are not hoisted (not pushed to the top): ran sequentially in the order of statement position
  - has no block scope: only function-scoped or global
  - ``` 
    if (true) {
      var test = true; // use "var" instead of "let"
    }
    alert(test); // true, the variable lives after if
    ```
- `let`: has block scope(e.g. inside `if`) 
- `const`: cannot be assigned after first assignment (usually in capital letters)
- naming: can only use numbers, letters, $, and _
- **tldr: `var` declarations are hoisted and minimum function scoped while `let` can be block scoped**

## IIFE (obscelete)
- immediately invoked function expressions
- used for instant variable declarations
- must be wrapped in ()
- ```js
  (function() {
    let message = "x";
  })
  ```
## Numbers
- `NaN` takes precedence: propagates through all expressions if exists
- biggest number is 2^53
- if the number ends in 'n' it means "bigint"

## Strings / Characters
- no such thing as a char type in JS

## Null
- just means empty or nothing or unknown

## Undefined
- not assigned/defined
- can technically assign `undefined`, but should just use `null`

## Objects/Symbols
- everything else is **primitive** since it's only 1 single thing
- symbols are used to create unique identifier for objects

## typeof
- defined as operator or function
- ```js
  typeof undefined // "undefined"
  typeof 0 // "number"
  typeof 10n // "bigint"
  typeof true // "boolean"
  typeof "foo" // "string"
  typeof Symbol("id") // "symbol"
  typeof Math // "object"  (1)
  typeof null // "object"  (2) - techically wrong(error within language)
  typeof alert // "function"  (3)  
  ```



---
# Extras

## Cache:
- files will be cached when attached as a source using the `script` tag
- allows a response to be re-used without calling it again
  - can be controlled by passing a `max-age` header
- any other pages that references the same script will not need to download again
- **reduces traffic and faster renders**

### [ECMA2020](https://tc39.es/ecma262/#sec-ordinary-and-exotic-objects-behaviours)
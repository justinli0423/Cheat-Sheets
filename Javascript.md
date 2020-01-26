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

## Boolean
- "0" is true
- " " also true(any non-empty string)

## Conversion
- having "+" with a string always results a string(e.g. '3' + 2 = '32')
- it still runs left to right (2 + 2 + '3' = '43')
- all other operators convert to numbers

## Increment / decrement
- count++ is the same as ++count unless the return value is used
- count++ returns count and then add
- ++count returns count + 1

## Comma operator
- evaluate multiple expressions, but keep only the last one

## String comparison
- lexicographical order(O(n) -> compares every string until one differs)
- it compares to the index of the js 'dictionary'(unicode)
- lowercase > uppercase
  
*Special: "0" is true but 0 is false. but 0 == "0"*

## == (regular) vs === (strict)
- `==` uses type conversion
- `===` compares types first
- `null === undefined`: false
- `null == undefined`: true
- in math comparisons: null -> 0 and undefined -> NaN
- `null == 0` is false while `null >= 0` is true
*Special: equality check does not convert to number while comparisons (>, >=, etc) does*

## Conditionals
- 0, empty string, null, undefined, NaN are all falsy
- conditional(ternary) operator should only be used with assignment, not function calls (designed this way, but it still works)
- ternary *cannot* contain break/continues even if it's in a loop, must use `if`

## OR operator (find first truthy value)
- if used as assignment, it will return the first 'truthy' evaluation or the last operand (e.g. x will be undefined under `true || (x = 1)`)

## AND operator (find first falsy value)
- if used as assignment, it will return the first 'falsy' evaluation or the last operand

*Special: the precedence of && is higher than || so use brackets*

## Labels
- used for references outer loops in nested loops
- can directly break up to the labeled loop using `break <label>;`
- usage: `<label> : for(let...)...`
- must be of higher scoped to break/continue (not a "skip to line x")

## Switch 
- must include `break` or it will run all the cases under the truth one
  - can be used to group cases together
- case comparisons are strict (no object conversion)

## Function scoping
- will use outer variables unless local one is defined (shadowing)
- if local one is defined, it is *not* linked to the outer variable
- functions will always get a *copy* when passed in parameter(only primitive types)

## Default values
- can define defaults to be anything (not just string, can be function return value)

## Return statement
- if a return value is not provided or no return statement, it returns `undefined`;
- multi-lined return statements must be enclosed in ()

## Function
- declarations: `function test() {}` 
  - this is visible to everywhere inside the scope, before or after
  - JS looks for global func declarations and creates it first
  - in `use strict`, declarations are only available inside code block
- expressions: `let x = function() {};`
  - same as assignment; will not exist until the statement has been processed
- function is a "special value": when calling it without (), it shows the source code as a string
- can copy functions (e.g. `let y = x;` or `let y = test;` will give y the function of x)
- function expressions require ';' because it's an assignment, so it is used to terminate the statement

---
# Extras

## Cache:
- files will be cached when attached as a source using the `script` tag
- allows a response to be re-used without calling it again
  - can be controlled by passing a `max-age` header
- any other pages that references the same script will not need to download again
- **reduces traffic and faster renders**

### [ECMA2020](https://tc39.es/ecma262/#sec-ordinary-and-exotic-objects-behaviours)
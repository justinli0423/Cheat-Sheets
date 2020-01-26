# <JavaScript - Interpreted />

# The Fundamentals

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

## Functions
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
# Objects: The Basics

## Objects
- always stored/accessed by reference to memeory
- object comparisons are done by checking memory, not value
- `const` objects *properties* can be changed - memory doesn't change since reference is same
  - allowed: `const_object.key = "new value";`
  - not allowed: `const_object = { key: "same value" };`
- simple cloning (depth of 1):
  - option 1: iterate and assign to new object
  - option 2: `Object.assign(destination, [object1, object2, ...]);`
    - takes all properties of the array of objects and puts into destination object
    - duplicate keys will be *overwritten*
    - returns the new object
    - this operates the same as option 1, but looks nice
- keys do not require quotes unless it contains spaces
- accessing multi-word keys should be done with `object['key with spaces']`
- `delete object.property`
  - returns true if successful, false otherwise
  - nothing to do with freeing memory, only breaks the reference (otherwise properties would be floating randomly in memory)
- object property names have no restrictions (can use keywords)
- way to test if key has been created (regardless of value): `"property" in object`
- looping through objects:
  - `for (let key in object) {}`
  - only integral keys are sorted (means if we convert to number and back, it's still the same)
    - can be bypassed by appending "+" infront of the integers so that it's not the same after `f(f^-1(x))`
  - other keys are kepy in creation order
- **property value shorthand**
  - if input param is the same as keyname, you can ignore the value input
  - ```js
    function makeUser(name, age) {
    return {
      name, // same as name: name
      age   // same as age: age
      // ...
    };
    ```
}
- **computed properties**
  - can use `[]` to use computed property as the key instead of the word itself
  ```js
  let fruit = prompt("Which fruit to buy?", "apple");

  let bag = {
    [fruit]: 5, // the name of the property is taken from the variable fruit
  };

  alert( bag.apple ); // 5 if input to prompt was apple
  ```

## Objects - `this`
 - references the object it is scoped in
 - `this` is evaluated at call time, not the position of declaration
 - can be called inside any function
   - if the function is not assigned to an object, it will return `undefined`
 - arrow functions have no `this`, it will reference the outer 'normal' function

### Advanced `this`
  - ```js
    let user = {
    name: "John",
    hi() { alert(this.name); },
    bye() { alert("Bye"); }
    };

    user.hi(); // John (the simple call works)

    // now let's call user.hi or user.bye depending on the name
    (user.name == "John" ? user.hi : user.bye)(); // Error!
    ```
  - object dot method works (`user.hi()`)
  - evaluated method doesn't
  - *tldr*: `obj.method()` uses the `.` to get the property and then `()` to execute it
  - evaluated methods fail to get this because it becomes `let hi = user.hi` first, which no longer has scoping (global `this` returns undefined)
    - it assigns it to the IIFE brackets
  - *JavaScript returns using a special type called the 'reference type' which is how the dot and bracket operators work on to get the object reference*
    - `function.bind()` is a useful tool

## Object to Primitive conversion
  - happens when operations occur between objects (`obj1 + obj2`)
  - all objects are true in boolean context
  - operators usually cause numeric conversion
  - outputs usually cause string conversion
  - 3 types of conversions (called 'hints')
    - string
    - number
    - default - when not sure what to do
  - *no boolean hint because all objects are default true*
  - JS tries all 3 object methods through `obj[Symbol.toPrimitive](hint)`
    - this can be self defined within the object as well
    - hint = string: tries `obj.toString()` before `obj.valueOf()`
    - hint = number/default: tries `obj.valueOf()` before `obj.toString()`
  - primitive conversion methods do not *necessarily* return the "hinted" primitive, but they *must* return some sort of primitive
  - ```js
    const user = {
      name: 'jhon',
      salary: 1000,

      [Symbol.toPrimitive](hint) {
        return hint === 'string' ? this.name : this.salary;
      },
      toString() {
        return this[Symbol.toPrimitive]('string');
      },
      valueOf() {
        return this[Symbol.toPrimitive]('number');
      },
    };
    ```

### toString / valueOf
  - `toString()` returns a string `[object Object]` unless overwritten
  - `valueOf()` returns the object itself unless overwritten
  - to fully convert, we should implement these methods inside the object and JS will use these during the `toPrimitive` operation
  - `toString()` will handle all conversions if other methods are absent

## Constructor and `new`
  - Constructor function
    - named capital letter
    - only executed with `new` keyword
  - When `new` is called on constructor functions
    1. new empty object is created
    2. constructor function is executed and `this` will reference the new object
    3. returns `this` 
  - technically any function can be ran with `new`

---
# Data Types

## Primitive Methods



---
# Extras

## Console.log()
  - not the same as `alert()`
    - it does not "expect" a certain type
    - will print it to the console (object -> prints object tree)
    - `alert()` expects a string

## Garbage Collection
  - *reachability*: any values that can be accessed (in any way) will be stored in memeory (e.g. references, local variables, etc)
    - e.g. if an (and only) object reference is overwritten, then garbage collector will clear the old object since it's unreachable
  - the outgoing links do *not* matter, if the object cannot be accessed it's garbage
  - the *mark-and-sweep* algorithm
    - garbage collector (gc) will mark all roots
    - traverse through the graph and marks all references
      - repeats until all reachable references are visited
    - all objects that are *unmarked* will be freed
  - additional optimizations to MS algo
    - generational collection: split objects into "new" and "old"
      - the old ones are checked less often
    - incremental collection: 
      - break apart massive objects so that engine isn't delayed for a long time but instead `n * short periods`
    - idle-time collection: only run the MS algorithm when CPU is idle so that user isn't effected

## Cache:
- files will be cached when attached as a source using the `script` tag
- allows a response to be re-used without calling it again
  - can be controlled by passing a `max-age` header
- any other pages that references the same script will not need to download again
- **reduces traffic and faster renders**

### [ECMA2020](https://tc39.es/ecma262/#sec-ordinary-and-exotic-objects-behaviours)
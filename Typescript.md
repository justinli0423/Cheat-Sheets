# TypeScript
*Note: use `let` instead of `var` whenever possible*

## Basic Types
`let <varName>: <type> = <default>;`
Types:
- boolean
- number (can be declared in any radix)
  - `let hex: number = 0xf00d;`
- string
- array (2 ways to declare)
 - `let list: number[] = [1,2,3];`
 - `let list: Array<number> = [1,2,3]'`
- tuple: fixed number of elements with known types, but not the same
 - `let x: [string, number] = ['hello', 1];`
 - Note: the element typing is preserved, the following will fail
   - `console.log(x[1].slice())`
 - Note: accessing indicies outside of known elements throws an error
- enum: giving names to numerical values
 - ```
    enum Color {
        Red = 1, // Overriding index
        Green,
        Blue,
    }
    let c: Color = Color.Green;
    let colorName: string = Color[2]; // Green
    ```
    - Indicies start at 0 by default, but then can be overwritten like above
    - Can also access name by index like above
- any: opt out of type checking
  - `let notSure: any = 4;`
- object: respresents the non-primitive type
  - `let objectName: object = {};`
  - Note: object type only allow value assignments. You cannot call arbitrary methods on them, even if they exist
- void: opposite of any (commonly used in function return types)
  - `function warnUser(): void {console.log('error')}`
  - Note: void typing can only be assigned `null` and `undefined`
- null/undefined: 
  - has their own typing of `undefined` and `null` respectively
  - `null` and `undefined` are subtypes of all other types
  - `--strictNullChecks` will alter these definitions
- never: types that can **never** occur
  - usually used in functions that always throws exception/error (aka *never* returning)
  - the `typeof(never) = never`
  - `never` is a subtype of every type; however, **not** type is assignable to `never` (except `never` itself)
  - `function error(message: string): never {throw new Error(message)}`
  - `function infLoop(): never {while(true) {// do something}}`

### Type assertions
- lets the compiler know to trust you
- referred to as "type cast" in other languages, but performs no special checking or restructuring of data.
- type checking has 2 forms:
  - `let stringLength: number = (<string>someStringVariable).length;`
  - `let stringLength: number = (someStringVariable as string).length;`


## Interface
```
interface LabeledValue {
    label: string; // requirements
}

function printLabel(labelObj: LabeledValue) {
    console.log(labelObj.label);
}

let myObj = {
    size: 10,
    label: 'size 10 obj',
}

printLabel(myObj);
```

- the compiler on checks that `labelObj` has the property `label`. In other words, it checks that *at least* the ones required are present

### Optional Properties
```
interface LabeledValue {
    label?: string;
    color?: string;
}

function printLabel(labelObj: LabeledValue) {
    // will throw error here because width is not defined in interface
    console.log(labelObj.width);
}

let myObj = {
    size: 10,
    label: 'size 10 obj',
}

printLabel(myObj);
```

- optional properties are written with a `?` attached at the end
- this determines the possibly available properties while preventing properties that are not in the interface
- *Note: objects are required to have atleast the required properties, but accessing a properties that is neither optional or required will have syntax error, but will still compile*

### Readonly Properties
```
interface Point { 
    readonly x: number;
    readonly y: number;
}
```
This can only be modified at declaration stage

Typescript also comes with a read-only array `ReadonlyArray<T>` that has all mutating methods removed
- assigning a `ReadonlyArray` will also fail, but type assertion will work
  - `let a: number[] = <someReadOnlyArray> as number[];`

*Note: Variables use `const` while properties use `readonly`*

### Excess Property Checks
TypeScript will throw error if there are *excess* types that aren't defined as an property
- to bypass, type assertion is necessary `let mySquare = createSquare({ width: 100} as SquareConfig);`
- a better way: allow any number/type of properties as long as they are not `color` or `width`
  ```
  interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any;
  }
  ```
- last way: assign it to a variable will bypass excess property checks as well

### Function Types
can use interface to describe `object` and `function` type
```
interface SearchFunc {
  (source: string, subString: string): boolean;
}
```
- the names of parameters do not have to match
```
let mySearch: SearchFunc;
mySearch = function(src: string, sub:string): boolean {
  // do something
}
```
- TypeScript's contextual typing can infer the argument types so *technically* you do not need to specify typings (this applies to both arguments and return values)

### Indexable Types
```
interface StringArray {
  [index: number]: string;
}
```
- only `string` and `number` can be index signatures
- JavaScript converts `number` to `string` when indexing: `arr[100] === arr["100"]`

Can also be used to describe the "dictionary" pattern - enforcing all properties match their return type
```
interface NumberDictionary {
  [index: string]: number;
  length: number; // makes sense
  name: string // will fail here since the indexer is declared return type of number
}
```
Can be fixed if a `union` is used
```
interface NumberDictionary {
  [index: string]: number | string;
  length: number; // makes sense
  name: string // will work now
}
```
Can also be made as `readonly` to prevent assignment to their **indicies**
```
interface ReadonlyStringArray {
  readonly [index: number]: string;
}
let myArr: ReadonlyStringArray = ['Alice', 'Bob'];
myArray[2] = 'Mallory'; // error, cannot assign to indicies
```

### Class Types (Interface Implementation)
enforcement of class to meet a particular contract
```
interface ClockInterface {
  currentTime: Date; // property
  setTime(d: Date): void; // function
}

class Clock implements ClockInterface {
  currentTime: Date = new Date();
  setTime(d: Date) {
    this.currentTime = d;
  }
  constructor(h: number, m: number) { }
}
```

*Note: Interface only describes the public side of the class; privates/static will not be checked*

**Recall:**
- static: shared across all instances
- instanced: different memory per instance
  
To check against static:
```
interface ClockConstructor {
  new (hour: number, minute: number): ClockInterface;
}

interface ClockInterface {
  tick(): void;
}

function createClock(constr: ClockConstructor, hour: number, min: number): ClockInterface {
  return new ctor(hour, minute);
}


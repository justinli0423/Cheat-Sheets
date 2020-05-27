# MobX - React Cheatsheet

## Idea
Simplifies state management by making it impossible to produce an *inconsistent* state - Make sure that everything that can be derived from the application state be derived automatically

![state diagram][stateDiagram]

### Application State
Graphs of objects, arrays, primitives, references that forms the model of the application

### Derivations
Values that can be computed automatically from application state (formulas)

### Reactions
Similar to derivations, but these do not produce values - these run automatically to perform some task (ensures that DOM is updated or requests are made at the right time)

### Actions
All things the **change** the state. MobX ensures that all changes to application state caused by actions are automatically processed by derivations and reactions.

## `@observable` decorator
Signal MobX that these values change over time
```
@obervable todos = [];
```

## `@computed` decorator
Signal MobX that these are derived from the state so they need to be recalculated when state changes
```
@computed get report() {
    // do something with state
}
```

## `autorun`
Creates a *reaction* that runs once, and after that whenever any observable data inside the callback changes (even if it is a property of an `observable` type)
```
mobx.autorun(() => {
    // do something
})
```

## `@observer`
Wraps the React component in `autorun`. Removes the need to call `setState`

## Working with References
Wrap the object in `mobx.observable(<obj>)` (it returns the object), everything will be automated
```
let peopleStore = mobx.observable([
    { name: "Michel" },
	{ name: "Me" }
]);
```

[stateDiagram]: https://mobx.js.org/assets/getting-started-assets/overview.png
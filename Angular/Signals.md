
A **signal** is a special container that holds a value. When another part of your code references this signal, it becomes a **dependent**. In other words, when the signal value changes, other values that use that signal will also change.

Signals are useful for many reasons:

>1. **Reactivity**- Templates will update automatically when the signal value changes, "reacting" to the change in data.

>2. **Simple states** - previously, state management was often done in RxJs, specifically with ***Subjects*** or ***Behavior Subjects*** . With signals, you don't have to worry about data streams or subscriptions. Signals are ==synchronous==, so the code happens in the order you write it.

>3. **State sharing** - reactive state can directly be passed into components. Event emitters and decorators are not necessary anymore for state sharing, eliminating extra boilerplate.

>4. **Computed state** - with the ==computed== signal, you can derive new state from existing signals. 

>5. **Effects** - when a signal's state changes, ==effects== allow code snippets to run in response, allowing for side effects to happen.


# Writable Signals

 **Writeable signals** let you read and update their values whenever desired. The `signal()` function is used to instantiate them. 
 
 ```typescript
const count = signal(0) // requires an initial value

// signals are getter functions - calling them reads their value
console.log('The count is: ' + count());

// to change the signal value completely, use .set()
count.set(100000)

// to update a signal using previous value, use .update()
count.update(val => val + 1)
```

Writable signals have the `WriteableSignal` data type.

# Computed signals


**Computed signals** derive new state from one or more other signals. When one of the dependent signal values changes, the computed signal value will also change. Computed signals are read-only, so they cannot be changed from the outside.

```typescript
const count: WritableSignal<number> = signal(0);
const doubleCount: Signal<number> = computed(() => count() * 2);
```

`doubleCount` will _always_ be updated when `count` updates.

Additionally, **computed signals** are:
1. ==Lazy evaluated== - computation doesn't happen until the computed signal is called

2. ==Memoized== - after the first computation, the computed value is cached, so all subsequent reads will use the cached value instead of recalculating each time. When dependent signal values change and the computed signal is read again, Angular knows the cached value is out of date and will rerun the calculation.


**Computed signals** are dynamic such that _only the signals actually read during the derivation are tracked_. 

For example, in this `computed` the `count` signal is only read if the `showCount` signal is true:

```typescript
const showCount = signal(false);
const count = signal(0);
const conditionalCount = computed(() => {
  if (showCount()) {
    return `The count is ${count()}.`;
  } else {
    return 'Nothing to see here!';
  }
});
```


If `showCount` is *false*, the `count` signal is _not_ read in the derivation, meaning even if you update `count`, it will not trigger a recalculation in the `computed`. 

If `showCount` is true, then under the conditional, `count` will be read this time, marking it as a dependent of the `computed`. The computation will be redone and cached value replaced.

# Effects

An **effect** is code that executes in response to one or more signals changing value. Similar to **computed signals**, effects keep track of their dependencies dynamically, and only track signals which were read in the most recent execution. Effects *always run at least once*. 

Effects are created with the `effect` function:

```typescript
effect(() => {
  console.log(`The current count is: ${count()}`);
});
```

> [!info]
> **Effects** are great for executing side effects in response to signal value changing. The documentation lists the following use cases:
> - Logging data being displayed and when it changes, either for analytics or as a debugging tool.
> -  Keeping data in sync with `window.localStorage`.
> - Adding custom DOM behavior that can't be expressed with template syntax.
> - Performing custom rendering to a `<canvas>`, charting library, or other third party UI library.

> [!warning] 
> It is generally recommended to use **computed** over **effects** for managing state. This is because effects can lead to the dreaded `ExpressionChangedAfterItHasBeenChecked` Angular error, as well unnecessary change detection cycles.
> 


## Why use signals over regular variables?

- ==Automatic change tracking== - because signals are reactive, only the parts of the template that *read* that signal will be re-evaluated. This increases performance because Angular doesn't have to perform **change detection** on the whole DOM.

- ==Computed states== - **computed signals** automatically update when dependent signals change. Without signals (or RxJs), a separate method would be required to update regular variable Y if regular variable X is changed.

- ==Reactive side effects== - **effect** provides an efficient way of executing any required side effects without writing extra code (no observables,  subscriptions, etc...)
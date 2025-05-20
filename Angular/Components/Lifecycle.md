A component's **lifecyle** is the sequence of steps that happen between the component's creation and its destruction. Each step represents a different part of Angular's process for rendering components and checking them for updates over time.

In components, you can implement **lifecycle hooks** to run code during these steps. A component's lifecycle if tightly connected to how Angular checks your components for changes over time. 

For the purposes of understanding this lifecycle, you only need to know that Angular walks your application tree from ==top to bottom==, checking template bindings for changes. Lifecyle hooks run while Angular is doing this traversal. This traversal visits each component exactly once, so you should always avoid making further state changes in the middle of the process.

# Summary

| **Phase**               | **Method**              | **Summary**                                                                                                                                                              |
| ----------------------- | ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Creation                | `constructor`           | [Standard JavaScript class constructor](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Classes/constructor) . Runs when Angular instantiates the component. |
| Change<br><br>Detection | `ngOnInit`              | Runs once after Angular has initialized all the component's inputs.                                                                                                      |
|                         | `ngOnChanges`           | Runs every time the component's inputs have changed.                                                                                                                     |
|                         | `ngDoCheck`             | Runs every time this component is checked for changes.                                                                                                                   |
|                         | `ngAfterContentInit`    | Runs once after the component's _content_ has been initialized.                                                                                                          |
|                         | `ngAfterContentChecked` | Runs every time this component content has been checked for changes.                                                                                                     |
|                         | `ngAfterViewInit`       | Runs once after the component's _view_ has been initialized.                                                                                                             |
|                         | `ngAfterViewChecked`    | Runs every time the component's view has been checked for changes.                                                                                                       |
| Destruction             | `ngOnDestroy`           | Runs once before the component is destroyed.                                                                                                                             |
## ngOnInit

The `ngOnInit` method runs after Angular has initialized all the components inputs with their initial values. A component's `ngOnInit` runs exactly once.

This step happens _before_ the component's own template is initialized. This means that you can update the component's state based on its initial input values.

## ngOnChanges

The `ngOnChanges` method runs after any component inputs have changed.

This step happens _before_ the component's own template is checked. This means that you can update the component's state based on its initial input values.

During initialization, the first `ngOnChanges` runs before `ngOnInit`


### Inspecting changes

The `ngOnChanges` method accepts one `SimpleChanges` argument. This object is a `Record` mapping each component input name to a `SimpleChange` object. 

Each `SimpleChange` contains the input's **previous value**, its **current value**, and a flag for whether this is the first time the input has changed.

```typescript
@Component({
  /* ... */
})
export class UserProfile {
  @Input() name: string = '';
  
  ngOnChanges(changes: SimpleChanges) {
    for (const inputName in changes) {
    
      const inputValues = changes[inputName];
      console.log(`Previous ${inputName} == ${inputValues.previousValue}`);
      console.log(`Current ${inputName} == ${inputValues.currentValue}`);
      console.log(
      `Is first ${inputName} change == ${inputValues.firstChange}`
      );
    }
  }
}
```

## ngOnDestroy

The `ngOnDestroy` method runs once just before a component is destroyed. Angular destroys a component when it is no longer shown on the page, such as being hidden by `@if` or navigating to another page.

>[!tip]
>When the user leaves the website or refreshes, this does **NOT** count as being destroyed

Often paired with [[RxJS]] to ensure subscriptions are removed when a component is destroyed, lest your application runs into performance problems.

## ngDoCheck

The `ngDoCheck` method runs before every time Angular checks a component's template for changes.

You can use this lifecycle hook to manually check for state changes outside of Angular's normal change detection, manually updating the component's state.

This method **runs very frequently** and can significantly impact your page's performance. ==Avoid defining this hook whenever possible==, only using it when you have no alternative.

During initialization, the first `ngDoCheck` runs after `ngOnInit`.

## ngAfterContentInit, ngAfterContentChecked

The `ngAfterContentInit` method runs once after all the children nested inside the component (its _content_) **have been initialized**.

The `ngAfterContentChecked` method runs every time the children nested inside the component (its _content_) **have been checked for changes**.

You can use this lifecycle hook to read the results of [[Queries#Content Queries|content queries]]. While you can access the initialized state of these queries, attempting to change any state in this method results in an [ExpressionChangedAfterItHasBeenCheckedError](https://angular.dev/errors/NG0100)

### ngAfterViewInit, ngAfterViewChecked

The `ngAfterViewInit` method runs once after all the children in the component's template (its _view_) **have been initialized

The `ngAfterViewChecked` method runs every time the children in the component's template (its _view_) **have been checked for changes**.

You can use this lifecycle hook to read the results of [[Queries#View Queries|view queries]]. While you can access the initialized state of these queries, attempting to change any state in this method results in an [ExpressionChangedAfterItHasBeenCheckedError](https://angular.dev/errors/NG0100)

# Execution order

![[hooks_execution_order.png]]
![[hooks_update_order.png]]
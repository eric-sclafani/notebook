
# Input
It is common to pass ==data== into components and Angular provides two ways to do this: `@Input()` and `input()`. Both are used to receive data from a parent component.

The former, `@Input()`,  is the traditional way.

```typescript
import { Component, Input } from '@angular/core';

// parent component
@Component({
  selector: 'app-parent',
  template: `<app-child [name]="'Alice'"></app-child>`
})
export class ParentComponent {}


// child component
@Component({
  selector: 'app-child',
  template: `<p>Hello, {{ name }}!</p>`
})
export class ChildComponent {
  @Input() name!: string;
}
```

Starting in Angular 17,  the `input()` signal was born. While the input decorator is still fine to use, input signals offer better reactivity and zoneless change detection.

>[!tip]
>See [[Signals|signals]] for more information about signal-based reactivity
>

```typescript
import { Component, input } from '@angular/core';

// parent component
@Component({
  selector: 'app-parent',
  template: `<app-child [name]="'Alice'"></app-child>`
})
export class ParentComponent {}


// child component
@Component({
  selector: 'app-child',
  template: `<p>Hello, {{ name }}!</p>`
})
export class ChildComponent {
   name = input<string>();
}
```


`input()` signals can also be made ==required==. This means that in any usage of the component, this input _must_ be provided.

```typescript
@Component({/*...*/})
export class CustomSlider {
  // Declare a required input named value
  value = input.required<number>();
}
```


## Input transforms

You can specify a `transform` function to change the value of an input

```typescript

@Component({
  selector: 'custom-slider',
  /*...*/
})
export class CustomSlider {
  label = input('', {transform: trimString});
}


function trimString(value: string | undefined): string {
  return value?.trim() ?? '';
}
```

The most common use-case for input transforms is to accept a wider range of value types in templates, often including `null` and `undefined`.

Input transform functions should always be pure functions.  Relying on state outside the transform function can lead to unpredictable behavior.

Angular includes two built-in transform functions for the two most common scenarios: coercing values to boolean and numbers.

```typescript
import {Component, input, booleanAttribute, numberAttribute} from '@angular/core';
@Component({/*...*/})
export class CustomSlider {
  disabled = input(false, {transform: booleanAttribute});
  value = input(0, {transform: numberAttribute});
}
```


## Model inputs

**Model inputs** allow for ==two way communication== between parent and child with the `model()` function. They can also be made required with `model.required()`.

```typescript
@Component({ /* ... */})
export class CustomSlider {
  value = model(0);
  increment() {
    this.value.update(oldValue => oldValue + 10);
  }
}


@Component({
  template: `<custom-slider [(value)]="volume" />`,
})
export class MediaControls {
  volume = signal(0);
}
```

In this example, the any updates from **CustomSlider** is propagated upwards to the parent component's **volume** signal. And any updates to **volume** will be pushed to the child's **value** model input.

>[!info]
When you declare a model input in a component or directive, Angular automatically creates a corresponding [[#Output|output]] for that model. The output's name is the model input's name suffixed with "Change".

### When to use model inputs

Use model inputs when you want a component to support two-way binding. This is typically appropriate when a component exists to modify a value based on user interaction. Most commonly, custom form controls, such as a date picker or combobox, should use model inputs for their primary value.

# Output

Components can output data using custom events. This is done with the `output` function:

```typescript
@Component({/*...*/})
export class ExpandablePanel {
  panelClosed = output<void>();
}
```

```html
<expandable-panel (panelClosed)="savePanelState()" />
```

The `output` function returns an `OutputEmitterRef`. You can emit an event by calling the `emit` method on the `OutputEmitterRef`:

```typescript
this.panelClosed.emit();
```

## Event data

You can pass event data when calling `emit`:
```typescript

this.valueChanged.emit(7);

this.thumbDropped.emit({
  pointerX: 123,
  pointerY: 456,
})
```

Access the data via **$event**
```html
<custom-slider (valueChanged)="logValue($event)" />
```

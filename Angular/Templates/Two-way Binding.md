**Two way binding** is a form of [[Property Binding]] and shorthand to simultaneously:

1. Bind a value into an element
2. Give that element the ability to propagate changes back through this binding

The syntax for two-way binding is a ==combination== of **square brackets** and **parentheses**, `[()]`.

>[!info]
> This syntax combines the syntax from property binding, `[]`, and the syntax from event binding, `()`. The Angular community refers to this syntax as "banana-in-a-box".

# Two-way binding form controls

Developers commonly **use two-way binding to keep component data in sync with a form control** as a user interacts with the control. 

For example, when a user fills out a text input, it should update the state in the component:

```angular-ts
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';
@Component({
  imports: [FormsModule],
  template: `
    <main>
      <h2>Hello {{ firstName }}!</h2>
      <input type="text" [(ngModel)]="firstName" />
    </main>
  `
})
export class AppComponent {
  firstName = 'Ada';
}
```

To use two-way binding with native form controls, you need to:

1. Import the `FormsModule` from `@angular/forms`
2. Use the `ngModel` directive with the two-way binding syntax (e.g., `[(ngModel)]`)
3. Assign it the state that you want it to update (e.g., `firstName`)

>[!tip]
>See [[Template-driven Forms]] to view more about `ngModel` and template-driven forms

# Two-way binding between components

Leveraging two-way binding between a parent and child component requires more configuration compared to form elements.

Here is an example where the `AppComponent` is responsible for **setting the initial count state**, but the logic for updating and rendering the UI for the counter primarily resides inside its child `CounterComponent`:

```angular-ts title="./app.component.ts"
import { Component } from '@angular/core';
import { CounterComponent } from './counter/counter.component';
@Component({
  selector: 'app-root',
  imports: [CounterComponent],
  template: `
    <main>
      <h1>Counter: {{ initialCount }}</h1>
      <app-counter [(count)]="initialCount"></app-counter>
    </main>
  `,
})
export class AppComponent {
  initialCount = 18;
}
```

```angular-ts title="./counter.component.ts"
import { Component, model } from '@angular/core';
@Component({
  selector: 'app-counter',
  template: `
    <button (click)="updateCount(-1)">-</button>
    <span>{{ count() }}</span>
    <button (click)="updateCount(+1)">+</button>
  `,
})
export class CounterComponent {
  count = model<number>(0);
  updateCount(amount: number): void {
    this.count.update(currentCount => currentCount + amount);
  }
}
```

In the code above, `model` is a [[Signals|signal]] that lets two components communicate, hence the term **two-way binding**.

The parent **AppComponent** passes in the initial value into the child **CounterComponent**, and whenever the user clicks the child's update buttons, it updates the `model` value. ==These changes are then reflected in the parent too.==


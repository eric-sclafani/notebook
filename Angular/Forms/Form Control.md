
Whether you're using [[Reactive Forms|reactive forms]] or [[Template-driven Forms|template-driven forms]], the center of all Angular forms is the `FormControl`, a class that tracks the **value and validation status** of a single form input.

It can be though of as an object that represents an individual input field. It handles:
- The **current value** of the input
- The **validation state** (valid/invalid)    
- Whether the input is **touched**, **dirty**, or **pristine**

>[!tip]
> **Touched** is ==true== the user focuses and then unfocuses the field. This lets you show validation errors only **after** a user has interacted with the field.
> 
> **Dirty** is ==true== when the user changes the **value** of the input field.
> 
> **Pristine** is ==true== when the user has _not_ changed the value of the input field (opposite of **dirty**)

The following example defines a simple form control (using reactive forms here just for example sake):

```angular-ts
import {Component} from '@angular/core';
import {FormControl, ReactiveFormsModule} from '@angular/forms';

@Component({
  selector: 'app-name-editor',
  templateUrl: './name-editor.component.html',
  styleUrls: ['./name-editor.component.css'],
  imports: [ReactiveFormsModule],
})

export class NameEditorComponent {
  name = new FormControl('');
...
}
```

```angular-html
<label for="name">Name: </label>
<input id="name" type="text" [formControl]="name">
```

### Displaying a form control value

You can display a form control value by accessing the `value` property, which provides a snapshot of the current value:

```angular-html
<p>Value: {{ name.value }}</p>
```


### Replacing a form control value

Reactive forms have methods to ==change a control's value programmatically==, which gives you the flexibility to update the value without user interaction. 

A form control instance provides a `setValue()` method that **updates the value of the form control** and **validates the structure of the value provided** against the control's structure. 

For example, when retrieving form data from a backend API or service, use the `setValue()` method to update the control to its new value, replacing the old value entirely.

```angular-ts
updateName() {
    this.name.setValue('Nancy');
  }
```

### Custom form controls

This [Angular University guide](https://blog.angular-university.io/angular-custom-form-controls/) details how to create custom form controls by implementing the **ControlValueAccessor** interface.

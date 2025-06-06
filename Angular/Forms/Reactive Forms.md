**==Reactive forms==** provide a _model-driven_ approach to handling form inputs whose values change over time. Aka, the structure and logic of the form are defined and managed entirely in the **component class (the model)**, not in the template.

# Overview

Reactive forms use an _explicit and immutable approach_ to managing the state of a form at a given point in time. ==Each change to the form state returns a new state==, which maintains the integrity of the model between changes. 

Reactive forms are built around [[Observables|observable]] streams, where **form inputs and values are provided as streams of input values**, which can be accessed synchronously. Reactive forms can also provide ==type safety==.


# Group form controls

Forms typically contain several related controls. Reactive forms provide two ways of grouping multiple related controls into a single input form.

| Form groups | Details                                                                                                                                                                                                                                                                    |
| :---------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Form group  | Defines a form with a fixed set of controls that you can manage together. Form group basics are discussed in this section. You can also [nest form groups](https://angular.dev/guide/forms/reactive-forms#creating-nested-form-groups "See") to create more complex forms. |
| Form array  | Defines a dynamic form, where you can add and remove controls at run time. You can also nest form arrays to create more complex forms. For more about this option, see [Creating dynamic forms](https://angular.dev/guide/forms/reactive-forms#creating-dynamic-forms).    |
Just as a form control instance gives you control over a single input field, a `FormGroup` instance **tracks the form state of a group of form control instances** (for example, a form).

Each control in a form group instance is ==tracked by name== when creating the form group. The following example shows how to manage multiple form control instances in a single group.

```angular-ts
import {Component} from '@angular/core';
import {FormGroup, FormControl, ReactiveFormsModule} from '@angular/forms';

@Component({
  selector: 'app-profile-editor',
  templateUrl: './profile-editor.component.html',
  styleUrls: ['./profile-editor.component.css'],
  imports: [ReactiveFormsModule],
})

export class ProfileEditorComponent {
  profileForm = new FormGroup({
    firstName: new FormControl(''),
    lastName: new FormControl(''),
...
  });
...
}
```

A form group tracks the **status** and **changes** for each of its controls, so if one of the controls changes, the ==parent control also emits a new status or value change==. The model for the group is maintained from its members. After you define the model, you must update the template to reflect the model in the view.

```angular-html
<form [formGroup]="profileForm">
  <label for="first-name">First Name: </label>
  <input id="first-name" type="text" formControlName="firstName" />
  <label for="last-name">Last Name: </label>
  <input id="last-name" type="text" formControlName="lastName" />
...
</form>
```

Just as a form group contains a group of controls, the _profileForm_ `FormGroup` is bound to the `form` element with the `FormGroup` directive, creating a **communication layer** between the model and the form containing the inputs. 

The `formControlName` input provided by the `FormControlName` directive binds each individual input to the form control defined in `FormGroup`. The form controls communicate with their respective elements. They also communicate changes to the form group instance, which provides the source of truth for the model value.

To save the form, implement the form like this:

```angular-html
<form [formGroup]="profileForm" (ngSubmit)="onSubmit()">
...
<p>Complete the form to enable button.</p>
<button type="submit" [disabled]="!profileForm.valid">Submit</button>
```


# Nested form groups

Form groups can accept both _individual form control instances_ and _other form group instances_ as children. This makes composing complex form models easier to maintain and logically group together.

When building complex forms, **managing the different areas of information is easier in smaller sections**. Using a nested form group instance lets you break large forms groups into smaller, more manageable ones.

```angular-ts
import {Component} from '@angular/core';
import {FormGroup, FormControl, ReactiveFormsModule} from '@angular/forms';

@Component({
  selector: 'app-profile-editor',
  templateUrl: './profile-editor.component.html',
  styleUrls: ['./profile-editor.component.css'],
  imports: [ReactiveFormsModule],
})

export class ProfileEditorComponent {
  profileForm = new FormGroup({
    firstName: new FormControl(''),
    lastName: new FormControl(''),
    address: new FormGroup({
      street: new FormControl(''),
      city: new FormControl(''),
      state: new FormControl(''),
      zip: new FormControl(''),
    }),
  });
...
}
```

Then, in the template, reference which inner form group you're using with the `formGroupName` directive:

```angular-html
<div formGroupName="address">
    <h2>Address</h2>
    <label for="street">Street: </label>
    <input id="street" type="text" formControlName="street" />
    <label for="city">City: </label>
    <input id="city" type="text" formControlName="city" />
    <label for="state">State: </label>
    <input id="state" type="text" formControlName="state" />
    <label for="zip">Zip Code: </label>
    <input id="zip" type="text" formControlName="zip" />
  </div>
```



# Updating parts of the model

When updating the value for a form group instance that contains multiple controls, you might only want to **update parts of the model**. This section covers how to update specific parts of a form control data model.

There are two ways to update the model value:

| Methods        | Details                                                                                                                                                               |
| :------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `setValue()`   | Set a new value for an individual control. The `setValue()` method strictly adheres to the structure of the form group and replaces the entire value for the control. |
| `patchValue()` | Replace any properties defined in the object that have changed in the form model.                                                                                     |

```angular-ts
updateProfile() {
    this.profileForm.patchValue({
      firstName: 'Nancy',
      address: {
        street: '123 Drew Street',
      },
    });
  }
```

# FormBuilder

Creating form control instances manually can become repetitive when dealing with multiple forms. The `FormBuilder` service provides convenient methods for generating controls.

```angular-ts
...
private formBuilder = inject(FormBuilder);
...

profileForm = this.formBuilder.group({
    firstName: [''],
    lastName: [''],
    address: this.formBuilder.group({
      street: [''],
      city: [''],
      state: [''],
      zip: [''],
    }),
...
  });
```

# Typed form groups

Angular lets you create strongly typed form groups so Typescript can catch errors before runtime.  Strong typing also lets intellisense detect properties on your form objects. 

Additional resource on typed reactive form groups: [typed reactive forms](https://blog.angular-university.io/angular-typed-forms/)



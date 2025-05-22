
In Angular, a **binding** creates a dynamic connection between a component's template and its data. This connection ensures that ==_changes to the component's data automatically update the rendered template_.==

You can bind dynamic text in templates with double curly braces, which tells Angular that it is responsible for the expression inside and ensuring it is updated correctly. This is called **text interpolation**.

```ts
@Component({
  template: `
    <p>Your color preference is {{ theme }}.</p>
  `,
  ...
})
export class AppComponent {
  theme = 'dark';
}
```

# Native element properties

In Angular, you use property bindings to set values **directly to the DOM representation of the element.**

```ts
@Component({
	...
})
export class AppComponent {
	isFormValid:boolean = false;
}
```

```html
<button [disabled]="isFormValid">Save</button>
```

# Component & directive properties

When an element is an Angular component, you can use property bindings to set component input properties using the same square bracket syntax.

```html
<my-listbox [value]="mySelection" />
```

You can bind to directive properties as well:
```html
<!-- Bind to the `ngSrc` property of the `NgOptimizedImage` directive  -->
<img [ngSrc]="profilePhotoUrl" alt="The current user's profile photo">
```

>[!tip]
>See [[Image Optimization|image optimization]] for how `[ngSrc]` is used

# Attributes

When you need to set HTML attributes that **do not have corresponding DOM properties**, such as ==ARIA== attributes or ==SVG== attributes, you can bind attributes to elements in your template with the `attr.` prefix.

```html
<ul [attr.role]="listRole">
```

You can also use **text interpolation** syntax in ==properties== and ==attributes== by using the double curly brace syntax instead of square braces around the property or attribute name. When using this syntax, Angular treats the assignment as a property binding.

```html
<img src="profile-photo.jpg" alt="Profile photo of {{ firstName }}" >

<button attr.aria-label="Save changes to {{ objectType }}">
```

# CSS classes

You can create a CSS class binding to conditionally add or remove a CSS class on an element based on whether the bound value is true or false.

```html
<!-- When `isExpanded` is true, add the `expanded` CSS class. -->
<ul [class.expanded]="isExpanded">
```

You can also bind directly to the `class` property. Angular accepts three types of value:

| Description of `class` value                                                                                                                                      | Typescript type      |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------- |
| A string containing one or more CSS classes separated by spaces                                                                                                   | `string`             |
| An array of CSS class strings                                                                                                                                     | `string[]`           |
| An object where each property name is a CSS class name and each corresponding value determines whether that class is applied to the element, based on truthiness. | `Record<string,any>` |

```ts
@Component({
  template: `
    <ul [class]="listClasses"> ... </ul>
    <section [class]="sectionClasses"> ... </section>
    <button [class]="buttonClasses"> ... </button>
  `,
  ...
})
export class UserProfile {
  listClasses = 'full-width outlined';
  sectionClasses = ['expandable', 'elevated'];
  buttonClasses = {
    highlighted: true,
    embiggened: false,
  };
}
```

When using static CSS classes, directly binding `class`, and binding specific classes, Angular intelligently combines all of the classes in the rendered result. 

In other words, you can combine the power of dynamic class/style binding with static classes/styles.

# CSS style properties

You can also bind to CSS style properties directly on an element.

```html
<!-- Set the CSS `display` property based on the `isExpanded` property. -->
<section [style.display]="isExpanded ? 'block' : 'none'">
```

You can also set multiple style values in one binding, similar to how it is done with classes:

```ts
@Component({
  template: `
    <ul [style]="listStyles"> ... </ul>
    <section [style]="sectionStyles"> ... </section>
  `,
  ...
})
export class UserProfile {
  listStyles = 'display: flex; padding: 8px';
  sectionStyles = {
    border: '1px solid black',
    'font-weight': 'bold',
  };
}
```

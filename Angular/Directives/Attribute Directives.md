
**Attribute directives** let you change the appearance or behavior of DOM elements and Angular components. 


# Built-in directives


| Common Directives | Details                                           |
| ----------------- | ------------------------------------------------- |
| NgClass           | Adds and removes a set of CSS classes             |
| NgStyle           | Adds and removes a set of HTML styles             |
| NgModel           | Adds two-way data binding to an HTML form element |
### NgClass

On the element you'd like to style, add `[ngClass]` and set it equal to an expression.

In the following code, `isSpecial` is a boolean set to `true` in `app.component.ts`. Because `isSpecial` is true, `ngClass` applies the class of `special` to the `<div>`.

```angular-html
<div [ngClass]="isSpecial ? 'special' : ''">This div is special</div>
```


You can also set `[ngClass]` equal to a component method to dynamically set classes:

```angular-ts
currentClasses: Record<string, boolean> = {};
...
  setCurrentClasses() {
    // CSS classes: added/removed per current state of component properties
    this.currentClasses = {
      saveable: this.canSave,
      modified: !this.isUnchanged,
      special: this.isSpecial,
    };
  }
```

```angular-html
<div [ngClass]="currentClasses">This div is initially saveable, unchanged, and special.</div>
```


### NgStyle

Use `NgStyle` to set multiple inline styles simultaneously, based on the state of the component.

```angular-ts
currentStyles: Record<string, string> = {};
...
  setCurrentStyles() {
    // CSS styles: set per current state of component properties
    this.currentStyles = {
      'font-style': this.canSave ? 'italic' : 'normal',
      'font-weight': !this.isUnchanged ? 'bold' : 'normal',
      'font-size': this.isSpecial ? '24px' : '12px',
    };
  }
```

```angular-html
<div [ngStyle]="currentStyles">
  This div is initially italic, normal weight, and extra large (24px).
</div>
```


### NgModel

Use the `NgModel` directive to display a data property and update that property when the user makes changes.

```angular-html
<label for="example-ngModel">[(ngModel)]:</label>
<input [(ngModel)]="currentItem.name" id="example-ngModel">
```

To customize your configuration, write the expanded form, which separates the property and event binding. Use [[Property Binding|property binding]] to set the property and [[Event Listeners|event binding]] to respond to changes. The following example changes the `<input>` value to uppercase:

```angular-html
<input 
	[ngModel]="currentItem.name" 
	(ngModelChange)="setUppercaseName($event)" 
	id="example-uppercase">
```

>[!info]
>See [[Template-driven Forms]] for more information on how to use `[ngModel]`

# Custom directives

Angular provides a way to create custom directives to suit your needs.

```angular-ts
import {Directive, ElementRef, inject} from '@angular/core';
@Directive({
  selector: '[appHighlight]',
})
export class HighlightDirective {
  private el = inject(ElementRef);
  constructor() {
    this.el.nativeElement.style.backgroundColor = 'yellow';
  }
}
```

In the code above, the `ElementRef` is **injected** into the directive because it ==grants direct access to the host DOM element== making use of the directive.

Adding some interactivity, you can bind `@HostListeners` to let the directive detect events:

```angular-ts
  @HostListener('mouseenter') onMouseEnter() {
    this.highlight('yellow');
  }
  
  @HostListener('mouseleave') onMouseLeave() {
    this.highlight('');
  }
  
  private highlight(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }
```

### Passing values into directives

To let your directive accept inputs, declare `input` signals to do so:

```angular-ts
highlight = input('');
```

And its usage:

```angular-html
<p [appHighlight]="color">Highlight me!</p>
```






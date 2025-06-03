**Structural directives** are responsible for HTML layout. T==hey shape or reshape the DOM's structure==, typically by adding, removing, and manipulating the host elements to which they are attached.

Structural directives are directives applied to an [[Template Fragments|<ng-template>]] element that conditionally or repeatedly render the content of that `<ng-template>`.

However, to avoid writing tons of boilerplate `<ng-template>` elements, Angular provided a very useful shorthand: **prefixing the directive witch asterisk(`*`)**. Angular ==automatically transforms== the asterisk in front of a structural directive into an `<ng-template>`.

For example, compare the following two statements which do the same thing:

```angular-html
<!-- Shorthand syntax: -->
<p class="data-view" *select="let data from source">The data is: {{data}}</p>

<!-- Long-form syntax: -->
<ng-template select let-data [selectFrom]="source">
  <p class="data-view">The data is: {{data}}</p>
</ng-template>
```

## One structural directive per element

You can only apply one structural directive per element **when using the shorthand syntax**. This is because there is only one `<ng-template>` element onto which that directive gets unwrapped. Multiple directives would require multiple nested `<ng-template>`, and it's unclear which directive should be first. 

`<ng-container>` can be used when to create wrapper layers when multiple structural directives need to be applied around the same physical DOM element or component, which allows the user to define the nested structure.


# Built-in directives

| Common built-in structural directives                                                  | Details                                                          |
| :------------------------------------------------------------------------------------- | :--------------------------------------------------------------- |
| [`NgIf`](https://angular.dev/guide/directives#adding-or-removing-an-element-with-ngif) | Conditionally creates or disposes of subviews from the template. |
| [`NgFor`](https://angular.dev/guide/directives#listing-items-with-ngfor)               | Repeat a node for each item in a list.                           |
| [`NgSwitch`](https://angular.dev/guide/directives#switching-cases-with-ngswitch)       | A set of directives that switch among alternative views.         |
>[!tip]
>The above structural directives are the older way of running conditions and applying loops. See [[Control Flow]] to see more about the newer directive-less syntax.


### Add/remove elements with NgIf

Add or remove an element by applying an `NgIf` directive to a host element.

When `NgIf` is `false`, Angular removes an element and its descendants from the DOM. Angular then disposes of their components, which frees up memory and resources.

```angular-html
<app-item-detail *ngIf="isActive" [item]="item"></app-item-detail>
```


### Listing items with NgFor

Use the `NgFor` directive to present a list of items.

```angular-html
<div *ngFor="let item of items">{{ item.name }}</div>
```

The following example references `item` first in an interpolation and then passes in a binding to the `item` property of the `<app-item-detail>` component.

```angular-html
<div *ngFor="let item of items">{{ item.name }}</div>
...
  <app-item-detail *ngFor="let item of items" [item]="item"></app-item-detail>
```


### Switching cases with NgSwitch

Like the JavaScript `switch` statement, `NgSwitch` displays one element from among several possible elements, based on a switch condition. Angular puts only the selected element into the DOM.

`NgSwitch` is a set of three directives:

| `NgSwitch` directives | Details                                                                                                                                                                |
| :-------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `NgSwitch`            | An attribute directive that changes the behavior of its companion directives.                                                                                          |
| `NgSwitchCase`        | Structural directive that adds its element to the DOM when its bound value equals the switch value and removes its bound value when it doesn't equal the switch value. |
| `NgSwitchDefault`     | Structural directive that adds its element to the DOM when there is no selected `NgSwitchCase`.                                                                        |
```angular-html
<div [ngSwitch]="currentItem.feature">
  <app-stout-item *ngSwitchCase="'stout'" [item]="currentItem"></app-stout-item>
  <app-device-item *ngSwitchCase="'slim'" [item]="currentItem"></app-device-item>
  <app-lost-item *ngSwitchCase="'vintage'" [item]="currentItem"></app-lost-item>
  <app-best-item *ngSwitchCase="'bright'" [item]="currentItem"></app-best-item>
...
  <app-unknown-item *ngSwitchDefault [item]="currentItem"></app-unknown-item>
</div>
```


# Custom directives

In this section, we will create a structural directive which fetches data from a given data source and renders its template when that data is available. This directive is called `SelectDirective`, after the SQL keyword `SELECT`, and match it with an attribute selector `[select]`.


1. Inject `TemplateRef` and `ViewContainerRef` into the directive as private properties.
2. Add a `selectFrom` input property.
3. Add the business logic.
``` angular-ts
@Directive({  
	selector: '[select]',
})
export class SelectDirective {  
	private templateRef = inject(TemplateRef);  
	private viewContainerRef = inject(ViewContainerRef);

	@Input({required:true}) selectFrom!:DataSource;
	
	async ngOnInit() {
	    const data = await this.selectFrom.load();
	    this.viewContainerRef.createEmbeddedView(this.templateRef, {
	      // Create the embedded view with a context object that contains
	      // the data via the key `$implicit`.
	      $implicit: data,
	    });
	  }
}
```


To see how Angular interprets the syntax and translates the shorthand, see [this link](https://angular.dev/guide/directives/structural-directives#structural-directive-syntax-reference)

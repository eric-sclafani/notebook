
Inspired by the [native `<template>` element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/template), the `<ng-template>` element lets you declare a **template fragment** – a section of content that you can dynamically or programmatically render.

You can create a template fragment inside of any component template with the `<ng-template>` element:

```angular-html
<p>This is a normal element</p>
<ng-template>
  <p>This is a template fragment</p>
</ng-template>
```

When the above is rendered, the content of the `<ng-template>` element is not rendered on the page. Instead, you can get a **reference to the template fragment** and write code to **dynamically render it**.

# Referencing a template fragment

You can add a ==template reference variable== to an `<ng-template>` element to reference that template fragment in other parts of the same template file:

```angular-html
<p>This is a normal element</p>
<ng-template #myFragment>
  <p>This is a template fragment</p>
</ng-template>
```

You can then query that template fragment using a [[Queries#View Queries|view query]]. 

If a template contains multiple fragments, you can assign a name to each fragment by adding a template reference variable to each `<ng-template>` element and querying for the fragments based on that name:

```angular-ts
@Component({
  /* ... */,
  template: `
    <p>This is a normal element</p>
    <ng-template #fragmentOne>
      <p>This is one template fragment</p>
    </ng-template>
    <ng-template #fragmentTwo>
      <p>This is another template fragment</p>
    </ng-template>
  `,
})
export class ComponentWithFragment {
  @ViewChild('fragmentOne', {read: TemplateRef}) fragmentOne: TemplateRef<unknown> | undefined;
  @ViewChild('fragmentTwo', {read: TemplateRef}) fragmentTwo: TemplateRef<unknown> | undefined;
}
```

>[!info]
>Angular documentation uses `@ViewChild`, but the `viewChild` signal should work too
>

### Injecting a template fragment

A directive can inject a `TemplateRef` if that directive is applied directly to an `<ng-template>` element:

```ts
@Directive({
  selector: '[myDirective]'
})
export class MyDirective {
  private fragment = inject(TemplateRef);
}
```

```angular-html
<ng-template myDirective>
  <p>This is one template fragment</p>
</ng-template>
```

# Template fragment rendering

Once you have a reference to a template fragment's `TemplateRef` object, you can render a fragment in one of two ways: in your template with the `NgTemplateOutlet` directive or in your TypeScript code with `ViewContainerRef`.

### NgTemplateOutlet

The `NgTemplateOutlet` directive from `@angular/common` accepts a `TemplateRef` and renders the fragment as a **sibling** to the element with the outlet. 

You should generally use `NgTemplateOutlet` on an [[Ng Container|<ng-container>]] element because `<ng-container>` does not render anything to the DOM by default.

```angular-html
<p>This is a normal element</p>
<ng-template #myFragment>
  <p>This is a fragment</p>
</ng-template>
<ng-container *ngTemplateOutlet="myFragment"></ng-container>
```

### ViewContainerRef

A **view container** is a node in Angular's component tree that can contain content. Any component or directive can inject `ViewContainerRef` to get a reference to a view container corresponding to that component or directive's location in the DOM.

You can use the `createEmbeddedView` method on `ViewContainerRef` to dynamically render a template fragment. 

When you render a fragment with a `ViewContainerRef`, Angular appends it into the DOM as the **next sibling of the component or directive that injected** the `ViewContainerRef`.

```angular-ts
@Component({
  /* ... */,
  selector: 'component-with-fragment',
  template: `
    <h2>Component with a fragment</h2>
    <ng-template #myFragment>
      <p>This is the fragment</p>
    </ng-template>
    <my-outlet [fragment]="myFragment" />
  `,
})
export class ComponentWithFragment { }

@Component({
  /* ... */,
  selector: 'my-outlet',
  template: `<button (click)="showFragment()">Show</button>`,
})
export class MyOutlet {
  private viewContainer = inject(ViewContainerRef);
  @Input() fragment: TemplateRef<unknown> | undefined;
  showFragment() {
    if (this.fragment) {
      this.viewContainer.createEmbeddedView(this.fragment);
    }
  }
}
```

In the example above, clicking the "Show" button results in the following output:

```angular-html
<component-with-fragment>
  <h2>Component with a fragment>
  <my-outlet>
    <button>Show</button>
  </my-outlet>
  <p>This is the fragment</p>
</component-with-fragment>
```

# Passing parameters when rendering a template fragment

When **declaring a template fragment** with `<ng-template>`, you can additionally declare parameters accepted by the fragment. 

When you **render a fragment**, you can optionally pass a `context` ==object corresponding to these parameters==. You can use data from this context object in binding expressions and statements, in addition to referencing data from the component in which the fragment is declared.


### Using NgTemplateOutlet

```angular-html
<ng-template #myFragment let-pizzaTopping="topping">
  <p>You selected: {{pizzaTopping}}</p>
</ng-template>

<ng-container
  [ngTemplateOutlet]="myFragment"
  [ngTemplateOutletContext]="{topping: 'onion'}"
/>
```

### Using ViewContainerRef

```ts
this.viewContainer.createEmbeddedView(this.myFragment, {topping: 'onion'});
```



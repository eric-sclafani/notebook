
In addition to rendering a component directly in the template, you can also **dynamically** render components. 

There are two main ways to dynamically render a component: in a template with `NgComponentOutlet`, or in your TypeScript code with `ViewContainerRef`.

# NgComponentOutlet


`NgComponentOutlet` is a structural directive that dynamically renders a given component in a template.

```ts
@Component({ ... })
export class AdminBio { /* ... */ }

@Component({ ... })
export class StandardBio { /* ... */ }

@Component({
  ...,
  template: `
    <p>Profile for {{user.name}}</p>
    <ng-container *ngComponentOutlet="getBioComponent()" /> `
})
export class CustomDialog {
  @Input() user: User;
  getBioComponent() {
    return this.user.isAdmin ? AdminBio : StandardBio;
  }
}
```

# ViewContainerRef

A **view container** is a node in Angular's component tree that can contain content. Any component or directive can inject `ViewContainerRef` to get a reference to a view container corresponding to that component or directive's location in the DOM.

You can use the `createComponent`method on `ViewContainerRef` to dynamically create and render a component. 

When you create a component with a `ViewContainerRef`, **Angular appends it to the DOM as the next sibling** of the component or directive that injected the `ViewContainerRef`.

```ts
@Component({
  selector: 'leaf-content',
  template: `
    This is the leaf content
  `,
})
export class LeafContent {}

@Component({
  selector: 'outer-container',
  template: `
    <p>This is the start of the outer container</p>
    <inner-item />
    <p>This is the end of the outer container</p>
  `,
})
export class OuterContainer {}

@Component({
  selector: 'inner-item',
  template: `
    <button (click)="loadContent()">Load content</button>
  `,
})
export class InnerItem {
  private viewContainer = inject(ViewContainerRef);
  loadContent() {
    this.viewContainer.createComponent(LeafContent);
  }
}
```

The code above renders the following DOM structure:
```html
<outer-container>
  <p>This is the start of the outer container</p>
  <inner-item>
    <button>Load content</button>
  </inner-item>
  <leaf-content>This is the leaf content</leaf-content>
  <p>This is the end of the outer container</p>
</outer-container>
```
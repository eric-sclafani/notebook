Often times, components need to act a containers for different types of content.

This is where `<ng-content>` comes in. Inside a component's template, `<ng-content>` is used as a placeholder to mark where content should go.

In the following example, any HTML passed into the **custom-card** component will be rendered where the `<ng-content>` is:

```ts
@Component({
  selector: 'custom-card',
  template: '<div class="card-shadow"> <ng-content></ng-content> </div>',
})
export class CustomCard {/* ... */}
```

```html
<!-- Using the component -->
<custom-card>
  <p>This is the projected content</p>
</custom-card>
```

>[!tip]
>`<ng-content>` works similarly to [the native `<slot>` element](https://developer.mozilla.org/docs/Web/HTML/Element/slot), but with some Angular-specific functionality.

All children passed into a component via `<ng-content>` is considered that component's **content**. This is distinguished from the component's **view**, which is the elements defined in the component's template. 

>[!info]
>See [[Queries#Content Queries|content queries]] for how to programmatically interact with a component's **content**

**The `<ng-content>` element is neither a component nor DOM element**. Instead, it is a special placeholder that tells Angular where to render content. Angular's compiler processes all `<ng-content>` elements at build-time. 

You cannot insert, remove, or modify `<ng-content>` at run time. You cannot add directives, styles, or arbitrary attributes to `<ng-content>`

# Multiple content placeholders

Angular supports projecting multiple different elements into different `<ng-content>` placeholders based on CSS selector. 

Expanding the card example from above, you could create two placeholders for a card title and a card body by using the `select` attribute:

```html
<!-- Component template -->
<div class="card-shadow">
  <ng-content select="card-title"></ng-content>
  <div class="card-divider"></div>
  <ng-content select="card-body"></ng-content>
</div>
```

```html
<!-- Using the component -->
<custom-card>
  <card-title>Hello</card-title>
  <card-body>Welcome to the example</card-body>
</custom-card>
```

# Fallback Content

Angular can show _fallback content_ for a component's `<ng-content>` placeholder if that component doesn't have any matching child content. 

You can specify fallback content by adding child content to the `<ng-content>` element itself.

```html
<!-- Component template -->
<div class="card-shadow">
  <ng-content select="card-title">Default Title</ng-content>
  <div class="card-divider"></div>
  <ng-content select="card-body">Default Body</ng-content>
</div>
```

```html
<!-- Using the component -->
<custom-card>
  <card-title>Hello</card-title>
  <!-- No card-body provided -->
</custom-card>
```

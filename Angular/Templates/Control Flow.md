
Angular templates support control flow blocks that let you **conditionally** show, hide, and repeat elements.

>[!info]
>This was previously accomplished with the _ngIf, ngFor, and ngSwitch_ directives. While these are still viable, it is recommended to use the newer control flow techniques


# Conditional displays with @if

The `@if` block conditionally displays its content when its condition expression is true:

```angular-html
@if (a > b) {
  <p>{{a}} is greater than {{b}}</p>
}

@if (a > b) {
  {{a}} is greater than {{b}}
} @else if (b > a) {
  {{a}} is less than {{b}}
} @else {
  {{a}} is equal to {{b}}
}
```

### Referencing the conditional's result

The `@if` conditional supports saving the result of the conditional expression into a variable for reuse inside of the block.

```angular-html
@if (user.profile.settings.startDate; as startDate) {
  {{ startDate }}
}
```

This can be useful for referencing longer expressions that would be easier to read and maintain within the template.

# Conditional displays with @switch

While the `@if` block is great for most scenarios, the `@switch` block provides an alternate syntax to conditionally render data. Its syntax closely resembles JavaScript's `switch` statement.

```angular-html
@switch (userPermissions) {
  @case ('admin') {
    <app-admin-dashboard />
  }
  @case ('reviewer') {
    <app-reviewer-dashboard />
  }
  @case ('editor') {
    <app-editor-dashboard />
  }
  @default {
    <app-viewer-dashboard />
  }
}
```


# Repeating content

The `@for` block loops through a collection and repeatedly renders the content of a block. The collection can be any JavaScript [iterable](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Iteration_protocols), but Angular has additional performance optimizations for `Array` values.

```angular-html
@for (item of items; track item.id) {
  {{ item.name }}
}
```

## Track

The `track` expressions allows Angular to maintain a relationship between your data and the DOM notes on the page. The allows Angular to **optimize** performance by executing the minimum necessary DOM operations when the data changes.

Using track effectively can **significantly improve your application's rendering performance** when looping over data collections.

Select a property that ==uniquely identifies each item== in the `track` expression. If your data model includes a uniquely identifying property, commonly `id` or `uuid`, use this value. If your data does not include a field like this, strongly consider adding one.

For static collections that never change, you can use `$index` to tell Angular to track each item by its index in the collection.

### Contextual variables

Inside `@for` blocks, several implicit variables are always available:

| Variable | Meaning                                       |
| -------- | --------------------------------------------- |
| `$count` | Number of items in a collection iterated over |
| `$index` | Index of the current row                      |
| `$first` | Whether the current row is the first row      |
| `$last`  | Whether the current row is the last row       |
| `$even`  | Whether the current row index is even         |
| `$odd`   | Whether the current row index is odd          |

These variables are always available with these names, but can be aliased via a `let` segment:

```angular-html
@for (item of items; track item.id; let idx = $index, e = $even) {
  <p>Item #{{ idx }}: {{ item.name }}</p>
}
```

The aliasing is useful when nesting `@for` blocks, letting you read variables from the outer `@for` block from an inner `@for` block.

# Fallback value

You can optionally include an `@empty` section immediately after the `@for` block content. The content of the `@empty` block displays when there are no items:

```angular-html
@for (item of items; track item.name) {
  <li> {{ item.name }}</li>
} @empty {
  <li aria-hidden="true"> There are no items. </li>
}
```


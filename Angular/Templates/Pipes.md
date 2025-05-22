**Pipes** are a special operator in Angular template expressions that allows you to ==transform data declaratively in your template==. 

Pipes let you declare a transformation function once and then use that transformation across multiple templates. Angular pipes use the vertical bar character (`|`).

```angular-html
<main>
	<h1>Purchases from {{ company | titlecase }} on {{ purchasedOn | date }}</h1>
	<p>Total: {{ amount | currency }}</p>
</main>
```


# Built-in pipes

The following is a list of all built-in pipes in Angular:

|Name|Description|
|---|---|
|[`AsyncPipe`](https://angular.dev/api/common/AsyncPipe)|Read the value from a `Promise` or an RxJS `Observable`.|
|[`CurrencyPipe`](https://angular.dev/api/common/CurrencyPipe)|Transforms a number to a currency string, formatted according to locale rules.|
|[`DatePipe`](https://angular.dev/api/common/DatePipe)|Formats a `Date` value according to locale rules.|
|[`DecimalPipe`](https://angular.dev/api/common/DecimalPipe)|Transforms a number into a string with a decimal point, formatted according to locale rules.|
|[`I18nPluralPipe`](https://angular.dev/api/common/I18nPluralPipe)|Maps a value to a string that pluralizes the value according to locale rules.|
|[`I18nSelectPipe`](https://angular.dev/api/common/I18nSelectPipe)|Maps a key to a custom selector that returns a desired value.|
|[`JsonPipe`](https://angular.dev/api/common/JsonPipe)|Transforms an object to a string representation via `JSON.stringify`, intended for debugging.|
|[`KeyValuePipe`](https://angular.dev/api/common/KeyValuePipe)|Transforms Object or Map into an array of key value pairs.|
|[`LowerCasePipe`](https://angular.dev/api/common/LowerCasePipe)|Transforms text to all lower case.|
|[`PercentPipe`](https://angular.dev/api/common/PercentPipe)|Transforms a number to a percentage string, formatted according to locale rules.|
|[`SlicePipe`](https://angular.dev/api/common/SlicePipe)|Creates a new Array or String containing a subset (slice) of the elements.|
|[`TitleCasePipe`](https://angular.dev/api/common/TitleCasePipe)|Transforms text to title case.|
|[`UpperCasePipe`](https://angular.dev/api/common/UpperCasePipe)|Transforms text to all upper case.|
# Using pipes

Angular's pipe operator uses the vertical bar character (`|`), within a template expression. The pipe operator is a **binary** operator– the l==eft-hand operand is the value passed== to the transformation function, and the ==right side operand is the name of the pipe and any additional arguments==.

### Multiple pipes

You can also apply multiple transformations to a value by using **multiple pipe operators**. Angular runs the pipes from left to right.

```angular-html
<p>The event will occur on {{ scheduledOn | date | uppercase }}.</p>
```


### Pipe parameters

Some pipes accept parameters to configure the transformation. To specify a parameter, append the pipe name with a colon (`:`) followed by the parameter value.

For example, the `DatePipe` is able to take parameters to format the date in a specific way:

```angular-html
<p>The event will occur at {{ scheduledOn | date:'hh:mm' }}.</p>
```

Some pipes may accept multiple parameters. You can specify additional parameter values separated by the colon character (`:`).

For example, we can also pass a second optional parameter to control the timezone.

```angular-html
<p>The event will occur at {{ scheduledOn | date:'hh:mm':'UTC' }}.</p>
```

# How pipes work

**Pipes** are straight-forward: they are functions that accept an input value and return a transformed value.

The pipe operator has **lower precedence** than other **binary operators**, including `+`, `-`, `*`, `/`, `%`, `&&`, `||`, and `??`:

```angular-html
<!-- firstName and lastName are concatenated before the result is passed to the uppercase pipe -->
{{ firstName + lastName | uppercase }}
```

The pipe operator has **higher precedence** than the **conditional (ternary) operator**. So always use parentheses in your expressions when operator precedence may be ambiguous.

```angular-html
{{ (isAdmin ? 'Access granted' : 'Access denied') | uppercase }}
```

### Change detection

By default, all pipes are considered `pure`, which means that it only executes when a primitive input value (such as a `String`, `Number`, `Boolean`, or `Symbol`) or a object reference (such as `Array`, `Object`, `Function`, or `Date`) is changed. 

Pure pipes offer a ==performance advantage== because Angular can _avoid calling the transformation function_ if the passed value has not changed.

# Custom pipes

You can define a custom pipe by implementing a TypeScript class with the `@Pipe` decorator. A pipe must have two things:

- A name, specified in the pipe decorator
- A method named `transform` that performs the value transformation.

The TypeScript class should additionally implement the `PipeTransform` interface to ensure that it satisfies the type signature for a pipe.

The naming convention for custom pipes consists of two conventions:

- `name` - camelCase is recommended. Do not use hyphens.
- `class name` - PascalCase version of the `name` with `Pipe` appended to the end

The `@Pipe` decorator requires a `name` that controls how the pipe is used in a template.

```ts title="kebab-case.pipe.ts"
import { Pipe, PipeTransform } from '@angular/core';
@Pipe({
  name: 'kebabCase',
})
export class KebabCasePipe implements PipeTransform {
  transform(value: string): string {
    return value.toLowerCase().replace(/ /g, '-');
  }
}
```

The first parameter of the `transform` method should always be the **value** being transformed. Any additional parameters can be added to modify how the transformation works:

```ts
import { Pipe, PipeTransform } from '@angular/core';
@Pipe({
  name: 'myCustomTransformation',
})
export class MyCustomTransformationPipe implements PipeTransform {
  transform(value: string, format: string): string {
    let msg = `My custom transformation of ${value}.`
    if (format === 'uppercase') {
      return msg.toUpperCase()
    } else {
      return msg
    }
  }
}
```

### Detecting changes within arrays or objects

By default, since `pipes` are **pure**, they do not detect changes within arrays or objects, only if the entire array of object is changed.

This behavior can be modified by setting the `pure` flag to **false**. Avoid creating impure pipes ==unless absolutely necessary==, as they can incur a significant performance penalty if used without care:

```ts
import { Pipe, PipeTransform } from '@angular/core';
@Pipe({
  name: 'joinNamesImpure',
  pure: false,
})
export class JoinNamesImpurePipe implements PipeTransform {
  transform(names: string[]): string {
    return names.join();
  }
}
```

Angular developers often adopt the convention of including `Impure` in the pipe `name` and class name to indicate the potential performance pitfall to other developers.
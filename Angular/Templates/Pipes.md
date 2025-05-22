**Pipes** are a special operator in Angular template expressions that allows you to ==transform data declaratively in your template==. 

Pipes let you declare a transformation function once and then use that transformation across multiple templates. Angular pipes use the vertical bar character (`|`).

```angular-html
<main>
	<h1>Purchases from {{ company | titlecase }} on {{ purchasedOn | date }}</h1>
	<p>Total: {{ amount | currency }}</p>
</main>
```


# Built-in pipes


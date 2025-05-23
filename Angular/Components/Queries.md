
Developers most commonly use queries to **retrieve references** to child components, directives, DOM elements, and more.

All query functions return [[Signals|signals]] that reflect the most up-to-date results. You can read the result by calling the signal, including in reactive contexts like `computed` and `effect`.

# View Queries


**View queries** retrieve results from the elements in the component's _view_ — the elements defined in the component's own template. You can query for a single result with the `viewChild` function.

To query an element from the template, we need to assign it a [[Template References|template reference]], using the # syntax:

```angular-ts
@Component({
    selector: "book",
    template: `
    <div>
      <b #title>Title</b>
    </div>
  `,
})
class BookComponent {

    title = viewChild<ElementRef>("title");

    constructor() {
        effect(() => {
            console.log("Title: ", 
            this.title()?.nativeElement);
        });
    }
}
```

The `viewChild` signal query returns a signal whose emitted value _is_ the queried element itself wrapped in an `ElementRef` object.

Aside from plain HTML elements, we can also query for **component instances**

```angular-ts
@Component({
  selector: 'custom-card-header',
  /*...*/
})
export class CustomCardHeader {
  text: string;
}
@Component({
  selector: 'custom-card',
  template: '<custom-card-header>Visit sunny California!</custom-card-header>',
})
export class CustomCard {
  header = viewChild(CustomCardHeader);
  headerText = computed(() => this.header()?.text);
}
```

We can change the default behavior off `viewChild` by providing it a **read** configuration. In the following code, the signal is being told to **return the ElementRef** to access the HTML, instead of returning the **component instance**.

```angular-ts
@Component({
  template: `
    <div>
      <book #book></book>
    </div>
  `,
})
class BookListComponent {
  bookComponent = viewChild("book", {
    read: ElementRef
  });
}
```

Additionally, if the queried element has multiple directives on it, the `read` configuration lets you target a specific one:

```angular-ts
@Component({
    template: `
    <div>
      <book #book 
       matTooltip="I'm a tooltip!"> 
      </book>
    </div>
  `,
    imports: [
        MatTooltip
    ]
})
class BookListComponent {
    bookComponent = viewChild("book", {
        read: MatTooltip
    });

    constructor() {
        effect(() => {
            console.log("Tooltip: ", 
            this.bookComponent()?.message);
        });
    }
}
```

View queries can also be made **required**. If the view query fails to find a matching element, Angular will throw an error.

You can also query for multiple results with the `viewChildren` function. All of the above also applies to this signal too.


### @ViewChild, @ViewChildren

The decorator versions are the older way to create these queries, but is still viable. Instead of using an `effect` to retrieve the result, you would use [[Lifecycle#ngAfterViewInit, ngAfterViewChecked|ngAfterViewInit]].



# Content Queries

**Content queries** retrieve results from the elements in the component's _content_— the elements projected into the component in the template via [[Content Projection|<ng-content>]]. 

You can query for a single result with the `contentChild` function.

```angular-ts
@Component({
  selector: "book",
  template: `
    <div class="book">
      <div class="features-container">
        <ng-content></ng-content>
      </div>
    </div>
  `,
})
class BookComponent {

  feature = contentChild("feature");

  constructor() {
    effect(() => {
      console.log("Feature: ", 
      this.feature());
    });
  }

}
```

Like with view queries, **content queries** can also be made required. You can query for multiple element using the `contentChildren` function as well.


### @ContentChild, @ContentChildren

The decorator versions are the older way to create these queries, but is still viable. Instead of using an `effect` to retrieve the result, you would use [[Lifecycle#ngAfterContentInit, ngAfterContentChecked|ngAfterContentInit]].
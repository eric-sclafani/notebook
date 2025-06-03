The `RouterOutlet` directive is a placeholder that marks the location where the router should render the component for the current URL.

```angular-html
<app-header />
<router-outlet />  <!-- Angular inserts your route content here -->
<app-footer />
```

```angular-ts
import { Component } from '@angular/core';
import { RouterOutlet } from '@angular/router';
@Component({
  selector: 'app-root',
  imports: [RouterOutlet],
  templateUrl: './app.component.html',
  styleUrl: './app.component.css'
})
export class AppComponent {}
```

In this example, if an application has the following routes defined:

```angular-ts
import { Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { ProductsComponent } from './products/products.component';
const routes: Routes = [
  {
    path: '',
    component: HomeComponent,
    title: 'Home Page'
  },
  {
    path: 'products',
    component: ProductsComponent,
    title: 'Our Products'
  }
];
```

When a user visits `/products`, Angular renders the following:

```angular-html
<app-header></app-header>
<app-products></app-products>
<app-footer></app-footer>
```


# Secondary routes with named outlets

==Pages may have multiple outlets ==— you can assign a name to each outlet to specify which content belongs to which outlet.

```angular-html
<app-header />
<router-outlet />
<router-outlet name='read-more' />
<router-outlet name='additional-actions' />
<app-footer />
```

Each outlet must have a unique name. The name cannot be set or changed dynamically. By default, the name is `'primary'`.

Angular matches the outlet's name to the `outlet` property defined on each route:

```angular-ts
{
  path: 'user/:id',
  component: UserDetails,
  outlet: 'additional-actions'
}
```

# Router lifecycle events

The Angular router is able to emit events during its lifecycle. There are a lot of possible events to listen to, go [here](https://dev.to/elviskim18/angular-router-events-lifecycle-4l7g) to see how to utilize them.


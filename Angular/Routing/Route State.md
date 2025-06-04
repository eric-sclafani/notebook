Angular Router allows you to read and use information associated with a route to create responsive and context-aware components.

# ActivatedRoute

`ActivatedRoute` is a service from `@angular/router` that provides all the information associated with the current route.

```angular-ts
import { Component } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
@Component({
  selector: 'app-product',
})
export class ProductComponent {
  private activatedRoute = inject(ActivatedRoute);
  constructor() {
    console.log(this.activatedRoute);
  }
}
```

The `ActivatedRoute` can provide different information about the route in the form of [[Observables|observables]]. Some common properties include:

| Property      | Details                                                                                                                           |
| :------------ | :-------------------------------------------------------------------------------------------------------------------------------- |
| `url`         | An `Observable` of the route paths, represented as an array of strings for each part of the route path.                           |
| `data`        | An `Observable` that contains the `data` object provided for the route. Also contains any resolved values from the resolve guard. |
| `params`      | An `Observable` that contains the required and optional parameters specific to the route.                                         |
| `queryParams` | An `Observable` that contains the query parameters available to all routes.                                                       |
# Route snapshots

Page navigations are events over time, and you can access the router state at a given time by retrieving a **route snapshot**.

Route snapshots contain ==essential information about the route==, including its **parameters**, **data**, and **child routes**. In addition, snapshots are static and will not reflect future changes.

Here’s an example of how you’d access a route snapshot:

```angular-ts
import { ActivatedRoute, ActivatedRouteSnapshot } from '@angular/router';

@Component({ ... })
export class UserProfileComponent {

  readonly userId: string;
  private activatedRoute = inject(ActivatedRoute);
  
  constructor() {
    // Example URL: https://www.angular.dev/users/123?role=admin&status=active#contact
    
    // Access route parameters from snapshot
    this.userId = this.route.snapshot.paramMap.get('id');
    // Access multiple route elements
    const snapshot = this.route.snapshot;
    console.log({
      url: snapshot.url,           // https://www.angular.dev
      // Route parameters object: {id: '123'}
      params: snapshot.params,
      // Query parameters object: {role: 'admin', status: 'active'}
      queryParams: snapshot.queryParams,  // Query parameters
    });
  }
}
```

Check out the [`ActivatedRoute` API docs](https://angular.dev/api/router/ActivatedRoute) and [`ActivatedRouteSnapshot` API docs](https://angular.dev/api/router/ActivatedRouteSnapshot) for a complete list of all properties you can access.


# Route parameters

Parameterized URLs allow you to define dynamic paths that allow multiple URLs to the same component while dynamically displaying data based on parameters in the URL.

You can define this type of pattern by adding parameters to your route’s `path` string and prefixing each parameter with the colon (`:`) character.

>[!info]
>Parameters are distinct from information in the URL's query string (query parameters)

The following example displays a user profile component based on the user id passed in through the URL.

```angular-ts
import { Routes } from '@angular/router';
import { ProductComponent } from './product/product.component';
const routes: Routes = [
  { path: 'product/:id', component: ProductComponent }
];
```

You can access parameters by subscribing to `route.params`:

```angular-ts
import { Component, inject, signal } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
@Component({
  selector: 'app-product-detail',
  template: `<h1>Product Details: {{ productId() }}</h1>`,
})
export class ProductDetailComponent {
  productId = signal('');
  private activatedRoute = inject(ActivatedRoute);
  constructor() {
    // Access route parameters
    this.activatedRoute.params.subscribe((params) => {
      this.productId.set(params['id']);
    });
  }
}
```

### withComponentInputBinding

When using **standalone components with the Angular Router**, you can now bind route data directly to component inputs **without having to manually pull from the `ActivatedRoute` service** — but to do this, you must opt into it with `withComponentInputBinding()`.

```angular-ts
export const routes: Routes = [
  {
    path: 'hero/:id',
    component: HeroComponent,
    providers: [withComponentInputBinding()]
  }
];
```

And in the component:

```angular-ts
@Component({
  standalone: true,
  selector: 'app-hero',
  template: `<p>Hero ID: {{ id }}</p>`
})
export class HeroComponent {
  id = input.required<string>()  // This will be automatically bound to route param `id`
}
```




# Query parameters

**Query parameters** provide a flexible way to pass optional data through URLs without affecting the route structure. 

Unlike route parameters, query parameters can **persist across navigation events** and are ==perfect for handling filtering, sorting, pagination, and other stateful UI elements==.

```angular-ts
// Single parameter structure
// /products?category=electronics
router.navigate(['/products'], {
  queryParams: { category: 'electronics' }
});
// Multiple parameters
// /products?category=electronics&sort=price&page=1
router.navigate(['/products'], {
  queryParams: {
    category: 'electronics',
    sort: 'price',
    page: 1
  }
});
```

You can also do the following to pass query parameters:

```angular-ts
router.navigateByUrl('/search?category=books&sortBy=price');
```

You can access query parameters with `route.queryParams`.

Here is an example of a `ProductListComponent` that updates the query parameters that affect how it displays a list of products:

```angular-ts
import { ActivatedRoute, Router } from '@angular/router';

@Component({
  selector: 'app-product-list',
  template: `
    <div>
      <select (change)="updateSort($event)">
        <option value="price">Price</option>
        <option value="name">Name</option>
      </select>
      <!-- Products list -->
    </div>
  `
})

export class ProductListComponent implements OnInit {

  private route = inject(ActivatedRoute);
  private router = inject(Router);
  
  constructor() {
    // Access query parameters reactively
    this.route.queryParams.subscribe(params => {
      const sort = params['sort'] || 'price';
      const page = Number(params['page']) || 1;
      this.loadProducts(sort, page);
    });
  }
  updateSort(event: Event) {
    const sort = (event.target as HTMLSelectElement).value;
    // Update URL with new query parameter
    this.router.navigate([], {
      queryParams: { sort },
      queryParamsHandling: 'merge' // Preserve other query parameters
    });
  }
}
```

# RouterLinkActive

You can use the `RouterLinkActive` directive to **dynamically style navigation elements** based on the current active route. This is common in navigation elements to inform users what the active route is.

```angular-html
<nav>
  <a class="button"
     routerLink="/about"
     routerLinkActive="active-button"
     ariaCurrentWhenActive="page">
    About
  </a> |
  <a class="button"
     routerLink="/settings"
     routerLinkActive="active-button"
     ariaCurrentWhenActive="page">
    Settings
  </a>
</nav>
```

In this example, Angular Router will apply the `active-button` class to the correct anchor link and `ariaCurrentWhenActive` to `page` when the URL matches the corresponding `routerLink`.

### Route matching strategy

By default, `RouterLinkActive` considers any ancestors in the route a match.

```angular-html
<a [routerLink]="['/user/jane']" routerLinkActive="active-link">
  User
</a>
<a [routerLink]="['/user/jane/role/admin']" routerLinkActive="active-link">
  Role
</a>
```

When the user visits `/user/jane/role/admin`, both links would have the `active-link` class.

If you only want to apply the class on an exact match, you need to provide the `routerLinkActiveOptions` directive with a configuration object that contains the value `exact: true`.

```angular-html
<a 
  [routerLink]="['/user/jane']"
  routerLinkActive="active-link"
  [routerLinkActiveOptions]="{exact: true}"
>
  User
</a>
<a 
  [routerLink]="['/user/jane/role/admin']"
  routerLinkActive="active-link"
  [routerLinkActiveOptions]="{exact: true}"
>
  Role
</a>
```

# Guards

Use **==route guards==** to prevent users from navigating to parts of an application without authorization. When users navigate to a "guarded" route, the guard code activates which either enables or disables the user from being able to navigate to that route.

Types of guards:
- **CanActivate**: Determines if a route can be activated.
- **CanActivateChild**: Determines if child routes can be activated.
- **CanDeactivate**: Determines if you can leave the current route.
- **Resolve**: Fetches data before the route is activated.
- **CanLoad**: Prevents lazy-loaded modules from loading until conditions are met.

```angular-ts
export const yourGuardFunction: CanActivateFn = (
  next: ActivatedRouteSnapshot,
  state: RouterStateSnapshot
) => {
  // your  logic goes here
}
```

```angular-ts
{
  path: '/your-path',
  component: YourComponent,
  canActivate: [yourGuardFunction],
}
```






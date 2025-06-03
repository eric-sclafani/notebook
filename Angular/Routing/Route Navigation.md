After declaring your [[Defining Routes|routes]], the `RouterLink` directive is Angular's approach to navigation. It allows you to use standard anchor elements (`<a>`) that seamlessly integrate with Angular's routing system.

Instead of using `<a>` elements with an `href` attribute, you add a `routerLink` directive with the appropriate path in order to leverage Angular routing. 

```angular-html
<nav>
  <a routerLink="/user-profile">User profile</a>
  <a routerLink="/settings">Settings</a>
</nav>
```

Angular has two different syntaxes for defining relative URLs: strings and arrays. When you need to **define dynamic parameters in a relative URL**, use the array syntax. Otherwise, the string syntax is preferred:

```angular-html
<!-- Navigates user to /dashboard -->
<a routerLink="dashboard">Dashboard</a>
<a [routerLink]="['user', currentUserId]">Current User</a>
```


In addition, Angular routing allows you specify whether you want the path to be relative to the current URL or to the root domain based on whether the relative path is prefixed with a forward slash (`/`) or not.

For example, if the user is on `example.com/settings`, here is how different relative paths can be defined for various scenarios:

```angular-html
<!-- Navigates to /settings/notifications -->
<a routerLink="notifications">Notifications</a>
<a routerLink="/settings/notifications">Notifications</a>

<!-- Navigates to /team/:teamId/user/:userId -->
<a routerLink="/team/123/user/456">User 456</a>
<a [routerLink]="['/team', teamId, 'user', userId]">Current User</a>”
```

# Programmatic navigation

While `RouterLink` handles declarative navigation in templates, Angular provides programmatic navigation for scenarios where you need to navigate based on logic, user actions, or application state. By injecting `Router`, you can dynamically navigate to routes, pass parameters, and control navigation behavior in your TypeScript code.

### router.navigate()

You can use the `router.navigate()` method to programmatically navigate between routes by specifying a URL path array.

```angular-ts
import { Router } from '@angular/router';
@Component({
  selector: 'app-dashboard',
  template: `
    <button (click)="navigateToProfile()">View Profile</button>
  `
})
export class AppDashboard {
  private router = inject(Router);
  navigateToProfile() {
    // Standard navigation
    this.router.navigate(['/profile']);
    // With route parameters
    this.router.navigate(['/users', userId]);
    // With query parameters
    this.router.navigate(['/search'], {
      queryParams: { category: 'books', sort: 'price' }
    });
  }
}
```

`router.navigate()` supports both simple and complex routing scenarios, allowing you to pass [[Route State#Route parameters|route parameters]], [[Route State#Query parameters|query parameters]], and control navigation behavior.

You can also build dynamic navigation paths relative to your component’s location in the routing tree using the `relativeTo` option:

```angular-ts
import { Router, ActivatedRoute } from '@angular/router';
@Component({
  selector: 'app-user-detail',
  template: `
    <button (click)="navigateToEdit()">Edit User</button>
    <button (click)="navigateToParent()">Back to List</button>
  `
})
export class UserDetailComponent {
  private route = inject(ActivatedRoute);
  private router = inject(Router);
  constructor() {}
  // Navigate to a sibling route
  navigateToEdit() {
    // From: /users/123
    // To:   /users/123/edit
    this.router.navigate(['edit'], { relativeTo: this.route });
  }
  // Navigate to parent
  navigateToParent() {
    // From: /users/123
    // To:   /users
    this.router.navigate(['..'], { relativeTo: this.route });
  }
}
```

### router.navigateByUrl()

The `router.navigateByUrl()` method provides a direct way to programmatically navigate using URL path strings **rather than array segments**. 

This method is ideal when you have a full URL path and need to perform absolute navigation, especially when working with externally provided URLs or deep linking scenarios.

```angular-ts
// Standard route navigation
router.navigateByUrl('/products');
// Navigate to nested route
router.navigateByUrl('/products/featured');
// Complete URL with parameters and fragment
router.navigateByUrl('/products/123?view=details#reviews');
// Navigate with query parameters
router.navigateByUrl('/search?category=books&sortBy=price');
```

In the event you need to replace the current URL in history, `navigateByUrl` also accepts a configuration object that has a `replaceUrl` option.

```angular-ts
// Replace current URL in history
router.navigateByUrl('/checkout', {
  replaceUrl: true
});
```



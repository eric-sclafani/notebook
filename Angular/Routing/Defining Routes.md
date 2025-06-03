When you navigate to a URL in your web browser, the b**rowser normally makes a network request to a web server and displays the returned HTML page.** When you navigate to a different URL, such as clicking a link, the browser makes another network request and replaces the entire page with a new one. 

A single-page application (SPA) differs in that the browser only makes a request to a web server for the first page, the `index.html`. After that, a ==client-side router ==takes over, controlling which content displays based on the URL. When a user navigates to a different URL, the router updates the page's content in place without triggering a full-page reload.

# What are routes?

In Angular, a **route** is an object that defines which component should render for a specific URL path or pattern, as well as additional configuration options about what happens when a user navigates to that URL.

```angular-ts
import { AdminPage } from './app-admin/app-admin.component';
const adminPage = {
  path: 'admin',
  component: AdminPage
}
```

For this route, when a user visits the `/admin` path, the app will display the `AdminPage` component.

# Route URL paths

**Static URL Paths** refer to routes with predefined paths that don't change bases on dynamic parameters. These are routes that match a `path` string exactly and have a fixed outcome.

Examples of this include:
- "/admin"
- "/blog"
- "/settings/account"


# Wildcards

When you need to catch all routes for a specific path, the solution is a wildcard route which is defined with the double asterisk (`**`).

A common example is defining a Page Not Found component.

```angular-ts
import { Home } from './home/home.component';
import { UserProfile } from './user-profile/user-profile.component';
import { NotFound } from './not-found/not-found.component';
const routes: Routes = [
  { path: 'home', component: Home },
  { path: 'user/:id', component: UserProfile },
  { path: '**', component: NotFound }
];
```

# How Angular matched URLs

When you define routes, the order is important because Angular uses a **first-match wins strategy**. This means that once Angular matches a URL with a route `path`, it stops checking any further routes. As a result, ==always put more specific routes before less specific routes.==

The following example shows routes defined from most-specific to least specific:

```angular-ts
const routes: Routes = [
  { path: '', component: HomeComponent },              // Empty path
  { path: 'users/new', component: NewUserComponent },  // Static, most specific
  { path: 'users/:id', component: UserDetailComponent }, // Dynamic
  { path: 'users', component: UsersComponent },        // Static, less specific
  { path: '**', component: NotFoundComponent }         // Wildcard - always last
];
```

# Lazy vs eager loading

==The examples thus far ==have been implemented with `eager` loading. Eagerly loading route components like this means that t**he browser has to download and parse all of the JavaScript for these components as part of your initial page load**, but the components are available to Angular immediately.

While including more JavaScript in your initial page load leads to slower initial load times, this can lead to more seamless transitions as the user navigates through an application.

On the contrary, with `lazy loading`, Angular will only load the Javascript for a route only at the point at which that route is active.

```angular-ts
import { Routes } from "@angular/router";
export const routes: Routes = [
  // The HomePage and LoginPage components are loaded lazily at the point at which
  // their corresponding routes become active.
  {
    path: 'login',
    loadComponent: () => import('./components/auth/login-page')
  },
  {
    path: '',
    loadComponent: () => import('./components/home/home-page')
  }
]
```

Lazily loading routes can ==significantly improve the load speed of your Angular application by removing large portions of JavaScript from the initial bundle==. These portions of your code compile into separate JavaScript "chunks" that the router requests only when the user visits the corresponding route.

### When to use eager or lazy loading

In general, eager loading is recommended for primary landing page(s) while other pages would be lazy-loaded.

>[!info]
>While lazy routes have the upfront performance benefit of reducing the amount of initial data requested by the user, it adds future data requests that could be undesirable. This is particularly true when dealing with nested lazy loading at multiple levels, which can significantly impact performance.

# Redirects

You can define a route that redirects to another route instead of rendering a component:

```angular-ts
import { BlogComponent } from './home/blog.component';
const routes: Routes = [
  {
    path: 'articles',
    redirectTo: '/blog', 
  },
  {
    path: 'blog',
    component: BlogComponent
  },
];
```

# Route-level providers for DI

Each route has a `providers` property that lets you provide dependencies to that route's content via [[Dependency Injection|dependency injection]].

Common scenarios where this can be helpful include applications that have different services based on whether the user is an admin or not.

```angular-ts
export const ROUTES: Route[] = [
  {
    path: 'admin',
    providers: [
      AdminService,
      {provide: ADMIN_API_KEY, useValue: '12345'},
    ],
    children: [
      {path: 'users', component: AdminUsersComponent},
      {path: 'teams', component: AdminTeamsComponent},
    ],
  },
  // ... other application routes that don't
  //     have access to ADMIN_API_KEY or AdminService.
];
```


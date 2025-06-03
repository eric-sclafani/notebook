As your application grows more complex, **you might want to create routes that are relative to a component other than your root component.** This enables you to create experiences where ==only part of the application changes when the URL changes==, as opposed to the users feeling like the entire page is refreshed.

These types of nested routes are called **==child routes==**. This means you're adding a second [[Router Outlet|<router-outlet>]] to your app, because it is in addition to the `<router-outlet>` in AppComponent.

In this example, the `Settings` component will display the desired panel based on what the user selects. One of the unique things you’ll notice about child routes is that the component often has its own `<nav>` and `<router-outlet>`.

```angular-html
<h1>Settings</h1>
<nav>
  <ul>
    <li><a routerLink="profile">Profile</a></li>
    <li><a routerLink="security">Security</a></li>
  </ul>
</nav>
<router-outlet />
```

A child route is like any other route, in that it needs both a `path` and a `component`. The one difference is that you place child routes in a children array within the parent route.

```angular-ts
const routes: Routes = [
  {
    path: 'settings-component',
    component: SettingsComponent, // this is the component with the <router-outlet> in the template
    children: [
      {
        path: 'profile', // child route path
        component: ProfileComponent, // child route component that the router renders
      },
      {
        path: 'security',
        component: SecurityComponent, // another child route component that the router renders
      },
    ],
  },
];
```


The ==**directive composition**== API lets you apply directives to a component's [[Component Basics#Host elements|host element ]] from within the component TypeScript class.

# Adding directives to a component

You apply directives to a component by adding a `hostDirectives` property to a component's decorator. We call such directives ==**host directives**==.

In the following example, we apply the directive `MenuBehavior` to the host element of `AdminMenu`. This works similarly to applying the `MenuBehavior` to the `<admin-menu>` element in a template.

```angular-ts
@Component({
  selector: 'admin-menu',
  templateUrl: 'admin-menu.html',
  hostDirectives: [MenuBehavior],
})
export class AdminMenu { }
```

# Including inputs and outputs

When you apply `hostDirectives` to your component, the inputs and outputs from the host directives are not included in your component's API by default. 

You can explicitly include inputs and outputs in your component's API by expanding the entry in `hostDirectives`:

```angular-ts
@Component({
  selector: 'admin-menu',
  templateUrl: 'admin-menu.html',
  hostDirectives: [{
    directive: MenuBehavior,
    inputs: ['menuId'],
    outputs: ['menuClosed'],
  }],
})
export class AdminMenu { }
```

By explicitly specifying the inputs and outputs, consumers of the component with `hostDirective` can bind them in a template: 

```angular-html
<admin-menu menuId="top-menu" (menuClosed)="logMenuClosed()">
```


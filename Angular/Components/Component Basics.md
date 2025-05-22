A `Component` is a typescript class containing:

> 1. HTML template for displaying content
> 2. A CSS selector that acts as a **unique identifier** for that component
> 3. Methods/properties for **rendering control** and **application-specific business logic**
> 4. Optional styling

```angular-ts title="nav-bar.component.ts"
@Component({
	selector: 'app-nav-bar',
	templateUrl: 'nav-bar.component.html',
	styleUrl: 'nav-bar.component.css',

	// also possible
	// template: '...',
	// styles: 'body: {height:100%}'
})
export class NavBar(){

	onClick():void{
		console.log('Howdy!')
	}
}
```


In the code above:

> 1. `selector` tells Angular to render a new instance of that component wherever the selector is encountered in the HTML. 
> 2. `templateUrl` links to an external HTML file
> 3. `styleUrl` links an to external CSS/SCSS file
> 4. `template` provides HTML as a string instead of file
> 5. `styles` provides CSS as a string

>[!info]
>Angular throws an error if you provide `templateUrl` AND `template` (same goes for the styles), so only one can be provided.

The `@Component` decorator registers the class as an Angular component.

The object passed into the component contains that component's ==metadata==. This metadata tells Angular how/where to render the component, what styles to apply, what dependencies are needed, etc... All things Angular needs to know how to handle this component.


Angular components are typically composed together to create a **tree of components**

![[component_tree.png|500]]

# Host elements

Angular creates an instance of a component **for every HTML element that matches the component's selector**. 

The DOM element that matches a component's selector is that component's ==**host element**==. The contents of a component's template are rendered inside its host element.

```angular-ts
@Component({
  selector: 'profile-photo',
  template: `
    <img src="profile-photo.jpg" alt="Your profile photo" />
  `,
})
export class ProfilePhoto {}
```

```html
<!-- Using the component -->
<h3>Your profile photo</h3>
<profile-photo />
<button>Upload a new profile photo</button>
```

```html
<!-- Rendered DOM -->
<h3>Your profile photo</h3>
<profile-photo>
  <img src="profile-photo.jpg" alt="Your profile photo" />
</profile-photo>
<button>Upload a new profile photo</button>
```

In the above example, `<profile-photo>` is the host element of the `ProfilePhoto` component.

## Binding to the host element

A component can bind **properties**, **attributes**, and **events** to its host element. This behaves identically to bindings on elements inside the component's template, but instead defined with the `host` property in the `@Component` decorator:

```angular-ts
@Component({
  ...,
  host: {
    'role': 'slider',
    '[attr.aria-valuenow]': 'value',
    '[class.active]': 'isActive()',
    '[tabIndex]': 'disabled ? -1 : 0',
    '(keydown)': 'updateValue($event)',
  },
})
export class CustomSlider {
  value: number = 0;
  disabled: boolean = false;
  isActive = signal(false);
  updateValue(event: KeyboardEvent) { /* ... */ }
  /* ... */
}
```

>[!warning]
> You can also use the `@HostListener` and `@HostBinding` decorators to do this too, but they only exist for _compatibility reasons_. Always prefer `host`.


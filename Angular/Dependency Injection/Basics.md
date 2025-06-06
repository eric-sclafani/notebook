**==Dependency injection,==** or DI, is one of the fundamental concepts in Angular. DI is wired into the Angular framework and **allows classes with Angular decorators, such as Components, Directives, Pipes, and Injectables, to configure dependencies that they need.**

Two main roles exist in the DI system: **dependency consumer** and **dependency provider**.

Angular facilitates the interaction between dependency consumers and dependency providers using an abstraction called `Injector`. 

**When a dependency is requested**, the injector ==checks its registry to see if there is an instance already available there==. If not, a new instance is created and stored in the registry.

Angular creates an application-wide injector (also known as the "==root==" injector) during the application bootstrap process. In most cases you don't need to manually create injectors, but you should know that there is a layer that connects providers and consumers.

# Providing a dependency

A **==service==** is a broad category encompassing any value, function, or feature that an application needs. A service is a class with a narrow, well-defined purpose. Services are typically responsible for things like data handling, client-side application logic, state management, logging, etc... 

This is contrasted to **components**, which should contain code strictly for rending logic in the view template. The component should never handle data fetching, processing, etc... 

A component can delegate certain tasks to services. By defining such processing tasks in an injectable service class, ==you make those tasks available to any component.== You can also make your application more adaptable by configuring different providers of the same kind of service, as appropriate in different circumstances.

# Service examples

Here's an example of a service class that logs to the browser console:

```angular-ts title="src/app/logger.service.ts"
export class Logger {
  log(msg: unknown) { console.log(msg); }
  error(msg: unknown) { console.error(msg); }
  warn(msg: unknown) { console.warn(msg); }
}
```

Services can depend on other services. For example, here's a `HeroService` that depends on the `Logger` service, and also used a `BackendService`. That service in turn might depend on the `HttpClient` service to fetch heroes asynchronously from a server:

```angular-ts
import { inject, Injectable } from "@angular/core";

@Injectable({
	providedIn: 'root',
})
export class HeroService {
	private heroes: Hero[] = [];

	private backend = inject(BackendService);
	private logger = inject(logger);

	async getHeroes() {
		this.heroes = await this.backend.getAll(Hero)
		this.logger.log(`Fetched ${this.heroes.length} heroes.`);
		return this.heroes;
	}
}
```

Next, the `HeroService` can be injected into a component that displayed the heroes:

```angular-ts
import { inject } from '@angular/core';

export class HeroListComponent {
	private heroService = inject(HeroService)
}
```


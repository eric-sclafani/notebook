Most front-end applications need to communicate with a server over the HTTP protocol, to download or upload data and access other back-end services.

Angular provides a client HTTP API for Angular applications, the `HttpClient` service class in `@angular/common/http`.

`HttpClient` is provided using the `provideHttpClient` helper function, which most apps include in the application `providers` in `app.config.ts`.

```angular-ts
export const appConfig: ApplicationConfig = {
  providers: [
    provideHttpClient(),
  ]
};
```

You can then inject the `HttpClient` service as a dependency of your components, services, or other classes:

```angular-ts
@Injectable({providedIn: 'root'})
export class ConfigService {
  private http = inject(HttpClient);
  // This service can now make HTTP requests via `this.http`.
}
```

# Configuration options

### withFetch
By default, `HttpClient` uses the [`XMLHttpRequest`](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest) API to make requests. The `withFetch` feature switches the client to use the [`fetch`](https://developer.mozilla.org/docs/Web/API/Fetch_API) API instead.

```angular-ts
export const appConfig: ApplicationConfig = {
  providers: [
    provideHttpClient(
      withFetch(),
    ),
  ]
};
```

`fetch` is a more modern API and is available in a few environments where `XMLHttpRequest` is not supported. It does have a few limitations, such as not producing upload progress events. 

>[!info]
>In general, it's not recommended to use fetch API here unless necessary, as `HttpClient` works fine on its own.

### withInterceptors

`withInterceptors` configures the set of interceptor functions which will process requests made through `HttpClient`. See [[Intercepting Requests]] for more information.

# Making HTTP requests

`HttpClient` has methods corresponding to the different HTTP verbs used to make requests, both to load data and to apply mutations on the server.

Each method returns an [[Observables|observable]] which, when subscribed, ==sends the request== and then ==emits the results== when the server responds.

All of the standard HTTP verbs are available (get, post, put, delete, patch)


### Fetching JSON data

Fetching data from a backend often requires making a GET request using the [`HttpClient.get()`](https://angular.dev/api/common/http/HttpClient#get) method. This method takes two arguments: the string endpoint URL from which to fetch, and an _optional options_ object to configure the request.

The following snippet fetches and returns an `Observable` of `User` objects:

```angular-ts

getAllUsers(){
	return this.http.get<User[]>('api/user/GetAllUsers');
}

```

You would then `subscribe` to this observable wherever it is needed:

```angular-ts
Component({
	selector: 'app-user-list',
	templateUrl: 'user-list.component.html',
	styleUrl: 'user-list.component.css',
})
export class UserListComponent implements OnInit{

	ngOnInit(){
		this.userService.getAllUsers().subscribe(users => {
			// perform component logic here...
		})
	}
}
```

### Fetching other types of data

By default, `HttpClient` assumes that servers will return JSON data. When interacting with a non-JSON API, you can tell `HttpClient` what response type to expect and return when making the request. This is done with the `responseType` option.

| **`responseType` value** | **Returned response type**                                                                                                                |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `'json'` (default)       | JSON data of the given generic type                                                                                                       |
| `'text'`                 | string data                                                                                                                               |
| `'arraybuffer'`          | [`ArrayBuffer`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer) containing the raw response bytes |
| `'blob'`                 | [`Blob`](https://developer.mozilla.org/docs/Web/API/Blob) instance                                                                        |


# Setting URL params


Specify request parameters that should be included in the request URL using the `params` option.

```angular-ts
http.get('/api/config', {
  params: {filter: 'all'},
}).subscribe(config => {
  // ...
});
```

```angular-ts
const baseParams = new HttpParams().set('filter', 'all');

http.get('/api/config', {
  params: baseParams.set('details', 'enabled'),
}).subscribe(config => {
  // ...
});
```

# Server response events

For convenience, `HttpClient` by default returns an `Observable` of the data returned by the server (the response body). 

Occasionally it's desirable to examine the actual response, for example to retrieve specific response headers.

```angular-ts
this.http.get<Config>('/api/config', {observe: 'response'}).subscribe(res => {
  console.log('Response status:', res.status);
  console.log('Body:', res.body);
});
```

# Raw progress events

In addition to the response body or response object, `HttpClient` can also return a ==stream of raw _events_== corresponding to specific moments in the request lifecycle. 

These events include when the **request is sent**, when the **response header is returned**, and **when the body is complete**. These events can also include **progress events** which report upload and download status for large request or response bodies.

```angular-ts
http.post('/api/upload', myData, {
  reportProgress: true,
  observe: 'events',
}).subscribe(event => {

  switch (event.type) {
  
    case HttpEventType.UploadProgress:
      console.log('Uploaded ' + event.loaded + ' out of ' + event.total + ' bytes');
      break;
      
    case HttpEventType.Response:
      console.log('Finished uploading!');
      break;
  }
});
```

Each `HttpEvent` reported in the event stream has a `type` which distinguishes what the event represents:

| **`type` value**                 | **Event meaning**                                                                  |
| -------------------------------- | ---------------------------------------------------------------------------------- |
| `HttpEventType.Sent`             | The request has been dispatched to the server                                      |
| `HttpEventType.UploadProgress`   | An `HttpUploadProgressEvent` reporting progress on uploading the request body      |
| `HttpEventType.ResponseHeader`   | The head of the response has been received, including status and headers           |
| `HttpEventType.DownloadProgress` | An `HttpDownloadProgressEvent` reporting progress on downloading the response body |
| `HttpEventType.Response`         | The entire response has been received, including the response body                 |
| `HttpEventType.User`             | A custom event from an Http interceptor.                                           |
# Handling request failure

There are two ways an HTTP request can fail:

- A network or connection error can prevent the request from reaching the backend server.
- The backend can receive the request but fail to process it, and return an error response.

`HttpClient` captures both kinds of errors in an `HttpErrorResponse` which it returns through the `Observable`'s error channel. Network errors have a `status` code of `0` and an `error` which is an instance of [`ProgressEvent`](https://developer.mozilla.org/docs/Web/API/ProgressEvent). 

Backend errors have the failing `status` code returned by the backend, and the error response as the `error`. Inspect the response to identify the error's cause and the appropriate action to handle the error.

You can use the `catchError` operator to transform an error response into a value for the UI. This value can tell the UI to display an error page or value, and capture the error's cause if necessary.

Sometimes transient errors such as network interruptions can cause a request to fail unexpectedly, and simply retrying the request will allow it to succeed. RxJS provides several _retry_ operators which automatically re-subscribe to a failed `Observable` under certain conditions. For example, the `retry()` operator will automatically attempt to re-subscribe a specified number of times.

[More information here](https://www.tektutorialshub.com/angular/angular-http-error-handling/)

# Best practices

While `HttpClient` can be injected and used directly from components, generally we recommend you create reusable, injectable services which isolate and encapsulate data access logic. For example, this `UserService` encapsulates the logic to request data for a user by their id:

```angular-ts

@Injectable({providedIn: 'root'})
export class UserService {

  private http = inject(HttpClient);
  
  getUser(id: string): Observable<User> {
    return this.http.get<User>(`/api/user/${id}`);
  }
}
```

Within a component, you can combine `@if` with the [[Pipes|async pipe]] pipe to render the UI for the data only after it's finished loading:

```angular-ts
import { AsyncPipe } from '@angular/common';

@Component({
  imports: [AsyncPipe],
  template: `
    @if (user$ | async; as user) {
      <p>Name: {{ user.name }}</p>
      <p>Biography: {{ user.biography }}</p>
    }
  `,
})

export class UserProfileComponent {

  userId = input.required<string>();
  user$!: Observable<User>;
  
  private userService = inject(UserService);
  
  constructor(): void {
  
    effect(() => {
      this.user$ = this.userService.getUser(this.userId());
    });
  }
}
```

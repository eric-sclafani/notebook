An ==**Observable**== is a core concept that represents a **stream of data over time**. It’s a way to model **asynchronous data flows**—things like events, HTTP requests, user input, WebSocket messages, and more.

An **Observable** is like a **function** that:

- Doesn’t return a value immediately.
- Can return **multiple values** over time.
- Allows you to **subscribe** to it and react when values arrive.
- **Emits values** (these could be numbers, objects, arrays, etc.)
- **Notifies** subscribers when values are emitted.
- Can also signal when:
    - It completes (`complete`)
    - An error occurs (`error`)


**Consumers** can then ==subscribe== to observables to listen to all the data they transmit. Consumers can be subscribed to multiple observables at the same time. This data can then be transformed as it moves through the data pipeline toward the user.


```js
const {Observable} = require('rxjs')

const wrapArrayIntoObservable = arr => {
    return new Observable(subscriber => {
        for(let item of arr) {
            subscriber.next(item);
        }
    });
}
const data = [1, 2, 3, 4, 5];

const observable = wrapArrayIntoObservable(data);

observable.subscribe(val => console.log('Subscriber 1: ' + val));
observable.subscribe(val => console.log('Subscriber 2: ' + val));
```


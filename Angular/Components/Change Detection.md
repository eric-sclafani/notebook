---
tags:
  - WIP
---
Angular uses `zone.js`   to ***detect*** whether it needs to update the UI in response to changes in dependent data. Change detection automatically runs in the following scenarios:

- Component initializes (`ngOnInit`)
- Event handlers (like `(click)`)
- Timers (`setTimeout`, `setInterval`)
- Observable/Promise resolutions (like `subscribe`)
- Form inputs

In some cases, it may be required to manually call the change detector like so:
```typescript {5}
constructor(private cd: ChangeDetectorRef) {}

someCallback() {
  this.title = 'Changed outside Angular';
  this.cd.detectChanges(); 
}
```

Angular's change detection strategy abstracts a lot of the complication away, leading to a more developer-friendly experience.

However, as Angular apps grow, performance can become an issue when change detection occurs often and unnecessarily.  Thus, Angular provides numerous ways to remedy this.

# OnPush change detection

By default, Angular uses the **default** change detection strategy, which means **every component is checked** whenever _anything_ changes—like a user event or async event.

With `OnPush`, Angular **only checks a component when**:

1. An `@Input()` property changes **by reference**.
2. An event originates from the component (e.g. click, form input).
3. You manually trigger change detection (e.g. via `ChangeDetectorRef.markForCheck()`).

`OnPush` can lead to performance increases because Angular runs less change detections. However, using it requires writing code in a specific way that works with this detection strategy [see here](https://blog.angular-university.io/onpush-change-detection-how-it-works/) for more information.

>[!todo]
>Come back to this section after playing around more with change detection
>


# Running code outside of Zone.js

>[!info]
>Angular is experimenting with the option to move away from `zone.js`. See [here](https://angular.dev/guide/experimental/zoneless).

Angular wraps `zone.js` in an injectable service called `ngZone` which lets you control what code to run outside of change detection.

Generally, any operations that don't **involve the UI** nor **change variables the UI uses** may benefit from running outside of `zone.js` . For example, if you're running ==frequent async operations== (like `setInterval`, animation loops, or third-party library events), change detection can be computationally expensive.

To run code outside `zone.js`, use `ngZone.runOutsideAngular` :

```typescript
constructor(private ngZone: NgZone) {}

ngOnInit() {
  this.ngZone.runOutsideAngular(() => {
    setTimeout(() => {
      // This code runs outside Angular
      // No change detection triggered
    }, 1000);
  });
}
```

The change detection zone can be re-entered by using `ngZone.run`:
```typescript
this.ngZone.run(() => {
  this.value = 42; // triggers change detection
});
```
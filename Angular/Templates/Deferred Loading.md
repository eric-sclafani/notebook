#WIP

Deferrable views, also known as `@defer` blocks, **reduce the initial bundle size of your application** by deferring the loading of code that is not strictly necessary for the initial rendering of a page. 

This often results in a ==faster initial load== and improvement in Core Web Vitals (CWV), primarily Largest Contentful Paint (LCP) and Time to First Byte (TTFB).

```angular-html
@defer {
  <large-component />
}
```

 
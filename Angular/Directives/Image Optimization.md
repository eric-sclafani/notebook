The `NgOptimizedImage` directive makes it easy to adopt performance best practices for loading images.

The directive ensures that the loading of the [Largest Contentful Paint (LCP)](http://web.dev/lcp) image is prioritized by:

- Automatically setting the `fetchpriority` attribute on the `<img>` tag
- Lazy loading other images by default
- Automatically generating a preconnect link tag in the document head
- Automatically generating a `srcset` attribute
- Generating a [preload hint](https://developer.mozilla.org/docs/Web/HTML/Link_types/preload) if app is using SSR

In addition to optimizing the loading of the LCP image, `NgOptimizedImage` enforces a number of image best practices, such as:

- Using [image CDN URLs to apply image optimizations](https://web.dev/image-cdns/#how-image-cdns-use-urls-to-indicate-optimization-options)
- Preventing layout shift by requiring `width` and `height`
- Warning if `width` or `height` have been set incorrectly
- Warning if the image will be visually distorted when rendered


# Using the directive

Apply the `ngSrc` attribute directive:

```angular-html
<img ngSrc="cat.jpg">
```

Marking an image as `priority` applies the following optimizations:
- Sets `fetchpriority=high` (read more about priority hints [here](https://web.dev/priority-hints))
- Sets `loading=eager` (read more about native lazy loading [here](https://web.dev/browser-level-image-lazy-loading))
- Automatically generates a [preload link element](https://developer.mozilla.org/docs/Web/HTML/Link_types/preload) if [rendering on the server](https://angular.dev/guide/ssr).

```angular-html
<img ngSrc="cat.jpg" width="400" height="200" priority>
```

### Fill mode

In cases where you want to have an image fill a containing element, you can use the `fill` attribute. This is often useful when you want to achieve a "background image" behavior. 

It can also be helpful when you **don't know the exact width and height** of your image, but you do have a parent container with a known size that you'd like to fit your image into (see "object-fit" below).

```angular-html
<img ngSrc="cat.jpg" fill>
```

 You can use the [object-fit](https://developer.mozilla.org/docs/Web/CSS/object-fit) CSS property to change how the image will fill its container. If you style your image with `object-fit: "contain"`, the image will maintain its aspect ratio and be "letterboxed" to fit the element. 
 
 If you set `object-fit: "cover"`, the element will retain its aspect ratio, fully fill the element, and some content may be "cropped" off.

>[!warning]
>**IMPORTANT:** For the "fill" image to render properly, its parent element **must** be styled with `position: "relative"`, `position: "fixed"`, or `position: "absolute"`.

# Using placeholders

`NgOptimizedImage` can display an automatic low-resolution placeholder for your image if you're using a CDN or image host that provides automatic image resizing. Take advantage of this feature by adding the `placeholder` attribute to your image:

```angular-html
<img ngSrc="cat.jpg" width="400" height="200" placeholder>
```

Adding this **attribute automatically requests a second**, smaller version of the image using your specified image loader. This small image will be applied as a `background-image` style ==with a CSS blur while your image loads.== If no image loader is provided, no placeholder image can be generated and an error will be thrown.

# More info

There's a lot to image optimization in Angular. [Documentation link](https://angular.dev/guide/image-optimization)


# Core Web Vitals

## CLS - Cumulative Layout Shift

- **What causes poor CLS?**

  - Images without dimensions.
  - Ads, embeds, iframes without dimensions.
  - Dynamically injected content.
  - Webfonts causing FOIT/FOUT.

- Always specify a width and height on image/video elements. Or reserve space with CSS aspect ratio boxes. Make sure the browser can allocate the proper space while it's loading.
- Image aspect ratio = ratio of width to height.
- Want a 16:9 ratio? If the image has a height of 360px the width is 360 \* (16/9) which is 640px.

  ```css
  img {
    aspect-ratio: attr(width) / attr(height);
  }
  ```

- Reserve enough space for dynamic content, like ads or promos.
- Avoid inserting new content above existing content, unless in response to a user interaction.
- If you reserve space with a set container size the elements inside that container will not shift the entire layout.

## LCP - Largest Contentful Paint

measures when the page becomes useful to the user or when the main content on the screen has likely loaded.

- **What causes poor LCP?**

  - Slow server response times.
  - Render-blocking JavaScript and CSS.
  - Slow resource load times.
  - Client-side rendering.

- **Elements that affect LCP:**

  - `<img>` elements.
  - `<image>` elements inside of an svg element.
  - `<video>` elements.
  - An element with a background loaded via the `url()` function as opposed to a CSS gradient.
  - Block level elements containing text nodes or other inline-level text elements.

  Optimize images, compress them, use an image CDN, use a more efficient file format, or consider removing the image if it's not important.

  - You can find the LCP using the performance tab in the Devtools under timimgs. It will highlight the element. You can use this as a stepping stone to figure out where you should start optimizing. You can also use Lighthouse.
  - Remove overly large `srcset` images.
  - Use WebP for images.

    WebP lossless images are 26% smaller in size than PNGs. WebP lossy images are 25 — 34% smaller than comparable JPEG images.

  - Defer any non-critical JavaScript and CSS to speed up loading of the main content of your web-page.
  - A faster server response time directly improves every single page-load metric, including LCP.
  - **HTTP/2 server push:** Start pushing as soon as you get a response to the initial request. You need to be careful as it's possible to over-push. It is not HTTP cache aware.

  ## FID - First Input Delay

  Measures your user's first impression of your site's interactivity and responsiveness.

  **What causes poor FID?**

  - Long tasks.
  - Long JavaScript execution time.
  - Large JavaScript bundles.
  - Render-blocking JavaScript.

# Resources

- [Optimize for Core Web Vitals](https://youtu.be/AQqFZ5t8uNc)

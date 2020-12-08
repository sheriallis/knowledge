# Next.js Basics

## Navigate Between Pages

- The Link component enables client side navigation which means the page transition is taken care of by JS.
- Next.js has automatic code splitting so only the necessary code will be loaded. Code for other pages is not automatically served at once.
- If one page throws an error, the other pages can still load.
- In a production build, the code for a linked page is pre-fetched once the link component appears in the browser's viewport.

## Assets, Metadata, and CSS

- Next has CSS modules, sass, and styled-jsx support built right in.
- CSS modules automatically generate unique class names.
- Next.js' code splitting also works on CSS modules.
- The `App` component is a toplevel component that you can use to keep state across pages. You use it by creating an `_app.js` file in the pages directory. It can used if you want to load certain styles on all pages.

  ```jsx
  export default function App({ Component, pageProps }) {
    return <Component {...pageProps} />;
  }
  ```

- When adding a `App` component it requires a dev server restart.

- You can **only import global styles in the \_app.js file**, nowhere else. The **filename can be anything you want** but it **has to be imported in this file.**

- There's a classlist library that allows you to toggle classnames easily. Install it by running:

  `npm install classnames` or `yarn add classnames`.

- Support for Sass is built in you need to remember to install sass though: `npm install sass`.
- You can use Sass with CSS modules using the file extension `.module.scss` or `.module.sas`.
- Next.js compiles CSS using PostCSS out of the box. This requires no configuration.
- If you want to use a customized PostCSS config, you need to create a file called `postcss-preset-env` and `postcss-flexbugs-fixes` after installing the following packages:

  ```bash
  npm install postcss-preset-env postcss-flexbugs-fixes
  ```

  [Advanced Features: Customizing PostCSS Config | Next.js](https://nextjs.org/docs/advanced-features/customizing-postcss-config)

## Pre-rendering and Data Fetching

- Next.js pre-renders pages on the client-side making sure the HTML for each page is generated in advance. This is good for performance and SEO. It also means your pages are still available to people that have JavaScript disabled.
- **Hydration:** The generated HTML is associated with the minimal amount of JavaScript needed. When the page is loaded the JavaScript is run and the page becomes fully interactive.
- A plain React.js app doesn't have pre-render pages into static HTML so without JavaScript enabled you will get a “You need to enable JavaScript to run this app.” message.

- **Static generation:** Generates the HTML at build time and that pre-rendered HTML is reused on each request.
- **Server-side rendering:** Generates the HTML on each request.
- **Next.js let's you choose which way you want to render each page individually so you can have an app with a mix of both types of rendering.**
- Use SSR for a page that shows content that is frequently updated and changes on every request for all other situation using SG would be the best option.
- For some pages you might need to fetch some data to render the HTML, maybe by accessing the filesystem, fetching an external API, or querying your database at build time. If this is the case, you can use the async function `getStaticProps` which runs at build time in production.

  ```jsx
  export default function Home(props) { ... }

  export async function getStaticProps() {
    // Get external data from the file system, API, DB, etc.
    const data = ...

    // The value of the `props` key will be
    //  passed to the `Home` component
    return {
      props: ...
    }
  }
  ```

- To parse the (YAML) metadata in markdown files you need to install the gray-matter package:

  ```bash
  npm install gray-matter
  ```

- You can query a database directly from `getStaticProps` **because it only runs on the server-side** it will never run on the client side. It **won't even be included in the JS bundle** for the browser.

  - querying a database directly

    ```jsx
    import someDatabaseSDK from 'someDatabaseSDK'

    const databaseClient = someDatabaseSDK.createClient(...)

    export async function getSortedPostsData() {
      // Instead of the file system,
      // fetch post data from a database
      return databaseClient.query('SELECT posts...')
    }
    ```

- Because it’s meant to be run at build time, you won’t be able to use data that’s only available during request time, such as query parameters or HTTP headers.

- To use Server-side Rendering, you need to export `getServerSideProps` instead of `getStaticProps`

  ```jsx
  export async function getServerSideProps(context) {
    return {
      props: {
        // props for your component
      },
    };
  }
  ```

## Dynamic Routes

- **In the case where each page path relies on external data you need to use dynamic URLs.**

- Pages that begin with `[` and end with `]` are dynamic routes in Next.js (like `[id].js`).

- **In order for `getStaticPaths` to run without failing each object returned when mapping over posts must have the `params` key and contain an object with the `id` key.**

  ```jsx
  export function getAllPostIds() {
    const fileNames = fs.readdirSync(postsDirectory);

    // Returns an array that looks like this:
    // [
    //   {
    //     params: {
    //       id: 'ssg-ssr'
    //     }
    //   },
    //   {
    //     params: {
    //       id: 'pre-rendering'
    //     }
    //   }
    // ]
    return fileNames.map((fileName) => {
      return {
        params: {
          id: fileName.replace(/\.md$/, ""),
        },
      };
    });
  }
  ```

- To render markdown content you need a library like `remark`: `npm install remark remark-html`

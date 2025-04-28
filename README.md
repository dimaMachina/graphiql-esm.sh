# Using GraphiQL 4 with React 19 via esm.sh CDN

> **TL;DR:**
>
> Time to say goodbye to UMD builds! 👋
>
> With React 19, [UMD builds have been removed](https://react.dev/blog/2024/04/25/react-19-upgrade-guide#umd-builds-removed).  
> It’s recommended to use an ESM-based CDN like [esm.sh](https://esm.sh).

## Setup

### 1. Add styles

Include the GraphiQL stylesheet:

```html
<link
  rel="stylesheet"
  href="https://esm.sh/graphiql@4.0.0-canary-bab1879f.0/dist/style.css"
/>
```

### 2. Load React, ReactDOM, GraphiQL, and GraphiQL Toolkit

Add the following `<script>` to your HTML:

```html
<script type="module">
  import { createElement } from 'https://esm.sh/react@19.1.0'
  import { createRoot } from 'https://esm.sh/react-dom@19.1.0/client'
  import { GraphiQL } from 'https://esm.sh/graphiql@4.0.0-canary-bab1879f.0?standalone'
  import { createGraphiQLFetcher } from 'https://esm.sh/@graphiql/toolkit@0.11.1'

  const fetcher = createGraphiQLFetcher({
    url: 'https://countries.trevorblades.com'
  })
  const container = document.getElementById('graphiql')
  const root = createRoot(container)
  root.render(createElement(GraphiQL, { fetcher }))
</script>
```

### 3. (Optional) Use Import Maps

To simplify imports, you can use [Import Maps](https://esm.sh/#using-import-maps):

```html
<script type="importmap">
  {
    "imports": {
      "react": "https://esm.sh/react@19.1.0",
      "react-dom/client": "https://esm.sh/react-dom@19.1.0/client",
      "graphiql": "https://esm.sh/graphiql@4.0.0-canary-bab1879f.0?standalone",
      "@graphiql/toolkit": "https://esm.sh/@graphiql/toolkit@0.11.1"
    }
  }
</script>
<script type="module">
  import { createElement } from 'react'
  import { createRoot } from 'react-dom/client'
  import { GraphiQL } from 'graphiql'
  import { createGraphiQLFetcher } from '@graphiql/toolkit'

  const fetcher = createGraphiQLFetcher({
    url: 'https://countries.trevorblades.com'
  })
  const container = document.getElementById('graphiql')
  const root = createRoot(container)
  root.render(createElement(GraphiQL, { fetcher }))
</script>
```

## Example

You can try a working version of this setup in [examples/index.html](./examples/index.html).

To run the example locally:

```sh
pnpm start
```

This will start a local server and open the demo in your browser.

> Make sure you have dependency installed by running `pnpm i`.

## Notes

> [!NOTE]  
> You might see a console warning: `module "@emotion/is-prop-valid" not found`.
>
> This is **safe to ignore**.
>
> Here’s the relevant source snippet:
>
> ```js
> try {
>   /**
>    * We attempt to import this package but require won't be defined in esm environments, in that case
>    * isPropValid will have to be provided via `MotionContext`. In a 6.0.0 this should probably be removed
>    * in favour of explicit injection.
>    */
>   loadExternalIsValidProp(require('@emotion/is-prop-valid').default)
> } catch (_a) {
>   // We don't need to actually do anything here - the fallback is the existing `isPropValid`.
> }
> ```

## Final Result

At the end, you will have a fully functional GraphiQL 4 instance running with React 19 — **without any UMD build needed**!

## 📚 Resources

- [esm.sh documentation](https://esm.sh)
- [React 19 Upgrade Guide](https://react.dev/blog/2024/04/25/react-19-upgrade-guide)
- [GraphiQL GitHub repository](https://github.com/graphql/graphiql)

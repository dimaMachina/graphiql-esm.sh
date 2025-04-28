# GraphiQL 4 with React 19 via esm.sh CDN

> TL;DR:
>
> Time to say good buy for UMD builds 👋.

Starting from React
19, [they no longer produce UMD builds](https://react.dev/blog/2024/04/25/react-19-upgrade-guide#umd-builds-removed).
Alternately they suggest of using an ESM-based CDN such as [esm.sh](https://esm.sh)

## Setup

1. Add styles

```html
<link
  rel="stylesheet"
  href="https://esm.sh/graphiql@4.0.0-canary-bab1879f.0/dist/style.css"
/>
```

2. Add a `<script>` tag to your HTML file to load React, ReactDOM, GraphiQL and GraphiQL Toolkit.

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

Alternatively, you can bare import specifiers instead of URLs
via [import maps](https://esm.sh/#using-import-maps).

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

> [!TIP]
>
> You can ignore error in console saying `module "@emotion/is-prop-valid" not found`.
>
> Because it fallbacks to `isPropValid`, here is source code of this message:
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

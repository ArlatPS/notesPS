# NextJS

## 13

### Routing

- app folder instead of pages
- folders inside give urls
  - about -> siteName/about
  - [slug] - dynamic (name taken in params)
  - (toOmit) - omits this name
- file conventions
  - page.js - the page on url
  - layout.js - create shared UI for segment and its children (wraps page or child segment)
  - template.js - like layout but a new component instance is mounted on navigation
  - loading.js - loading UI for segment and children - wraps in React Suspense Boundary
  - error.js - error UI - React Error Boundary
  - not-found.js - triggered by `notFound() from next/navigation` for example when user not found (not 404)
  - head.js - for `<head>` (can be also done in layout) - deprecated with 13.2 (replaced by export const metadata)
- export default function component from these files
- using turbopack - in "scripts" `"dev": "next dev --turbo"`

### Server Side Components

- **components are server side static by default**
- type of rendering is changed by adding **cache** or **next** option to fetch
- `fetch("url", { cache: "no-store" })` - **SSR** (old getServerSideProps)
- `fetch("url", { cache: "force-cache" })` - **SSG** (old getStaticProps)
- `fetch("url", {
  next: { revalidate: 10 },
})` - **ISR** time in seconds (old getStaticProps with revalidate)

- client side when at the top before imports `"use client";`
- client side for state...
- createContext only in client side, so separate file with `"use client";`
- to refresh from client component

  ```
  import { useRouter } from "next/navigation"
  const router = useRouter();
  router.refresh();
  ```

- generateStaticParams with dynamic route segments to statically generate routes at build time instead of on-demand at request time
  ```
  /product/[id]	{ id: string }[]
  export function generateStaticParams() {
    return [
      { id: '1' },
      { id: '2' },
      { id: '3' },
    ];
  }
  ```

### Route Segment Config Options

- config for behaviour of Page, Layout or a Route Handler (API Routes)
- dynamic
  ```
  export const dynamic = 'auto'
  // 'auto' | 'force-dynamic' | 'error' | 'force-static'
  ```
- revalidate
  ```
  export const revalidate = false
  // false | 'force-cache' | 0 | number
  ```
- ...docs

### Caching

- `import { cache } from 'react';` to cache request that don't use fetch() ex. server component connecting to db
- fetch() in 13 is automatically cached
- In this new model, we recommend fetching data directly in the component that needs it, even if you're requesting the same data in multiple components instead of prop drilling.
- POST requests are not automatically deduplicated when using fetch
- preload pattern for prefetching
- pattern for preload and caching only on server

  ```
  import { cache } from 'react';
  import 'server-only';

  export const preload = (id: string) => {
    void getUser(id);
  }

  export const getUser = cache(async (id: string) => {
    // ...
  });
  ```

## General

- useSearchParams() - client side
  ```
  const searchParams = useSearchParams();
  const search = searchParams.get('search');
  ```
- cookies() - opt to dynamic rendering
- NextResponse
  - .json()
  - cookies
    - .set(name, value)
    - .get(name)
    - .delete(name)
    - .getAll()
  - .redirect()
  - .next() - middleware
  - .status()
  - .statusText()
  - .error()
- Request.text() - returns a promise that resolves with a text representation of the request body


## Streaming
- With SSR, there's a series of steps that need to be completed before a user can see and interact with a page:
1.First, all data for a given page is fetched on the server.
2.The server then renders the HTML for the page.
3.The HTML, CSS, and JavaScript for the page are sent to the client.
4.A non-interactive user interface is shown using the generated HTML, and CSS.
5.Finally, React hydrates the user interface to make it interactive.

- Streaming allows you to break down the page's HTML into smaller chunks and progressively send those chunks from the server to the client.
- with loading.js
- with <Suspense> from "react" wrapped around loading component

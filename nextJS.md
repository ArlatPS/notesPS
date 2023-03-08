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
  - head.js - for `<head>` (can be also done in layout)
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

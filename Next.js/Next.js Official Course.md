```table-of-contents
style: nestedList # TOC style (nestedList|inlineFirstLevel)
maxLevel: 0 # Include headings up to the speficied level
includeLinks: true # Make headings clickable
debugInConsole: false # Print debug info in Obsidian console
```
This documentation is a condensed form of: https://nextjs.org/learn/

As such it essentially goes through the process of creating a full-stack dashboard application. Therefore some parts may not be relevant to LTT. Examples include database querying, the necessity of fetching data on the server side to avoid exposing database secrets and more...

I've tried to make note of the areas which aren't relevant to this project but are useful to know about.

*Note that this documentation is in TypeScript but you can use JavaScript for Next.js too (just replace `tsx` or `ts` with `jsx` and `js` respectively.*

The TypeScript-specific parts are annoying but bare with.

Next.js uses JSX/TSX so a lot of it is exactly the same as in normal React. It differs in certain areas like data fetching, optimisation, rendering methodologies (static/dynamic) etc.

It has a lot of useful features for modern web development, but these features also sometimes complicate the development process.

I believe on the whole it's well worth using Next for its many benefits despite the complications, as at the very least those complications are built-in to Next itself and so by understanding these docs you won't be blindsided as can often occur in normal React, where you have to come up with custom solutions.

It is very important to read chapters 7 onwards as this is where the fundamentals really differ from `create-react-app`. For example, if your data is constantly changing, you need to use streaming as you should try to avoid `useEffect` which will negate the benefits of SSR. Streaming also has a built-in feature to display skeleton components (blank boxes).

See the tutorial application here:
https://nextjs-dashboard-v12i.vercel.app/

See the repo for it here:
https://github.com/arazabedi/nextjs-dashboard

Use the folder structure in the above example of a Next.js application as a template for a new app.
# 0. Extras
#### Data Visualisation:
https://www.tremor.so/
https://www.chartjs.org/
https://airbnb.io/visx/

# 1. Folder Structure

- **`/app`**: Contains all the routes, components, and logic for your application, this is where you'll be mostly working from.
- **`/app/lib`**: Contains functions used in your application, such as reusable utility functions and data fetching functions.
- **`/app/ui`**: Contains all the UI components for your application, such as cards, tables, and forms. To save time, we've pre-styled these components for you.
- **`/public`**: Contains all the static assets for your application, such as images.
- **`/scripts`**: Contains a seeding script that you'll use to populate your database in a later chapter.
- **Config Files**: You'll also notice config files such as `next.config.js` at the root of your application. Most of these files are created and pre-configured when you start a new project using `create-next-app`. You will not need to modify them in this course.

# 2. Styling

##### CSS Modules

Next.js recommends Tailwind, however you can also use CSS Modules:

- Instead of using Tailwind, you can import a particular CSS module from a different file like this.
- Provides a way to make CSS classes locally scoped to components by default, reducing the risk of styling conflicts.

```tsx
//page.tsx
import styles from '@/app/ui/home.module.css';

export default function Page() {
  return (
	  <>
		  <div className={styles.shape}></div>
	  </>
  );
}

```

```css
/* home.module.css */
.shape {
  height: 0;
  width: 0;
  border-bottom: 30px solid black;
  border-left: 20px solid transparent;
  border-right: 20px solid transparent;
}
```

#### Using `clsx` to Toggle Class Names

You can use `clsx` to conditionally change class names.

Just import the library and then use the following format:

```tsx
import { CheckIcon, ClockIcon } from '@heroicons/react/24/outline';
import clsx from 'clsx';

export default function InvoiceStatus({ status }: { status: string }) {
  return (
    <span
      className={clsx(
        'inline-flex items-center rounded-full px-2 py-1 text-xs',
        {
          'bg-gray-100 text-gray-500': status === 'pending',
          'bg-green-500 text-white': status === 'paid',
        },
      )}
    >
      {status === 'pending' ? (
        <>
          Pending
          <ClockIcon className="ml-1 w-4 text-gray-500" />
        </>
      ) : null}
      {status === 'paid' ? (
        <>
          Paid
          <CheckIcon className="ml-1 w-4 text-white" />
        </>
      ) : null}
    </span>
  );
}

```
# 3. Optimising Fonts and Images

#### `next/font`

Use the `next/font` in order to render the test *just* in the required font. Otherwise the browser might render the original text and then fetch the desired font, causing Cumulative Layout Shift, where components move in an undesirable way after the initial load.

Next.js will download fonts at build time and host them in the server. No additional requests for fonts are made by the browser, improving performance.
#### Primary Font

Make a `font.ts` file and import a font:

```ts
//fonts.ts
import { Inter } from 'next/font/google';

export const inter = Inter({ subsets: ['latin'] });
```

Then import the module from that file in your layout.tsx and add it the the body element to use it throughout your application:

```ts
// layout.tsx
import '@/app/ui/global.css';
import { inter } from '@/app/ui/fonts';
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body className={`${inter.className} antialiased`}>{children}</body>
    </html>
  );
}
```

*antialiased is a Tailwind class which smooths out the font*
#### Secondary Font

Simply just add another import and create its variable:

```ts
// fonts.ts
import { Inter } from 'next/font/google';
import { Lusitana } from 'next/font/google';

export const inter = Inter({ subsets: ['latin'] });
export const lusitana = Lusitana({weight: ['400', '700'], subsets: ['latin']})
```

Import like so: 

```tsx
<p className={`${lusitana.className}`}>
	Hello
</p>
```

*Use `layout.tsx` for high-level styling, and scope to components to override.*


#### Image Optimisation

Serve static assets such as images under `/public`.

The HTML `<img>` is unresponsive, size-agnostic, can cause layout shift, and you have to lazy load images outside the viewport.

Don't use it.

Use...

#### `next/image`

The `<Image>` component automatically:
- Prevents layout shift when loading
- Resized images to avoid sending large images to small viewports
- Lazy loads by default
- Serves WebP and AVIF (modern formats)


Example:

```ts
// page.ts
import Image from 'next/image';

<Image src="/hero-desktop.png" 
width={1000} 
height={760} 
className="hidden md:block" 
alt="Screenshots of the dashboard project showing desktop version" />
```

"*hidden" removes the image in mobile screens, "md:block" shows it on desktops.*


# 4. Layouts and Pages

Next.js uses file-system routing. You use folders to create nested routes:

![Diagram showing how folders map to URL segments](https://nextjs.org/_next/image?url=%2Flearn%2Fdark%2Ffolders-to-url-segments.png&w=3840&q=75&dpl=dpl_EvrJe3pVoW1mxXZ5jz9LxC4S1Lpm)

![Diagram showing how adding a folder called login creates a new route '/login'](https://nextjs.org/_next/image?url=%2Flearn%2Fdark%2Frouting-solution.png&w=3840&q=75&dpl=dpl_EvrJe3pVoW1mxXZ5jz9LxC4S1Lpm)

- Each route folder can contain a `layout.tsx` and a `page.tsx`.
- `page.tsx` exports a React component. Therefore it needs to be in every route.
-  Only `pag.tsx` is routable (as well as `route.ts`). Any other file such a `button.tsx` will not have a corresponding route. This is useful for colocating your styles, components, tests etc. within its appropriate folder.
- The `/ui` and `/lib` folders are colocated within `/app`, yet there is no /ui or /lib route.
- To create a nested route, just create a folder within another folder and add a `page.tsx` file.
- Any `layout.tsx` file will apply to nested routes. For example, the `layout.tsx` inside the `dashboard` folder will also apply to `invoices` (in the structure shown in the image above.
- In Next.js, only the components update on navigation. The layout won't re-render.

![Folder structure showing the dashboard layout nesting the dashboard pages, but only the pages UI swap on navigation](https://nextjs.org/_next/image?url=%2Flearn%2Fdark%2Fpartial-rendering-dashboard.png&w=3840&q=75&dpl=dpl_EvrJe3pVoW1mxXZ5jz9LxC4S1Lpm)

- All Next.js projects require a root `layout.tsx` file
- Use this to modify `<html>` and `<body>` tags and add metadata

# 5. Navigating Between Pages

#### `next/link`

You should use the `<Link>` component instead of `<a>` as it allows for client-side navigation with JS (no requesting new files).

The attributes are the same (`<Link href="..."`).

**Prefetching**

Try switching from `<a>` to `Link`. You'll see that there is no loading required for nested routes (it's loaded as soon as the `<Link>` component enters the viewport).

**Automatic Code-Splitting**

Next.js splits your application by route segments. Therefore an error in one route won't affect another.

#### Get Current Path

```tsx
'use client';

import { usePathname } from 'next/navigation';

export default function NavLinks() {
	const pathname = usePathname();
	return ()
}
// ...
```

You can use the path to for example light up a button in a particular button when you are on the route corresponding to the button (e.g. Home button is blue when the browser is on the home page).


# 6. Database Setup

*We won't need this for Leeds Think Tank but just for future reference*

Host your Next.js application on Vercel for free, and it will re-deploy your site every time you push to master/main.

Next.js recommends Vercel Postgres (Next.js is a Vercel product).

Connect your database to Next.js by adding details to the .env file. If using Vercel Postgres then go to the Storage tab on the application hosting dashboard.

Get the Vercel Postgres SDK (allows for interaction with the database):

```shell
npm i @vercel/postgres
```

You can also interact with the database through Storage > Data in Vercel.
# 7. Data Fetching

- Fetch API - to fetch from an API *(Just doing think for LTT)*
- Direct database querying
- Create endpoints through Next.js (https://nextjs.org/docs/app/building-your-application/routing/route-handlers)

The default way to fetch data is with React Server Components. These are the default components in Next.js. 

When fetching data from a server component, make sure that it's `async` so that you can use `await`.
#### Fetch API

Use `async/await` without `useEffect`, `useState`, or Axios to fetch data from an API using the built in Fetch API.

Since server components execute on the server, you can save the client-side from expensive data fetches/logic.

```tsx
async function getArticles() {
 const response = await fetch(articles_path);
 const data = await response.json()
 return data
}


async function getArticle(id) {
 const response = await fetch(`${articles_path}`, {
   method: 'POST',
   headers: {
     'Content-Type': 'application/json',
   },
   body: JSON.stringify({ id }),
 });
 const data = await response.json();
 return data;
}
```

#### Direct Database Querying

You can also query the database directly without an API layer.

```tsx
// /app/lib/data.ts
import { sql } from '@vercel/postgres';
```

Example of fetching component given in the tutorial:

```ts
// /app/lib/data.ts
export async function fetchRevenue() {
  // Add noStore() here prevent the response from being cached.
  // This is equivalent to in fetch(..., {cache: 'no-store'}).

  try {
    // Artificially delay a response for demo purposes.
    // Don't do this in production :)

    // console.log('Fetching revenue data...');
    // await new Promise((resolve) => setTimeout(resolve, 3000));

    const data = await sql<Revenue>`SELECT * FROM revenue`;

    // console.log('Data fetch completed after 3 seconds.');

    return data.rows;
  } catch (error) {
    console.error('Database Error:', error);
    throw new Error('Failed to fetch revenue data.');
  }
}
```

The following demonstrates the benefit of querying the database directly. If you need only the 5 latest invoices from your database, then without a direct query either the API must have an endpoint that exposes this or you have to use JS to do some logic to get the latest invoices. 

Direct queries are I guess useful for small team that don't have a clear idea of where they're going as it allows you do some backend stuff on the fly.

```ts
// /app/lib/data.ts
export async function fetchLatestInvoices() {
  try {
    const data = await sql<LatestInvoiceRaw>`
      SELECT invoices.amount, customers.name, customers.image_url, customers.email, invoices.id
      FROM invoices
      JOIN customers ON invoices.customer_id = customers.id
      ORDER BY invoices.date DESC
      LIMIT 5`;

    const latestInvoices = data.rows.map((invoice) => ({
      ...invoice,
      amount: formatCurrency(invoice.amount),
    }));
    return latestInvoices;
  } catch (error) {
    console.error('Database Error:', error);
    throw new Error('Failed to fetch the latest invoices.');
  }
}
```


#### Parallel Data Fetching

Keep in mind that you may be causing:
- Request waterfall - you have to wait for one fetch to occur before the next can start
- Outdated data displayed - Next.js pre-renders so it won't show any changes in the data (Static Rendering)

Use `Promise.all()` or `Promise.allSettled()` to initiate all promises at the same time:

```js
// app/lib/data.js
export async function fetchCardData() {
  try {
    const invoiceCountPromise = sql`SELECT COUNT(*) FROM invoices`;
    const customerCountPromise = sql`SELECT COUNT(*) FROM customers`;
    const invoiceStatusPromise = sql`SELECT
         SUM(CASE WHEN status = 'paid' THEN amount ELSE 0 END) AS "paid",
         SUM(CASE WHEN status = 'pending' THEN amount ELSE 0 END) AS "pending"
         FROM invoices`;
 
    const data = await Promise.all([
      invoiceCountPromise,
      customerCountPromise,
      invoiceStatusPromise,
    ]);
    // ...
  }
}
```

But what if one request is slower than all the others?

...

# 8. Static and Dynamic Rendering

Data fetching thus far has two pitfalls:
1. Waterfall (data fetching occurs one after the other, leading to possible failure to fetch some needed data)
2. Static data (meaning no updates to the UI upon changes to the data)

#### Static Rendering (VERY IMPORTANT)

Data Fetching and rendering happens on the server at build time  (when you deploy) or revalidation. The result can then be distributed and cached in a [Content Delivery Network (CDN)](https://nextjs.org/docs/app/building-your-application/rendering/server-components#static-rendering-default).

*For LTT we can use revalidation. See below:*

```
fetch('https://...', { next: { revalidate: 3600 } })
```

or

```js
// layout.js/page.js
export const revalidate = 3600 // revalidate at most every hour
```

The cached result is served to the client upon visiting the application.

Benefits:
- Faster websites (since it's pre-rendered, there's less of a wait)
- Reduced server load (content is cached so the server doesn't generate anything on each request)
- SEO (static content is easy to index)

*BASICALLY: Next.js is amazing for static content, but since we aren't using `useEffect` etc., there no re-fetch of data and no re-render. For a site that displays articles this is fine as you can just revalidate every hour/day/whatever.*

#### Dynamic Rendering (`noStore`)

Content is rendered on the server at ***request time***

Benefits:
- Real-Time Data (frequently changing data)
- User-Specific Content (based on user input)
- Request Time Information (e.g. needs cookies or URL search parameters)

To OPT-OUT of static rendering, use `unstable_noStore`. This indicates that a particular component should not be cached. This is equivalent to:

```js
`cache: 'no-store'`
//or 
`next: { revalidate: 0 }`,
```

*Meaning that there is no stored cache at client-request time*

Usage:

```ts
// /app/lib/data

import { unstable_noStore as noStore } from 'next/cache';

export async function fetchRevenue() {
  // Add noStore() here to prevent the response from being cached.
  // This is equivalent to in fetch(..., {cache: 'no-store'}).
  noStore();
 
  // ...
}
```


*Just add it into each fetch function!*

>[!warning]
>This is an experimental API. You can use `export const dynamic = "force-dynamic"` at the top of your `layout.tsx`/`page.tsx`/`route.ts` if there are any issues. See here for more information: https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config

There is however a problem with dynamic rendering.

The whole page can be blocked by one slow data fetch.

So we have to use ***streaming***...



# 9. Streaming

Streaming is used to avoid issues of slow data fetching.

Each route is broken into smaller chunks which are streamed from the server to the client as and when they become ready.

This fixes the previous issue of one data fetch blocking a whole page from loading.

Each component can be considered a "chunk".

You can implement streaming with `loading.tsx` or `<Suspense>`.

#### `loading.tsx`

This is a special file that uses React Suspense to create a fallback UI to show as a replacement while the page content loads.

```tsx
// /app/dashboard/loading.tsx

export default function Loading() {
  return <div>Loading...</div>;
}
```

>[!note]
>Static content is shown immediately, so the page doesn't have to load completely for the client to be able to see static content. Also, the page doesn't have to load completely before the user can naviagte away (interruptible navigation).



#### Skeleten Components

In your `loading.tsx`, put a skeleton component such as `DashboardSkeleton`:


```tsx
// /app/dashboard/loading.tsx
import DashboardSkeleton from '@/app/ui/skeletons';
 
export default function Loading() {
  return <DashboardSkeleton />;
}
```

![Dashboard page with loading skeletons](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Floading-page-with-skeleton.png&w=1920&q=75&dpl=dpl_9jreweDj1d1RFNt2yi7F9bctztm3)

#### Route Groups (override loading component for nested components)

`loading.tsx` will apply to all nested components as well. If you want to override this, use Route Groups - https://nextjs.org/docs/app/building-your-application/routing/route-groups.

Create a new folder called `(overview)` and move `page.tsx` and `loading.tsx` into it:

![Folder structure showing how to create a route group using parentheses](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Froute-group.png&w=3840&q=75&dpl=dpl_9jreweDj1d1RFNt2yi7F9bctztm3)

Now `loading.tsx` will only apply to the `page.tsx` inside the `(overview)` folder.

It doesn't need to be called overview, it just need the parenthesis.

Any folder with parenthesis won't be included the url path, so:

`/dashboard/(overview)/page.tsx` become `/dashboard`.

This is used to group files logically without affecting routes.


#### Streaming Individual Components

Using `loading.tsx` in the previous way allows for streaming a whole page, but you can use React Suspense directly in order to stream specific components.

This is useful for conditional rendering.

Usage:

```tsx
import { Suspense } from 'react';

export default async function Page() {
	<>
		<Suspense fallback={<RevenueChartSkeleton />}>
			 <RevenueChart /> 
		 </Suspense>
	</>
}
```



#### Grouping Components (so that they load in together)

To avoid a popping effect where individual components pop in at different times, wrap everything you want to load at the same time in a `<Wrapper>` component and then wrap the wrapper in Suspense:

```tsx
// app/dashboard/(overview)/page.tsx
<div className="grid gap-6 sm:grid-cols-2 lg:grid-cols-4">
	<Suspense fallback={<CardsSkeleton />}>
	  <CardWrapper />
	</Suspense>
</div>
```


#### Deciding on Suspense Boundaries

Generally, move data fetching down to the components that need it, then wrap those components in Suspense.

But depending on the requirements of the application you have these three options (3rd is the general case):

- Stream a whole page with `loading.tsx`
- Stream every component individually with `<Suspense>` (but this may lead to popping)
- Stream page sections with wrapper components

Experiment!

# 10. Partial Pre-Rendering

*Experimental Feature - don't use in production*

Currently, if you call a dynamic function inside your route, then the whole route becomes dynamic.

But sometimes a route has a mix of dynamic and static components. For example:
- Social media posts are static, but likes are dynamic (go up and down constantly)
- E-commerce product details are static, but the cart is dynamic

![Diagram showing how the sidenav is static while page's children are dynamic](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Fdashboard-static-dynamic-components.png&w=3840&q=75&dpl=dpl_9jreweDj1d1RFNt2yi7F9bctztm3)

In the above dashboard, the `<SideNav>` component is static as it doesn't rely on data and is not personalised to the user. However the components in `<Page>` rely on oft-changing data so they can be dynamic.

Partial Pre-rendering is a new rendering model that renders a route with a static loading shell, while keeping some parts dynamic (isolate the dynamic parts of a route):


![Partially Prerendered Product Page showing static nav and product information, and dynamic cart and recommended products](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Fthinking-in-ppr.png&w=3840&q=75&dpl=dpl_9jreweDj1d1RFNt2yi7F9bctztm3)

When a user visits a route:
- A static route shell is served (fast initial load)
- The shell leaves holes for the dynamic content
- The async holes are loading in parallel, reducing overall load time

To use this preview feature (NOT IN PRODUCTION), see here:
- https://nextjs.org/docs/app/api-reference/next-config-js/partial-prerendering
- https://nextjs.org/learn/dashboard-app/partial-prerendering

# 10.5 Summary of Data Fetch Optimisation

In the Next.js Learn Course, we have:

1. Created a database in the same region as your application code to reduce latency between your server and database.
2. Fetched data on the server with React Server Components. This allows you to keep expensive data fetches and logic on the server, reduces the client-side JavaScript bundle, and prevents your database secrets from being exposed to the client.
3. Used SQL to only fetch the data you needed, reducing the amount of data transferred for each request and the amount of JavaScript needed to transform the data in-memory.
4. Parallelize data fetching with JavaScript - where it made sense to do so.
5. Implemented Streaming to prevent slow data requests from blocking your whole page, and to allow the user to start interacting with the UI without waiting for everything to load.
6. Move data fetching down to the components that need it, thus isolating which parts of your routes should be dynamic in preparation for Partial Prerendering.

# 11. Search and Pagination

There are three main Next.js APIs for search and pagination:
1. `searchParams`
2. `usePathname`
3. `useRouter`

#### Search

Search is based on URL search params.

Benefits over client-side state:
- Bookmarks and Shareable URLs
- SSR and Initial Load - URL parameters consumed on the server directly to render initial state
- Analytics and Tracking - Easier to track user behaviour without extra client-side logic


Next.js client hooks for search functionality:

- **`useSearchParams`**- Allows you to access the parameters of the current URL. For example, the search params for this URL `/dashboard/invoices?page=1&query=pending` would look like this: `{page: '1', query: 'pending'}`.
- 
- **`usePathname`** - Lets you read the current URL's pathname. For example, for the route `/dashboard/invoices`, `usePathname` would return `'/dashboard/invoices'`.
- 
- **`useRouter`** - Enables navigation between routes within client components programmatically. There are [multiple methods](https://nextjs.org/docs/app/api-reference/functions/use-router#userouter) you can use.

Here's a quick overview of the implementation steps:

1. Capture the user's input.
2. Update the URL with the search params.
3. Keep the URL in sync with the input field.
4. Update the table to reflect the search query.

It's a bit complex so just see here: https://nextjs.org/learn/dashboard-app/adding-search-and-pagination:

#### Handling User Input (sync search bar and url)

But here's the code for capturing input and updating/syncing the url with the following breakdown:

- `${pathname}` is the current path, in the case of the tutorial app, `"/dashboard/invoices"`.
- As the user types into the search bar, `params.toString()` translates this input into a URL-friendly format.
- The URL (in the browser) is updated by `replace...`  as part of `handleSearch()`.
- The URL is updated without reloading the page due to client-side navigation (see [[#5. Navigating Between Pages]])
- Ensure input/URL sync by passing : `defaultValue={searchParams.get('query')?.toString()}`  to the input component.

```tsx
// /app/ui/search.tsx
'use client';

import { MagnifyingGlassIcon } from '@heroicons/react/24/outline';
import { useSearchParams, usePathname, useRouter } from 'next/navigation';

export default function Search({ placeholder }: { placeholder: string }) {
  const searchParams = useSearchParams();
	const pathname = usePathname();
  const { replace } = useRouter();

  function handleSearch(term: string) {
    const params = new URLSearchParams(searchParams);
    if (term) {
      params.set('query', term);
    } else {
      params.delete('query');
    }
	replace(`${pathname}?${params.toString()}`);
  }

  return (
    <div className="relative flex flex-1 flex-shrink-0">
      <label htmlFor="search" className="sr-only">
        Search
      </label>
      <input
        className="peer block w-full rounded-md border border-gray-200 py-[9px] pl-10 text-sm outline-2 placeholder:text-gray-500"
        placeholder={placeholder}
        onChange={(e) => {
          handleSearch(e.target.value);
        }}
        defaultValue={searchParams.get('query')?.toString()}
      />
      <MagnifyingGlassIcon className="absolute left-3 top-1/2 h-[18px] w-[18px] -translate-y-1/2 text-gray-500 peer-focus:text-gray-900" />
    </div>
  );
}

```

>[!tip]
>**`defaultValue` vs. `value` / Controlled vs. Uncontrolled**
>
>If you're using state to manage the value of an input, you'd use the `value` attribute to make it a controlled component. This means React would manage the input's state.
>
>However, since you're not using state, you can use `defaultValue`. This means the native input will manage its own state. This is okay since you're saving the search query to the URL instead of state.


#### Updating a Component Based on Search Params

Page components accept the `searchParams` prop:

```tsx
export default async function Page({
  searchParams,
}: {
  searchParams?: {
    query?: string;
    page?: string;
  };
}) {
  const query = searchParams?.query || '';
  const currentPage = Number(searchParams?.page) || 1;
```

Use this to pass the query to a component displaying search items - in this case `<Table>` (and make sure it's wrapped in suspense if dynamic):

```tsx
<Suspense key={query + currentPage} fallback={<InvoicesTableSkeleton />}>
	<Table query={query} currentPage={currentPage} />
</Suspense>
```



#### When to use `useSearchParams()` vs `searchParams` prop?

- Client components can use hooks (`useSearchParams()`)
- Server components cannot use hooks, so pass the `searchParams` prop.




#### Optimise Search using Debouncing (delayed function trigger)

When the database being queried is very large, you don't want to query based on every keystroke.

Debouncing means waiting for a set period of time to end before triggering the function. It works by starting a timer on each trigger, and resetting the timer whenever the function is triggered again. The function only fires when the timer ends.

Install:

```shell
npm i use-debounce
```

Reformat the `handleSearch()` function to use the Debouncing library as follows:

```tsx
// app/ui/search.tsx
import { useDebouncedCallback } from 'use-debounce';
 
// Inside the Search Component...
const handleSearch = useDebouncedCallback((term) => {
  console.log(`Searching... ${term}`);
 
  const params = new URLSearchParams(searchParams);
  if (term) {
    params.set('query', term);
  } else {
    params.delete('query');
  }
  replace(`${pathname}?${params.toString()}`);
}, 300);

```

#### Pagination

Pagination also used URL params.

>[!warning]
>*This Warning is Not Relevant to LTT*
>*Only Relevant if Directly Querying Database*
>
>Just a reminder not to fetch data in client components as this would expose database secrets since the tutorial application is not using an API layer. Fetch data on the server and pass it to the component as a prop.

How it works (for more detail: https://nextjs.org/learn/dashboard-app/adding-search-and-pagination):

- Get the number of pages based on the query (this is manually coded):

```tsx
// app/dashboard/invoices/page.tsx
import { fetchInvoicesPages } from '@/app/lib/data';

const totalPages = await fetchInvoicesPages(query);
```

- Pass the information to the Pagination component:

```tsx
// app/dashboard/invoices/page.tsx
<Pagination totalPages={totalPages} />
```

- Go to the Pagination component itself (you design/code it) and make variables for pathname, search params, and the current page:

```tsx
// /app/ui/invoices/pagination.tsx
import { usePathname, useSearchParams } from 'next/navigation';
 
export default function Pagination({ totalPages }: { totalPages: number }) {
  const pathname = usePathname();
  const searchParams = useSearchParams();
  const currentPage = Number(searchParams.get('page')) || 1;
 
  // ...
}
```

- `createPageURL` creates an instance of the current search parameters. It then updates the "page" parameter to the provided page number, and contracts the full URL using the pathname and updated search parameters.

```tsx
// /app/ui/invoices/pagination.tsx

//

const createPageURL = (pageNumber: number | string) => {
    const params = new URLSearchParams(searchParams);
    params.set('page', pageNumber.toString());
    return `${pathname}?${params.toString()}`;
  };

//

return (
//
)
}
```

#### Reset page to 1 (important for pagination)

Just add `params.set('page', '1');`

```tsx
// app/ui/search.tsx
const handleSearch = useDebouncedCallback((term) => {
console.log(`Searching... ${term}`);
const params = new URLSearchParams(searchParams);
	params.set('page', '1');
if (term) {
  params.set('query', term);
} else {
  params.delete('query');
}
replace(`${pathname}?${params.toString()}`);
}, 300);
```



# 12. Mutating Data (using Server Actions)

*Some relevance to LTT*

Learning Objectives (there's a lot of information here so it's worth laying it all out):
- What are Server Actions and how to use them to mutate data?
- Forms and Server Components
- Best practices for working with the native formData object (+ type validation)
- How to revalidate the client cache with the `revalidatePath` API
- How to create dynamic route segments with specific IDs
- How to use React's `useFormStatus` hook for optimistic updates

#### Server Actions (New Feature)



- Allows you to run aynchronous code on the server
- No need for API endpoint to mutate data
- Can be invoked from Client or Server Components
- Enhanced safety features





#### Using Forms with Server Actions

```tsx
// Server Component
export default function Page() {
  // Action
  async function create(formData: FormData) {
    'use server';
 
    // Logic to mutate data...
  }
 
  // Invoke the action using the "action" attribute
  return <form action={create}>...</form>;
}
```

>[!tip]
>The form will work even when JavaScript is disabled on the client!

You can both mutate data and revalidate the associated cache through APIs like `revalidatePath` and `revalidateTag` (using Server Actions).

Add `"use server"` at the top of your file to mark all exported functions within the file as server functions. You can import them into Client and Server components.

For example:

```ts
// /app/lib/actions.ts
"use server"

export async function createInvoice(formData: FormData) {}
```

You can also put `"use server"` inside Server Component Server Actions (see the first example).

If you have it in a seperate file, just pass it in to the form as a prop:

```tsx
import { createInvoice } from '@/app/lib/actions';

//

return(
<>
	//
		<form action={createInvoice}>
	//
</>
)
//
```

>[!info]
>In HTML, you pass a URL to the `action` attribute, but in React `action` is a special prop. Server Actions will create a `POST` API endpoint by through the use of this prop.

Continue on with actions.ts

```ts
'use server';
 
export async function createInvoice(formData: FormData) {
  const rawFormData = {
    customerId: formData.get('customerId'),
    amount: formData.get('amount'),
    status: formData.get('status'),
  };
  // Test it out:
  console.log(rawFormData);
}
```

>[!tip]
>You can use the following for forms with many fields: `const rawFormData = Object.fromEntries(formData.entries())`

...
...
...
*We're not using Server Actions so I'm going to skip a lot. See here from for information: https://nextjs.org/learn/dashboard-app/mutating-data*

*Check out the `actions.ts` file but ignore `zod` as that's TypeScript*
#### Redirecting

```tsx
import { redirect } from 'next/navigation';

// (in a function)
redirect('/dashboard/invoices');
```



#### Dynamic Route Segments (important)

To create routes based on data, put square brackets either side of a folder name.

Examples include `[id]`, `[post]`, or `[slug]`

When linking to a particular id route just send it to a components via a prop.

```jsx
export function UpdateInvoice({ id }) {
  return (
    <Link
      href={`/dashboard/invoices/${id}/edit`}
      className="rounded-md border p-2 hover:bg-gray-100"
    >
      <PencilIcon className="w-5" />
    </Link>
  );
}
```

#### Getting the id of a Route

```jsx
const id = params.id;
// or sometimes (I can't remember why but this happened to me at some point)
const id = params.params.id;
```



# 13. Error Handling

Covers:
- `error.tsx` to catch errors in route segments and show a fallback UI to the user
- How to use `notFound` function and `not-found` file to handle 404 errors (for non-existent resources)

If using Server Actions, put try/catch statements. For example:

```tsx
// /app/lib/actions.ts
export async function createInvoice(formData: FormData) {
  const { customerId, amount, status } = CreateInvoice.parse({
    customerId: formData.get('customerId'),
    amount: formData.get('amount'),
    status: formData.get('status'),
  });
  const amountInCents = amount * 100;
  const date = new Date().toISOString().split('T')[0];

  try {
		await sql`
    INSERT INTO invoices (customer_id, amount, status, date)
    VALUES (${customerId}, ${amountInCents}, ${status}, ${date})
  `;
	} catch (error) {
		return {
      message: 'Database Error: Failed to Create Invoice.',
    };
	}

	revalidatePath('/dashboard/invoices');
  redirect('/dashboard/invoices');
}
```

`redirect` must be after the try/catch statement because is works by throwing an error.

#### `error.tsx`

Use this file to display a fallback UI for all errors in a route segment

- It needs to be a Client Component.
- It accepts 2 props:
	1. `error` - JS error object
	2. `reset` - a function that resets the error boundary (re-render the route segment)

```tsx
// /dashboard/invoices/error.tsx
'use client';
 
import { useEffect } from 'react';
 
export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  useEffect(() => {
    // Optionally log the error to an error reporting service
    console.error(error);
  }, [error]);
 
  return (
    <main className="flex h-full flex-col items-center justify-center">
      <h2 className="text-center">Something went wrong!</h2>
      <button
        className="mt-4 rounded-md bg-blue-500 px-4 py-2 text-sm text-white transition-colors hover:bg-blue-400"
        onClick={
          // Attempt to recover by trying to re-render the invoices route
          () => reset()
        }
      >
        Try again
      </button>
    </main>
  );
}
```

#### 404 Errors

`error.tsx` will kick in whenever an error occurs in a child route of the folder in which `error.tsx` lies.

However you can use a 404 error where appropriate through the `notFound` function:

```tsx
// /dashboard/invoices/[id]/edit/page.tsx
import { fetchInvoiceById, fetchCustomers } from '@/app/lib/data';
import { updateInvoice } from '@/app/lib/actions';
import { notFound } from 'next/navigation'; //THIS
 
export default async function Page({ params }: { params: { id: string } }) {
  const id = params.id;
  const [invoice, customers] = await Promise.all([
    fetchInvoiceById(id),
    fetchCustomers(),
  ]);
 
  if (!invoice) { //THIS
    notFound();   //THIS
  }               //THIS
 
  // ...
}
```

To give the `notFound` error a UI, make a `not-found.tsx` file in your route folder.

```tsx
// /dashboard/invoices/[id]/edit/not-found.tsx
import Link from 'next/link';
import { FaceFrownIcon } from '@heroicons/react/24/outline';
 
export default function NotFound() {
  return (
    <main className="flex h-full flex-col items-center justify-center gap-2">
      <FaceFrownIcon className="w-10 text-gray-400" />
      <h2 className="text-xl font-semibold">404 Not Found</h2>
      <p>Could not find the requested invoice.</p>
      <Link
        href="/dashboard/invoices"
        className="mt-4 rounded-md bg-blue-500 px-4 py-2 text-sm text-white transition-colors hover:bg-blue-400"
      >
        Go Back
      </Link>
    </main>
  );
}
```

>[!info]
>`notFound` takes precedence over `error.tsx`. Use it to handle more specific errors.

# 14. Accessibility and Forms

Learning Objectives:
- Use `eslint-plugin-jsx-a11y` for accessibility best practices
- Implement server-side form validation
- Use `useFormState` hook to handle form errors and display them to the user

#### Linting

`eslint-plugin-jsx-a11y` is built in. Just follow these steps to lint:

```json
// /package.json
"scripts": {
    "build": "next build",
    "dev": "next dev",
    "seed": "node -r dotenv/config ./scripts/seed.js",
    "start": "next start",
    "lint": "next lint"
},
```

```shell
npm run lint
```

#### Improving Form Accessibility

There are three things we're already doing to improve accessibility in our forms:

- **Semantic HTML**: Using semantic elements (`<input>`, `<option>`, etc) instead of `<div>`. This allows assistive technologies (AT) to focus on the input elements and provide appropriate contextual information to the user, making the form easier to navigate and understand.
- **Labelling**: Including `<label>` and the `htmlFor` attribute ensures that each form field has a descriptive text label. This improves AT support by providing context and also enhances usability by allowing users to click on the label to focus on the corresponding input field.
- **Focus Outline**: The fields are properly styled to show an outline when they are in focus. This is critical for accessibility as it visually indicates the active element on the page, helping both keyboard and screen reader users to understand where they are on the form. You can verify this by pressing `tab`.

These practices lay a good foundation for making your forms more accessible to many users. However, they don't address **form validation** and **errors**.



#### Client-Side Form Validation

Add `required` to `<input>` and `<select>`:

```tsx
// /app/ui/invoices/create-form.tsx
<input
  id="amount"
  name="amount"
  type="number"
  placeholder="Enter USD amount"
  className="peer block w-full rounded-md border border-gray-200 py-2 pl-10 text-sm outline-2 placeholder:text-gray-500"
  required
/>
```


##### [Server-Side validation](https://nextjs.org/learn/dashboard-app/improving-accessibility#server-side-validation)

*I would honestly skip this. Click the link if you really wanna get into it.*

Benefits:
- Ensure the format prior to sending it to the database/API
- Reduce the risk of malicious users bypassing client-side validation
- Have one source of truth for what it considered *valid* data

Inside your Form Component, the `useFormState` hook:

- Takes two arguments: `(action, initialState)`.
- Returns two values: `[state, dispatch]` - the form state, and a dispatch function (similar to [useReducer](https://react.dev/reference/react/useReducer))

```tsx
// /app/ui/invoices/create-form.tsx
'use client';

import { useFormState } from 'react-dom';

export default function Form({ customers }: { customers: CustomerField[] }) {
  const [state, dispatch] = useFormState(createInvoice, initialState);
 
  return <form action={dispatch}>...</form>;
}
```

# 15. Authentication

*As with the last few chapters, this is very Next specific, using NextAuth.js, however you can still take inspiration from it.*

#### [Setting up NextAuth.js](https://nextjs.org/learn/dashboard-app/adding-authentication#setting-up-nextauthjs)

Read the link if needed.

# 16. Adding [Metadata](https://nextjs.org/learn/dashboard-app/adding-metadata#what-is-metadata) (SEO)

Two ways:
- Config-based 
- File-based (special files)

#### [Favicon and Open Graph image](https://nextjs.org/learn/dashboard-app/adding-metadata#favicon-and-open-graph-image)

Put your `favicon.ico` and `opengraph-image.jpg` in the root of the app folder.

Next.js will automatically use these files as the app's favicon and OG image.

#### [Page title and Descriptions](https://nextjs.org/learn/dashboard-app/adding-metadata#page-title-and-descriptions)

In the root layout.tsx:

```tsx
// app/layout.tsx
import { Metadata } from 'next';
 
export const metadata: Metadata = {
  title: 'Acme Dashboard',
  description: 'The official Next.js Course Dashboard, built with App Router.',
  metadataBase: new URL('https://next-learn-dashboard.vercel.sh'),
};
 
export default function RootLayout() {
  // ...
}
```

#### Custom Title for Specific Page

Define individually:

```tsx
// /app/dashboard/invoices/page.tsx
import { Metadata } from 'next';
 
export const metadata: Metadata = {
  title: 'Invoices | Acme Dashboard',
};
```

or define a template within the root layout:

```tsx
// /app/layout.tsx
import { Metadata } from 'next';
 
export const metadata: Metadata = {
  title: {
    template: '%s | Acme Dashboard',
    default: 'Acme Dashboard',
  },
  description: 'The official Next.js Learn Dashboard built with App Router.',
  metadataBase: new URL('https://next-learn-dashboard.vercel.sh'),
};
```
```table-of-contents
style: nestedList # TOC style (nestedList|inlineFirstLevel)
maxLevel: 0 # Include headings up to the speficied level
includeLinks: true # Make headings clickable
debugInConsole: false # Print debug info in Obsidian console
```

*Note that this documentation is in TypeScript but you can use JavaScript for Next.js too (just replace `tsx` or `ts` with `jsx` and `js` respectively.*

 It is very important to read chapters 7 onwards as this is where the fundamentals really differ from `create-react-app`. For example, if your data is constantly changing, you need to use streaming as you should try to avoid `useEffect` which will negate the benefits of SSR. Streaming also has a built-in feature to display skeleton components (blank boxes).

Fetch data within the lowest level components that require them.

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

# 8. Static and Dynamic Rendering ()

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

To OPT-OUT of static rendering, use `unstable_noStore`. This indicates that a particular component should no be cached. This is equivalent to:

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


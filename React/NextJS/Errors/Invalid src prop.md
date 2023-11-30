![[Pasted image 20230214004607.png]]

You have to add in the image host like so:

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    appDir: true,
  },
	images: {
		domains: ["image.tmdb.org"]
	}
}

module.exports = nextConfig

```
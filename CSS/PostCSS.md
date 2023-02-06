December 22 - From Web Dev Simplified

**Install:**
`npm i --save-dev postcss`
`npm i --save-dev postcss postcss-cli` *to include the cli*

PostCSS is a transpiler like Babel for JavaScript. You write CSS that is incompatible with a particular browser, and you use PostCSS plugins to transpile your incompatible CSS into compatible CSS.

**Compilation:**

You need to actually compile manually using the CLI, so a little config is needed.

In `package.json` :

```js
"scripts" : {
	"build:css: "postcss src/styles.css --dir dest"
}
```

Then `npm run build:css` in order to run the CLI command above (`postcss src/style.css --  dir dest).

This will take your existing css and transpile it to a new folder called dest, with a new css file in that folder. The website will use that file for its styling.

You can add `--w` or `--watch` to the CLI command for auto-compilation:

```js
"scripts" : {
	"watch:css: "postcss src/styles.css --dir dest --watch"
}
```

Now use  `npm run watch:css`.

**Using Plugins:**

Create a `postcss.config.js` file in the directory.

Find plugins here:
[PostCSS.parts](http://www.postcss.parts)

You will require the library (plugins are just libraries) in your `postcss.config.js` file using the code from the website above (from each plugin page).

You can change presets etc. within that config. 

Every time you change the config file you need to re-run 
`npm run watch:css`.

The `preset-env` is a useful plugin that by defult uses stage 2 CSS features (this is changeable).

Run `npm i --save-dev  postcss-preset-env` in the CLI. Then add the require code.

**Use with Frameworks:**

**Vite**:
in your `postcss.config.js`, use ``import`` rather than `require`. 

You still have to use `require`, but then you have to `import` as well:

```js
import cssnano from "cssnano"
const cssnano = require("cssnano")

export default = {
	plugins: [
		cssnano({
			preset: "default",
		}),
		postcssPresetEnv({ stage: 1}),
	]
}
```
# Svelte app

This is a project template for [Svelte](https://svelte.dev) apps. It lives at https://github.com/fndvit/mvtec-google-workshop.

To create a new project based on this template using [degit](https://github.com/Rich-Harris/degit):

```bash
npx degit fndvit/mvtec-google-workshop#main YOUR_PROJECT_NAME
cd YOUR_PROJECT_NAME
```

*Note that you will need to have [Node.js](https://nodejs.org) installed.*


## Get started

Install the dependencies...

```bash
cd YOUR_PROJECT_NAME
npm install
```

...then start [Rollup](https://rollupjs.org):

```bash
npm run dev
```

Navigate to [localhost:5000](http://localhost:5000). You should see your app running. Edit a component file in `src`, save it, and reload the page to see your changes.

By default, the server will only respond to requests from localhost. To allow connections from other computers, edit the `sirv` commands in package.json to include the option `--host 0.0.0.0`.

If you're using [Visual Studio Code](https://code.visualstudio.com/) we recommend installing the official extension [Svelte for VS Code](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode). If you are using other editors you may need to install a plugin in order to get syntax highlighting and intellisense.

### Visualization components
The template has a set of (admittedly half-baked) premade graphic components:
* `Line`
* `Multiline`
* `Area`
* `Scatter`
* `ScatterCanvas` (animatable)
* `Map`
* `Mapbox`

They have (should have) a `data` prop and an `options` prop; each with their unique configurations.

For example `Line` has:

    ```svelte
    <Line 
		data={data /* Your data */}
		options={
			{
				key:{x: 'time', y: 'value'}, /* What column corresponds with the x values, which with the y values */
				format: format, /* {x: formatX, y: formatY} Number formats for x and y values  */
				color: color, /* Of the line */
				layout: 'col', /* CSS class with the size of your chart */
				title:'Title', /* A11y title */
				desc:'Description' /* A11y title */
			}
		}
	/>
    ```

For example `Map` has:

    ```svelte
    <Map 
		data={data /* Your data */}
		map={{json:world /* A topojson */, features:'countries' /* The name of the feature with the geographies */}}
		options={
			{
				scale:palette(), /* Color scheme for the choropleth */
				projection:projection, /* Map projection */
				join:{data:'id', map:'alpha3'}, /* Which variables to use to 'join' the records with the geography */
				value:'value', /* Which variable visualize */
				legend:{title: '', format: ''}, /* Title of the map legend, and number format of the ticks */
				layout:'wide' /* CSS class with the size of your chart */
                title:'Title', /* A11y title */
				desc:'Description' /* A11y title */
			}
		}
	/>
    ```

### UI
The template uses [Svelte Materialify](https://svelte-materialify.vercel.app/) so you don't need to overthink UI elements.

It also uses [Svelte Scroller](https://github.com/sveltejs/svelte-scroller) because why wouldn't you scrollytell stories.

## Building and running in production mode

To create an optimised version of the app:

```bash
npm run build
```

You can run the newly built app with `npm run start`. This uses [sirv](https://github.com/lukeed/sirv), which is included in your package.json's `dependencies` so that the app will work when you deploy to platforms like [Heroku](https://heroku.com).


## Single-page app mode

By default, sirv will only respond to requests that match files in `public`. This is to maximise compatibility with static fileservers, allowing you to deploy your app anywhere.

If you're building a single-page app (SPA) with multiple routes, sirv needs to be able to respond to requests for *any* path. You can make it so by editing the `"start"` command in package.json:

```js
"start": "sirv public --single"
```

## Using TypeScript

This template comes with a script to set up a TypeScript development environment, you can run it immediately after cloning the template with:

```bash
node scripts/setupTypeScript.js
```

Or remove the script via:

```bash
rm scripts/setupTypeScript.js
```

## Deploying to the web

### With [Vercel](https://vercel.com)

Install `vercel` if you haven't already:

```bash
npm install -g vercel
```

Then, from within your project folder:

```bash
cd public
vercel deploy --name my-project
```

### With [surge](https://surge.sh/)

Install `surge` if you haven't already:

```bash
npm install -g surge
```

Then, from within your project folder:

```bash
npm run build
surge public my-project.surge.sh
```

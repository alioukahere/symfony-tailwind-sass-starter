# symfony-tailwind-sass-starter

This repo is a starter for Symfony projects with [TailwindCSS](https://tailwindcss.com/) and [Sass](https://sass-lang.com/) pre-configured. Just clone the repo, install the dependencies and you are ready to go.

## Installation

> Before you start, make sure you have PHP, composer, Node and npm or yarn
> installed on your computer.

First of all clone this repo on your local computer with:

```bash
git clone https://github.com/alioukahere/symfony-tailwind-sass-starter.git
```

Change directory then install the PHP dependencies with `composer`:

```bash
composer install
```

Once `composer` finish its execution, you can install Javascript dependencies with `yarn`

```bash
# install dependencies with yarn
yarn

# or with npm
npm install
```

Once all dependencies have been installed, build the assets with `yarn` or `npm`:

```bash
# build with yarn
yarn encore dev

# or with npm
npm run dev
```

Then you can start your local server with `symfony` CLI:

```bash
symfony serve
```

Navigate to http://127.0.0.1:8000 and you should see this page:

![enter image description here](https://imgur.com/d3o3qEb.jpg)

And you did it! You now have a Symfony project with Tailwind and Sass configured, you can then build your great web app with the power of Symfony, TailwindCSS and Sass.

The project comes with a controller `CoreController.php` located in `src/controller/CoreController.php`, you can edit this file or delete it and create your own controller.

The `assets/styles/app.scss` is the main `Sass` file for the project, you can import your other stylesheets in there and the `assets/app.js` file is the project main `Javascript` file used in `webpack-encore`.

The `templates/base.html.twig` is the layout of the app and includes the built stylesheets and scripts.

## I don't use Sass

What if you just need TailwindCSS? Well, it's OK!

First of all, rename the file in `assets/styles/app.scss` to `assets/styles/app.css` and replace the path to your CSS file in `assets/app.js`:

```js
/*
 * Welcome to your app's main JavaScript file!
 *
 * We recommend including the built version of this JavaScript file
 * (and its CSS file) in your base layout (base.html.twig).
 */

// any CSS you import will output into a single css file (app.css in this case)
import './styles/app.css';

// Need jQuery? Install it with "yarn add jquery", then uncomment to import it.
// import $ from 'jquery';

console.log('Hello Webpack Encore! Edit me in assets/app.js');
```

Then disable the Sass loader in your `webpack.config.js`:

```js
var Encore = require('@symfony/webpack-encore');

// Manually configure the runtime environment if not already configured yet by the "encore" command.
// It's useful when you use tools that rely on webpack.config.js file.
if (!Encore.isRuntimeEnvironmentConfigured()) {
    Encore.configureRuntimeEnvironment(process.env.NODE_ENV || 'dev');
}

Encore
    // directory where compiled assets will be stored
    .setOutputPath('public/build/')
    // public path used by the web server to access the output path
    .setPublicPath('/build')
    // only needed for CDN's or sub-directory deploy
    //.setManifestKeyPrefix('build/')

    /*
     * ENTRY CONFIG
     *
     * Add 1 entry for each "page" of your app
     * (including one that's included on every page - e.g. "app")
     *
     * Each entry will result in one JavaScript file (e.g. app.js)
     * and one CSS file (e.g. app.css) if your JavaScript imports CSS.
     */
    .addEntry('app', './assets/app.js')
    //.addEntry('page1', './assets/page1.js')
    //.addEntry('page2', './assets/page2.js')

    // When enabled, Webpack "splits" your files into smaller pieces for greater optimization.
    .splitEntryChunks()

    // will require an extra script tag for runtime.js
    // but, you probably want this, unless you're building a single-page app
    .enableSingleRuntimeChunk()

    /*
     * FEATURE CONFIG
     *
     * Enable & configure other features below. For a full
     * list of features, see:
     * https://symfony.com/doc/current/frontend.html#adding-more-features
     */
    .cleanupOutputBeforeBuild()
    .enableBuildNotifications()
    .enableSourceMaps(!Encore.isProduction())
    // enables hashed filenames (e.g. app.abc123.css)
    .enableVersioning(Encore.isProduction())

    // enables @babel/preset-env polyfills
    .configureBabelPresetEnv((config) => {
        config.useBuiltIns = 'usage';
        config.corejs = 3;
    })

    // enables Sass/SCSS support
    // .enableSassLoader()

    // enables PostCSS
    .enablePostCssLoader()

    // uncomment if you use TypeScript
    //.enableTypeScriptLoader()

    // uncomment to get integrity="..." attributes on your script & link tags
    // requires WebpackEncoreBundle 1.4 or higher
    //.enableIntegrityHashes(Encore.isProduction())

    // uncomment if you're having problems with a jQuery plugin
    //.autoProvidejQuery()

    // uncomment if you use API Platform Admin (composer req api-admin)
    //.enableReactPreset()
    //.addEntry('admin', './assets/admin.js')
;

module.exports = Encore.getWebpackConfig();
```

You just have to comment the `.enableSassLoader()`. Finally remove the `node-sass` and `sass-loader` libraries with `npm` or `yarn`:

```bash
yarn remove sass-loader node-sass

# or with npm
npm remove sass-loader node-sass
```

And you are all set!

You can enjoy your new Symfony project.

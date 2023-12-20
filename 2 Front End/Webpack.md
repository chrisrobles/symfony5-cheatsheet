# Webpack

Instead of traditional `<link>` and `<script>`, use a Node library called **Webpack**

- Helps for working with a frontend framework like React or Vue
- Industry-standard tool for managing your frontend assets 
- Useful for combining and minifying CSS and JS files

## Symfony Integration

Made easy with the Symfony *Webpack Encore* bundle

[Tutorial](https://symfonycasts.com/screencast/webpack-encore)

1. `$ node -v` 
   - <https://nodejs.org/en/download/>
   - make sure nodejs is installed
2. `$ yarn -v` 
   - <https://classic.yarnpkg.com/en/docs/install>
   - make sure yarn is installed
   - yarn is package manager for Node
3. `$ composer require "encore:^1.8"`
   - Installs Webpack Encore bundle
     - Helps Symfony integrate with Webpack Encore
   - Adds to .gitignore
     - Ignores `node_modules/` which is like `vendor/` for Node
   - Adds `package.json` - which is like `composer.json` for Node
4. `$ yarn install`
5. `$ yarn watch`
   - shortcut for `yarn run encore dev --watch`
   - What does it do?
     - Reads `assets/`
     - Processes the files
     - Outputs a *built* version inside `public/build/`

## Install JS Libraries

Jquery
1. `$ yarn add jquery --dev`
   - equivalent to `composer require`
2. `import $ from 'jquery'`
   - in `app.js`
   - "loads" the `jquery` package we installed and *assigns* it to the `$` varaible

## Install CSS Libraries

Bootstrap
1. `$ yarn add bootstrap --dev`
2. `@import "~bootstrap";`
   - in `assets/css/app.css`
   - `~` is a way to load css from a bootstrap package in `node_modules/`
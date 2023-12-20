# Webpack

Instead of traditional `<link>` and `<script>`, use a Node library called **Webpack**

- Helps for working with a frontend framework like React or Vue
- Industry-standard tool for managing your frontend assets

Useful for:
- Combining and minifying CSS and JS files

## Symfony Integration

Made easy with the Symfony *Webpack Encore* bundle

[Tutorial](https://symfonycasts.com/screencast/webpack-encore)

1. `$ node -v` 
   - make sure nodejs is installed
2. `$ yarn -v` 
   - make sure yarn is installed
   - yarn is package manager for Node
3. `$ composer require "encore:^1.8`
   - Installs Webpack Encore bundle
   - Helps Symfony integrate with Webpack Encore
   - Adds to .gitignore
     - Ignores `node_modules/` which is like `vendor/` for Node
   - Adds `package.json` - which is like `composer.json` for Node 
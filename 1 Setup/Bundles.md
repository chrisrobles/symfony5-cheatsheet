# Bundles

Symfony calls **packages** bundles

- Usually require some setup (edit file to enable bundle, create file for config, etc.)
- Automate bundle setup with **Symfony Flex**
  - Tool to simplify install/removal of packages in Symfony apps
    - Will reference bundles' "recipes" (automated instructions to install and enable)
    - Recipes stored in the [Main recipe repository](https://github.com/symfony/recipes) or [Contrib recipe repository](https://github.com/symfony/recipes-contrib) (recipes that are guaranteed to work but associated packages could be unmaintained)
  - It's a Composer plugin that alters how require, update, and remove work
  - [Setup](https://symfony.com/doc/5.4/setup/flex.html)
  - Use:
    - `$ composer require logger` without flex, composer wont recognize this package

## Recipes

Every recipe has at least a `manifest.json` file that describes all of the "things" it should do

### Repositories

[Main](https://github.com/symfony/recipes/)
-quality control is higher

[Community](https://github.com/symfony/recipes-contrib)

## Auto Used Bundles

### Flex

Allows bundles to update the `bundles.php` file and create new files in `config/packages/` automatically during installation

- Enabled by default

Adds 2 features to Composer itself:
1. aliases
   - `$ composer require sec-checker --no-scripts`
   - instead of 
   - `$ composer require something/something-else --no-scripts`
   - allows you to download a package without knowing the name of it
2. Recipe System
   - Composer edits `composer.json` and `composer.lock` when installing a package
   - If the package has a **recipe**
     - Flex manages `symfony.lock` and `bundles.php` and creates `config/packages/security_checker.yaml`
     - May also add files, create directories, and modify files (ex. .gitignore)
   - See full list of available recipes at <https://bit.ly/flex-recipes>

### Profiler | Debug Toolbar

Debug toolbar at the bottom of webpages and ajax call pages that shows:
- status code
- controller and route
- Performance tab (speed & memory)
- Request and Response Info
- Twig calls
- Configuration
- Caching
- Debug database queries
- and more!

### dump() / dd()

Show information about a variable in the debug bar

`dd()` = `dump(); die();`

## Usable Bundles

aka Services 

Some Bundles are composed of classes with useful tools for development / building the project. These are called Services.

More can be found in [Services.md](../Back\ End/Services.md)
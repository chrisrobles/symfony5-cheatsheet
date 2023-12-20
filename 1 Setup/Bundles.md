# Bundles

Symfony packages 
(i.e. PHP libraries with integration for Symfony)

- Usually require some setup (edit file to enable bundle, create file for config, etc.)
  - Automate bundle setup with **Symfony Flex**

## Configure Bundles

You can pass configuration to a Service before using it

### See Bundle Configuration

Get class name from `bundles.php`

`$ php bin/console config:dump FrameworkBundle cache`

`$ php bin/console config:dump TwigBundle` or by alias `twig`

### Change Configuration

Configure bundle via YAML (usually), PHP, or XML

1. `$ php bin/console config:dump KnpMarkdownBundle`
   - Shows what configuration is possible
2. Get "key" of bundle (first line after comment)
   - `knp_markdown`
3. Create a bundle configuration file 
   - `config/packages/{KEY OF BUNDLE}.yaml`
4. Paste in configuration you want to change
   - Make sure there is 4 spaces as tabs

## Symfony Used Bundles

### Flex

Automate install/removal of bundles in Symfony apps
(i.e. plugin for Composer to alter how require, install, update, and remove work)

- Enabled by default

#### Setup

[Setup](https://symfony.com/doc/5.4/setup/flex.html)

#### How it works

1. Reference bundles' "recipes" (automated instructions to install and enable)
2. update the `bundles.php` file
3. create new files in `config/packages/` automatically during installation

#### Recipes

Every recipe has at least a `manifest.json` file that describes all of the "things" it should do

Common use for Recipes is an alias to make it easier to be referenced (LoggerInterfaceBundle to logger)

Recipes stored in repositories

##### Repositories

[Main](https://github.com/symfony/recipes/)
-quality control is higher

[Community](https://github.com/symfony/recipes-contrib)
- recipes that are guaranteed to work but associated packages could be unmaintained


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

#### dump() / dd()

Show information about a variable in the debug bar

`dd()` = `dump(); die();`

## Services | Developer Used Bundles

Some Bundles are composed of classes with useful tools for development / building the project. These are called Services.

### Logger

```php
use Psr\Log\LoggerInterface;
public function controllerFunc(LoggerInterface $logger)
{
    $logger->info("Message"); //put Message in the Logs tab of the Profiler
}
```

### Markdown -> HTML | KnpMarkdownBundle

### CacheInterface

```php
$parsedQuestionText = $cache->get('markdown_'.md5($questionText), function() use ($questionText, $markdownParser) {
    return $markdownParser->transformMarkdown($questionText);
});
```
1. 1st argument = unique cache key
2. 2nd argument = callback function
   1. If the key is already in the cache, the `get()` method will return the value
   2. else a "cache miss" will occur, and it will call the callback function
      1. Pass variables with `use` to put it in the function's scope  

Stored `project/var/cache/dev/pools`
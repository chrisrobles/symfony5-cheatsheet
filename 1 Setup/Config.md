# [Configuring Symfony](https://symfony.com/doc/5.4/configuration.html)

Configuration done in `config/`

```text
your-project/
├─ config/
│  ├─ packages/         Stores the configuration of every package installed
│  ├─ bundles.php       Enables/Disables packages in application
│  ├─ routes.yaml       Defines the routing config
│  └─ services.yaml     Configures the services of the service container
```

## [Configuration Format](https://symfony.com/doc/5.4/configuration.html#configuration-formats) | File Format

By default Symfony only loads files defined in YAML format

Update `src/Kernel.php` file to change format

### Format Performance Differences

There is none.

Symfony transforms all of them into PHP and caches them before running the app.

### YAML vs XML vs PHP

- YAML
  - \+ simple and readable
  - \- not all IDEs support linting
- XML
  - \+ linted by most IDEs, parsed natively by PHP
  - \- too verbose
- PHP
  - \+ create dynamic configuration w/ arrays or [ConfigBuilder](https://symfony.com/doc/5.4/configuration.html#config-config-builder)

## Config Component | Import Config Files

Loads using [Config component](https://symfony.com/doc/5.4/components/config.html)

- config component has advanced features
  - import other config files in different formats
  - find, load, combine, fill, and validate configuration values of any kind or source (YAML, XML, PHP)

- import example
  ```yaml
  # config/services.yaml
  imports:
      - { resource: 'legacy_config.php' }

      # glob expressions are also supported to load multiple files
      - { resource: '/etc/myapp/*.yaml' }

      # ignore_errors: not_found silently discards errors if the loaded file doesn't exist
      - { resource: 'my_config_file.xml', ignore_errors: not_found }
      # ignore_errors: true silently discards all errors (including invalid code and not found)
      - { resource: 'my_other_config_file.xml', ignore_errors: true }
  ```

## Configuration Parameters

Can define reused configuration values into parameters

```yaml
# config/services.yaml

parameters:
    app.arbitrary_string: ''

    app.admin_email: 'chris@email.com'

    app.some_array: ['','','']

    #base64_encode()
    app.binary_content_parameter: !!binary VGhpcyBpcyBhIEJlbGwgY2hhciAH

    app.some_constant: !php/const GLOBAL_CONSTANT
    app.some_other_constant: !php/const App\Entity\BlogPost:MAX_ITEMS

    app.defined_parameter: '%app.admin_email%' #if email has % in it you have to replace it with %%
imports:
    - { resource: %kernel.project_dir%/somefile.yaml' } #DOESNT WORK cant use parameters in imports
```

### %kernel%

`%kernel.project_dir`
root of project

## File Load Order

Configuration files loaded in this order (the lasts files can override the previous files)

1. `config/packages/*.<extension>`
2. `config/packages/<environment-name>/*.<extension>`
   - Its common for environments to be similar so, you can create symbolic links between `config/packages/<environment-name` directories to reuse the same configuration
3. `config/services.<extension>`
4. `config/services_<environment-name>.<extension>`

### Package File Load Order Example

For the `framework` package in **prod** environment:

1. `config/packages/framework.yaml`
2. `config/packages/prod/framework.yaml`
3. `config/services.yaml`
4. `config/services_prod.yaml`


## Configuration Environment

Variable status of the application so that the app can behave differently in different environments

Dev:
- Log everything
- Debug toolbar
- Local Database

Prod:
- Log only errors
- Faster
- Cache
- Production database

### Different Environment Configurations in a Single File

Use `when@<environment>`

```yaml
# config/packages/webpack_encore.yaml
webpack_encore:
    # ...
    output_path: '%kernel.project_dir%/public/build'
    strict_mode: true
    cache: false

# cache is enabled only in the "prod" environment
when@prod:
    webpack_encore:
        cache: true

# disable strict mode only in the "test" environment
when@test:
    webpack_encore:
        strict_mode: false

# YAML syntax allows to reuse contents using "anchors" (&some_name) and "aliases" (*some_name).
# In this example, 'test' configuration uses the exact same configuration as in 'prod'
when@prod: &webpack_prod #set alias
    webpack_encore:
        # ...
when@test: *webpack_prod #reference alias
```

### Create a New Environment

For a new environment called "staging"

1. Create `config/packages/staging/`
2. Add the files that you would like to change into the new directory
3. `APP_ENV=staging` in .env.local

### Set environment

```env
# .env.local
...

# Environment
APP_ENV=prod

...
```

- Override `APP_ENV` value when running console commands
  - `$ APP_ENV=prod php bin/console command_name`


## .env.*

.env.* files define the values of environment variables

- Read and parsed on every request
- its env vars are added to `$_ENV` & `$_SERVER`
- Existing env vars are *never* overwritten by .env
- Symfony Flex automatically adds to it when installing bundles

### All .env files

- `.env`: default values of the env vars needed to run, not production values
- `.env.local`: overrides `.env`, local to the machine running the app (on prod and local)
- `.env.<environment>`: overrides only for one environment but all machines (they are committed)
- `.env.<environment>.local`: overrides  for only one machine AND environment

.local is not committed / tracked

### Improve performance

```yaml
# parses ALL .env files and dumps their final values to .env.local.php
$ composer dump-env prod
```
- After running this, Symfony wll load .env.local.php vs spending time parsing .env file

## Environment Variables

Environment variables are used for 
1. setting values that are specific to an environment
   - database credentials
2. changing dynamic values to prevent from redeploying an application
   - API key
3. In *any* other cases than these it is recommended to use [configuration parameters](#configuration-parameters)


- Can only be strings
  - Type cast the values into another value with [Env Var Processors](https://symfony.com/doc/5.4/configuration/env_var_processors.html)
- Real Environment Variables always wins over those set in .env
  - Real env vars are set in the shell and web server

### Set Env Vars

`APP_CUSTOM_PARAM="custom env param"` in .env or .env.local
   
### Access Env Vars

In YAML (services.yaml)
- `%env(ENV_VAR_NAME)%`

#### In PHP

- `$_ENV['APP_ENV']` (not the best)
- `$_SESSION['APP_ENV']` (not the best)

##### In Controllers that extend `AbstractController`:

`$this->getParameters('kernel.project_dir');`
`$this->getParameters('app.admin_email');`

##### Inject the Parameters as Arguments to the Service / Controllers Constructors:

```yaml
# config/services.yaml

parameters:
    app.contents_dir: '%env(APP_CONTENTS_DIR)%'

services:
    App\Service\MessageGenerator:
        arguments:
            $contentsDir: '%app.contents_dir%'
```
```php
<?php
namespace App\Service;
class YourCustomService
{
  protected $contentsDir;
  public function __construct($contentsDir){ //custom param autowired
    $this->contentsDir = $contentsDir;
  }
}
```


##### If you inject the same parameters over and over again:

```yaml
# config/services.yaml
services:
    _defaults:
        bind:
            # pass this value to any $projectDir argument for any service
            # that's created in this file (including controller arguments)
            $projectDir: '%kernel.project_dir%'

    # ...
```

##### Inject All Parameters

```php
// src/Service/MessageGenerator.php
namespace App\Service;

// ...

use Symfony\Component\DependencyInjection\ParameterBag\ContainerBagInterface;

class MessageGenerator
{
    private $params;

    public function __construct(ContainerBagInterface $params)
    {
        $this->params = $params;
    }

    public function someMethod()
    {
        // get any container parameter from $this->params, which stores all of them
        $sender = $this->params->get('mailer_sender');
        // ...
    }
}
```

### Default env vars

Define a parameter with the same name as the env var

```yaml
# config/packages/framework.yaml
parameters:
    # if the SECRET env var value is not defined anywhere, Symfony uses this value
    env(SECRET): 'some_secret'
```

### Reference another ENV VAR

- `DB_PASS=${DB_USER}pass #use the user as a password prefix`
- the ENV VAR being used must be defined first
  - if it was defined first in another file that was loaded first that will be the one referenced instead
- Define default value to handle if its not defined
  - `DB_PASS=${DB_USER:-root}pass`

### Embed Commands

`START_TIME=$(date)`

### Encrypt the value as secret

If the value is sensitive, encrypt using the [secrets management systems](https://symfony.com/doc/5.4/configuration/secrets.html)

Requires sodium PHP extension that is bundled with PHP 7.2 or use the [package](https://github.com/paragonie/sodium_compat)

## Security

- DONT DUMP `$_SERVER` or `$_ENV` or `phpinfo()` it will show database credentials
- The [Symfony Profiler](https://symfony.com/doc/5.4/profiler.html) (the debug toolbar) must never be enabled in production
  - It shows env vars and other sensitive things

## Commands

### Understand how Symfony Parses the different .env files

`$ php bin/console debug:dotenv`

### List Environment Variables

`$ php bin/console --env-vars`

### List Configuration Parameters

`$ php bin/console debug:container --parameters`

## PHP ConfigBuilder

Object that will help build the PHP config's large nested arrays

- Symfony generates the `ConfigBuilder` classes automatically in the [kernel build directory](https://symfony.com/doc/5.4/reference/configuration/kernel.html#configuration-kernel-build-directory) for all the bundles installed.
- Only root classes in the namespace `Symfony\Config` are ConfigBuilders
  - Nested configs (`\Symfony\Config\Framework\CacheConfig`) are regular PHP objects which arent autowired

```php
// config/packages/security.php

use Symfony\Config\SecurityConfig; //by convention all live in the Symfony\Config namespace

return static function (SecurityConfig $security) {
    $security->firewall('main')
        ->pattern('^/*')
        ->lazy(true)
        ->anonymous();

    $security
        ->roleHierarchy('ROLE_ADMIN', ['ROLE_USER'])
        ->roleHierarchy('ROLE_SUPER_ADMIN', ['ROLE_ADMIN', 'ROLE_ALLOWED_TO_SWITCH'])
        ->accessControl()
            ->path('^/user')
            ->role('ROLE_USER');

    $security->accessControl(['path' => '^/admin', 'roles' => 'ROLE_ADMIN']);
};
```


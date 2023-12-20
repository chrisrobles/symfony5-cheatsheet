# [Routing](https://symfony.com/doc/5.4/routing.html)

Maps incoming URLs to a controller that uses the Request to generate a response

YAML
```yaml
blog_list: #name
    path: /blog # URL/PATH/{PARAMS}
    controller: App\Controller\BlogController::list
    option:
    option:
```

Annotations
`$ composer require annotations`
```php
namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\Routing\Annotation\Route;

class BlogController extends AbstractController
{
    /**
     * Route("/URL/PATH/{param}/PATH", name="", options="")
     * @Route("/blog", name="blog_list")
     */
    public function list(): Response  // myFunction(string $param): Response {}
    {
        // ...
    }
}
```

## routes.yaml

`routes.yaml` can be configured in YAML, XML, PHP 

### YAML vs Annotations

- Performance and Features are the same for using a yaml file or attributes / annotations 
- Symfony **recommends** using attributes or annotations for simplification and readability.

#### PHP version

- PHP 7 <= ? annotations `$ composer require doctrine/annotations`
- PHP 8 ? attributes

#### Alternative File Formats to YAML

- Symfony only loads the routes defined in YAML format
- If using XML and/or PHP formats, you need to [update the src/Kernel.php file](https://symfony.com/doc/5.4/configuration.html#configuration-formats)

---

## Path

url

`/clients/{id}/invoices`

- no trailing slash
- no file extension


---

## Parameters

Variable parts of URL wrapped in `{ }`

`/blog/posts-about-{category}/page/{pageNumber}`


- *aka Route Parameters / Wildcards / Placeholders / Request Parameters / Request Attributes*
- Can only use a parameter once in route
- the 'app_' prefix is recommended to better differentiate your parameters from Symfony parameters

### Special Characters in Parameter

#### Allow / in parameter (escape /)

add to `requirements:` `token: .+`

- only the last parameter should be allowed to have a `/` in it (otherwise the last parameter will only get everything after the last /)

#### token._format | file.extension

- if using `{token}.{_format}`
  - token shouldnt accept all characters because it will accept the `.`
  - instead, replace `requirements:` `.+` with `[^.]+` to allow any character except dots

### Parameter Conversion

Converts route parameters into objects using built-in converters

```php
public function myControllerAction(Request $request, Timecard $timecard): Response {}
```

- *example of Type Wiring*

#### Doctrine Converter

Convert route parameter to Entity (via doctrine.orm)

```php
// user/{id}
public function view(User $user)
```

- Wildcards' name determines how `$user` is fetched
  - `/user/{id}` 
    - fetch User via primary key with `find()`
  - `/user/{property_name}` 
    - fetch User via entity property `findBy({property_name})`
    - if wildcard != entity property, it is ignored
    - *all* wildcards in route will be tried
    - any wildcard besides `{id}` will use `findBy()`
  - `/user/{not_entity_property}` 
    - fetch by specifying an expression in `@Entity` (shortcut for `@ParamConverter`)
      ```php
      use Sensio\Bundle\FrameworkExtraBundle\Configuration\Entity;

      /**
      * @Route("/blog/{post_id}")
      * @Entity("post", expr="repository.find(post_id)")
      * ^ Has all the same options as @ParamConverter
      */
      public function show(Post $post){}
      ```
  - Use both Automatic and Expression fetching
    ```php
    /**
     * @Route("/blog/{id}/comments/{comment_id}")
     * @Entity("comment", expr="repository.find(comment_id)"
     * Both cant be automatic
     */
    public function show(Post $post, Comment $comment) {}
    ```

##### Options for @Entity / @ParamConverter

`* @Entity("post", ... , options={"id" = "post_id"})`
- `id`
  - if `post_id` is a route parameter, converter will fetch by `find({post_id})`
  - `find()` searches the entity's primary key
- `mapping`
  - configure properties and values for converter to plug into `findOneBy()` to fetch entity with
  ```php
  * @Route("/blog/{date}/{slug}/comments/{comment_slug}")
  * @ParamConverter("post", options={"mapping": {"date": "date", "slug": "slug"}})
  * @ParamConverter("comment", options={"mapping": {"comment_slug": "slug"}})
  ```
- `exclude`
  - Configure the properties used by `findOneBy()` by excluding properties
  - Example
    - "/blog/{date}/{slug}"
    - `options={"exclude": {"date"}}` will use {slug}
- `strip_null`
  - If true, then when `findOneBy()` is used, any values that are `null` will not be used
- `entity_manager`
  - Set one different from the default entity manager
- `evict_cache`
  - Force Doctrine to always fetch from the database instead of cache

#### DateTime Converter

Convert any route parameter into a DateTime instance

```php
/**
 * @Route("/blog/archive/{start}/{end}")
 */
public function blogArchive(\DateTime $start, \DateTime $end){}`
```

- Format 
  - `@ParamConverter("start", options={"format": "!Y-m-d"})`

#### Explicitly Disable Converters

```php
# config/packages/sensio_framework_extra.yaml
sensio_framework_extra:
    request:
        converters: true
        disable: ['doctrine.orm', 'datetime']
```

#### [Custom Converter](https://symfony.com/bundles/SensioFrameworkExtraBundle/current/annotations/converters.html#creating-a-converter)

### Symfony Usable Parameters

- `_controller`
  - determine which controller and action (function) is executed when the route is matched
- `_format`
  - set the "request format" of the `Request object`
    - ex. setting the `Content-Type` of the response \
    - (e.g. `json` format translates into a `Content-Type` of `application/json`)
- `_fragment`
  - used to set the fragment identifier (`#` at the end of URLs)
- `_locale`
  - used to set the locale on the request


---

## Name

- Each route name must be unique across the entire application
- app_controller_action (app_lucky_number) would be autogenerated for the name if not specified

---

## Controller

- If the action is implemented as the __invoke() method of the controller class
  - skip the '::method_name' part
  - `controller: App\Controller\BlogController`

---

## Options

### method: (Matching HTTP Methods)

- Routes match any HTTP verb (GET, POST, PUT, HEAD, etc.) usually
- Specify the HTTP verbs to match to with method option

YAML
```php
# config/routes.yaml
api_post_show:
    path:       /api/posts/{id}
    controller: App\Controller\BlogApiController::show
    methods:    GET|HEAD

api_post_edit:  # still different names
    path:       /api/posts/{id}
    controller: App\Controller\BlogApiController::edit  # and different controllers
    methods:    PUT
```

Annotations
```php
/**
* @Route("/api/posts/{id}", methods={"GET", "HEAD"})
 */
...

/**
* @Route("/api/posts/{id}", methods={"PUT"})
 */
...
```

### condition: (Route Validation | Matching Expressions)

Match the route only if an expression is true
- Uses [expression language](https://symfony.com/doc/5.4/reference/formats/expression_language.html) syntax in yaml and php
  - Compiles down to raw php, so no extra overhead
- Conditions not taken into account when generating URLs
- Can use the variables created in Symfony
  - `context`
    - An instance of [RequestContext](https://github.com/symfony/symfony/blob/5.4/src/Symfony/Component/Routing/RequestContext.php), which holds the most fundamental information about the route being matched.
  - `request`
    - The [Symfony Request](https://symfony.com/doc/5.4/components/http_foundation.html#component-http-foundation-request) object that represents the current request
  - `env(string $name)` function
    - returns the value of a variable using [Environment Variable Processors](https://symfony.com/doc/5.4/configuration/env_var_processors.html)

YAML
```yaml
# config/routes.yaml
contact:
    path:       /contact
    controller: 'App\Controller\DefaultController::contact'
    condition:  "context.getMethod() in ['GET', 'HEAD'] and request.headers.get('User-Agent') matches '/firefox/i'"
    # expressions can also include configuration parameters:
    # condition: "request.headers.get('User-Agent') matches '%app.allowed_browsers%'"
    # expressions can even use environment variables:
    # condition: "context.getHost() == env('APP_MAIN_HOST')"
```

Annotations
```php
// src/Controller/DefaultController.php
namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class DefaultController extends AbstractController
{
    /**
     * @Route(
     *     "/contact",
     *     name="contact",
     *     condition="context.getMethod() in ['GET', 'HEAD'] and request.headers.get('User-Agent') matches '/firefox/i'"
     * )
     *
     * expressions can also include configuration parameters:
     * condition: "request.headers.get('User-Agent') matches '%app.allowed_browsers%'"
     */
    public function contact(): Response
    {
        // ...
    }
} 
```

### requirements: (Parameters Validation)

Type validation for parameters
- Symfony uses the route that was defined first, so a path can be overloaded if it has different requirements
- Uses PHP regular expression
- Inline `/blog/{page<\d+>}`

YAML
```yaml
...
  requirements:
      page: '\d+'  #int
...
```

Annotations
```php
/**
* @Route("/blog/{page}", name="blog_list", requirements={"page"="\d+"})
*/
...
/**
* @Route("/blog/{slug}", name="blog_show")
*/
...
```

### defaults: (Optional Parameters)

Sets a default value for parameters, making them optional
- i.e. Map `/blog` to same result as `/blog/1`

- `/blog` will still be the generated URL
  - `{!page}` will force the generated URL to include the default parameter
- Everything after a default parameter must be optional
  - GOOD `/blog/{slug}/{page}` (slug and page optional)
  - BAD `/{page}/blog`
- Inline with path for default values
  - `{parameter_name?default_value}`
  - `/blog/{page<\d+>?1}` for both Annotations and YAML
- Default to null? `{page?}`
  - If you do this, make sure to update the controller arguments to allow passing null
  - `?int $page`

Annotations

```php
/**
* @Route("/blog/{page}", name="blog_list", requirements={"page"="\d+"})
*/
public function list(int $page = 1): Response   // default through parameter
{
```

YAML (default option)

```yaml
    defaults:
        page: 1
```

#### Extra Parameters / Variables

In the `defaults` option of a route, you can define parameters that dont exist in the route to pass extra arguments to the controllers

```php
/**
 * @Route("/blog/{page}", name="blog_index", defaults={"page": 1, "title": "Hello world!"})
 */
```

### priority: (Parameter Priority)

In YAML, the route that was defined first gets priority.

In Annotations, you need to use the priority option to set priorities.

```php
/**
* This route could not be matched without defining a higher priority than 0. 0 is default.
*
* @Route("/blog/list", name="blog_list", priority=2)
*/
```

### alias:

YAML
`alias:`

Annotations
- nothing mentioned

#### alias deprecating

```YAML
new_route_name:
    alias: original_route_name

    # this outputs the following generic deprecation message:
    # Since acme/package 1.2: The "new_route_name" route alias is deprecated. You should stop using it, as it will be removed in the future.
    deprecated:
        package: 'acme/package'
        version: '1.2'

    # you can also define a custom deprecation message (%alias_id% placeholder is available)
    deprecated:
        package: 'acme/package'
        version: '1.2'
        message: 'The "%alias_id%" route alias is deprecated. Do not use it anymore.'
```

---

## How to

### See all routes

`$ php bin/console debug:router`

### See route details

`$ php bin/console debug: router app_lucky_number`

Pass the name (or part of the name) of some route to this argument to print the route details:

### See which route will match URL

```terminal
$ php bin/console router:match /lucky/number/8

[OK] Route "app_lucky_number" matches
```


### Generate URLS

Get the URL of a route with the name (in controller)

Pros
- Define url path in **one location**
  - Allows you to not hardcode urls in `<a href="...">`
  - If the URL of some route changes, all links will be updated
- Can generate from Controllers, Services, Templates, Javascript, Commands
- Can force HTTPS on generated urls

Warnings
- Conditions not taken into account when generating URLs

### Group Routes / Prefixes ?

Most routes share a prefix (`/blog`), so its common to make them share some configurations

YAML
```YAML
# config/routes/annotations.yaml
controllers:
    resource: '../../src/Controller/'
    type: annotation
    # this is added to the beginning of all imported route URLs
    prefix: '/blog'
    # this is added to the beginning of all imported route names
    name_prefix: 'blog_'
    # these requirements are added to all imported routes
    requirements:
        _locale: 'en|es|fr'

    # An imported route with an empty URL will become "/blog/"
    # Uncomment this option to make that URL "/blog" instead
    # trailing_slash_on_root: false

    # you can optionally exclude some files/subdirectories when loading annotations
    # exclude: '../../src/Controller/{DebugEmailController}.php'
```

Annotations
```php
/**
 * @Route("/blog", requirements={"_locale": "en|es|fr"}, name="blog_")
 * These configurations will apply to every route below
 */
class BlogController extends AbstractController
{
    /**
     * @Route("/{_locale}", name="index")
     */
    public function index(): Response
    {
        // ...
    }

    /**
     * @Route("/{_locale}/posts/{slug}", name="show")
     */
    public function show(string $slug): Response
    {
        // ...
    }
  ///...
}
```

### Custom Route Loader ?

[How to Create a custom Route Loader ?](https://symfony.com/doc/5.4/routing/custom_route_loader.html)

### Getting the Route Name and Parameters in Controller

`Request` object stores all the route configs found in the "request attributes"

### Render a Template Directly from a Route

```yaml
# config/routes.yaml
acme_privacy:
    path:          /privacy
    controller:    Symfony\Bundle\FrameworkBundle\Controller\TemplateController
    defaults:
        # the path of the template to render
        template:  'static/privacy.html.twig'

        # the response status code (default: 200)
        statusCode: 200

        # special options defined by Symfony to set the page cache
        maxAge:    86400
        sharedAge: 86400

        # whether or not caching should apply for client caches only
        private: true

        # optionally you can define some arguments passed to the template
        context:
            site_name: 'ACME'
            theme: 'dark'
```



### Redirect to URLS and Routes Directly from a Route 

Use the `defaults` option and add `route`

```yaml
# config/routes.yaml
doc_shortcut:
    path: /doc
    controller: Symfony\Bundle\FrameworkBundle\Controller\RedirectController
    defaults:
        route: 'doc_page'
        # optionally you can define some arguments passed to the route
        page: 'index'
        version: 'current'
        # redirections are temporary by default (code 302) but you can make them permanent (code 301)
        permanent: true
        # add this to keep the original query string parameters when redirecting
        keepQueryParams: true
        # add this to keep the HTTP method when redirecting. The redirect status changes
        # * for temporary redirects, it uses the 307 status code instead of 302
        # * for permanent redirects, it uses the 308 status code instead of 301
        keepRequestMethod: true
        # add this to remove the original route attributes when redirecting
        ignoreAttributes: true

legacy_doc:
    path: /legacy/doc
    controller: Symfony\Bundle\FrameworkBundle\Controller\RedirectController
    defaults:
        # this value can be an absolute path or an absolute URL
        path: 'https://legacy.example.com/doc'
        permanent: true
```
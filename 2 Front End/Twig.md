# Twig

[Docs](https://twig.symfony.com/doc/3.x/) | [Symfony Docs](https://symfony.com/doc/5.4/templates.html)

Template language (like JS)

- `{{ ... }}`: "show something", print
- `{% ... %}`: "do something", logic tag, run logic statement (multi line)
- `{# ... #}`: comments (not rendered)

## Tips

- Extend base.html.twig for all **default HTML elements**
- Cant run PHP inside Twig templates
- Templates are cached in `prod` environment and recompiled each time in `dev`

## How 2s

- Reference static CSS, JS, or image file
  - `href="{{ asset('css/app.css') }}"`
- Generate URL
  - Get route name `$ php bin/console debug:router`
  - `path('route_name')`

## Common Syntax

```twig
{# comment #}

{% set foo = 'bar' %}

{% for k, v in list %}
    {{ k }}: {{ v }} 
{% else %}
    No items in list.
{% endfor %}

{% if item is defined and item or item.price > 0%}
    {{ item|date('Y-m-d') }}
{% endif %}
```

## Example

```php
//src/Controller/myController.php
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController; //render()
//...
return $this->render('MyClass/notifications.html.twig', [
    'user_first_name' => $userFirstName,
    'notifications' => $userNotifications,
]);  //returns a Response object
```
```twig
{# templates/user/notifications.html.twig #}
<h1>Hello {{ user_first_name }}!</h1>
<p>You have {{ notifications|length }} new notifications.</p>
```
---

## Render

### In Controllers

if controller extends `AbstractController`, use the `render()` helper

```php
public function index(): Response
{
    //render() returns a `Response` object with contents created by template
    return $this->render('product/index.html.twig', [
        'category' => '...',
        'promotions' => ['...', '...'],
    ]);

    //renderView() method only returns the contents created by the template
    $contents = $this->renderView('product/index.html.twig', [
        'category' => '...',
        'promotions' => ['...', '...'],
    ]);

    return new Response($contents);
}
```

### In Services

Can use Twig Service in custom service

```php
use Twig\Environment;
...
  private $twig;

  public function __construct(Environment $twig){
    $this->twig = $twig;
  }
```

#### Check if exists before render

```php
use Twig\Environment;
...
public function __construct(Environment $twig){
  $loader = $twig->getLoader();

  if($loader->exists('theme/layout_responsive.html.twig')) {
    //template exists
  }
}
```

### In Emails
[In Emails](https://symfony.com/doc/5.4/templates.html#rendering-a-template-in-emails)

### In Route

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

### In Twig | Reuse

#### Including Templates

1. Create template file
   - `{# blog/_user_profile.html.twig #}` `_` prefix shows its a template fragment
2. `{{ include('blog/_user_profile.html.twig') }}`
   - can pass variables too `{{ include(path, {user: blog_post.author}) }}`

#### Embedding Controllers

Embed the result of executing a controller

- Needed when a database query is needed
- Can have a significant impact on the application performance if you embed lots of controllers
  - [Cache the template fragment to fix](https://symfony.com/doc/5.4/http_cache/esi.html)

1. Create controller that renders
  `return $this->render(templates/blog/_recent_articles.html.twig, [...])`
2. Create template fragment
  `templates/blog/_recent_articles.html.twig`
3. Call the controller from template
    ```twig
    {# call by route #}
    {{ render(path('latest_articles', {max: 3})) }} {# relative #}
    {{ render(url('latest_articles', {max: 3})) }} {# absolute #}

    {# call by controller #}
    {{ render(controller('App\\Controller\\BlogController::recentArticles', {max: 3})) }}
    ```

#### Render [async](https://symfony.com/doc/5.4/templates.html#how-to-embed-asynchronous-content-with-hinclude-js)

Embed asynchronously w/ `hinclude.js` JavaScript library

1. Include the hinclude.js library in your page
2. Render with hinclude
`{{ render_hinclude(url(...)) }}`
`{{ render_hinclude(controller(...)) }}`

- When Javascript is disabled / takes a long time to load, display a default content rendering some template
  ```yaml
  # config/packages/framework.yaml
  framework:
    #...
    fragments:
      hinclude_default_template: hinclude.html.twig
  ```
  - Or define per render (can put a template or string)
    ```twig
    {{ render_hinclude(controller(...), {
      default: 'default/content.html.twig'
    }) }}
    ```

#### Template Inheritance | Layouts

When pages share a common structure, its better to use **inheritance**

- Page sections are put in `{% block section %}`
  - Can be empty
  - Child templates
    - can override `{% block section %}`
    - can append `{% block section %} {{ parent() }} {% endblock %}`
    - can extend (by not redefining the block)
  - When using `extends`, all code must be in a `block` tag

**Recommended Hierarchy:**
- `templates/base.html.twig` = common elements of all templates
  - `<head>, <header>, <footer>, ...`
  ```twig
  {# templates/base.html.twig #}
  <!DOCTYPE html>
  <html>
      <head>
          <meta charset="UTF-8">
          <title>{% block title %}My Application{% endblock %}</title>
          {% block stylesheets %}
              <link rel="stylesheet" type="text/css" href="/css/base.css"/>
          {% endblock %}
      </head>
      <body>
          {% block body %}
              <div id="sidebar">
                  {% block sidebar %}
                      <ul>
                          <li><a href="{{ path('homepage') }}">Home</a></li>
                          <li><a href="{{ path('blog_index') }}">Blog</a></li>
                      </ul>
                  {% endblock %}
              </div>

              <div id="content">
                  {% block content %}{% endblock %}
              </div>
          {% endblock %}
      </body>
  </html>
  ```
- `templates/layout.html.twig` = extends base and defines the content structure used in all/most pages
  - ex. two-column content + sidebar layout
  - Some sections of the application can define their own layout (`templates/blog/layout.html.twig`)
  ```twig
  {# templates/blog/layout.html.twig #}
  {% extends 'base.html.twig' %}

  {% block content %}
      <h1>Blog</h1> {# leave out if generic layout #}

      {% block page_contents %}{% endblock %}
  {% endblock %}
  ```
- `templates/*.html.twig` = extends layout and is the application page
  ```twig
  {# templates/blog/index.html.twig #}
  {% extends 'blog/layout.html.twig' %}

  {% block title %}Blog Index{% endblock %}

  {% block page_contents %}
      {% for article in articles %}
          <h2>{{ article.title }}</h2>
          <p>{{ article.body }}</p>
      {% endfor %}
  {% endblock %}
  ```

## Functions

`{{ func() }}`

- Tips
  - Named arguments are supported
  - chain functions with a pipe
    - `{{ include('template.html')|upper }}`

### Functions Documentation

#### Twig Functions

- [attribute()](https://twig.symfony.com/doc/3.x/functions/attribute.html) - access a "dynamic" attribute of a variable
    ```twig
    {{ attribute(object, method) }}
    {{ attribute(object, method, arguments) }}
    {{ attribute(array, item) }}
    ```

- [block()](https://twig.symfony.com/doc/3.x/functions/block.html) - reprint a block (defined from `{% block name="" %}`)

  `{{ block('title') }}`

  `{{ block('title', 'common_blocks.twig') }}`

  `{% if block("footer", "common_blocks.twig") is defined %}`

- [constant()](https://twig.symfony.com/doc/3.x/functions/constant.html) - returns the constant value for a given string

- [cycle()](https://twig.symfony.com/doc/3.x/functions/cycle.html)- cycles through array values

- [date()](https://twig.symfony.com/doc/3.x/functions/date.html) - converts an argument to a date
  - Use case example would be to allow date comparison

- [dump()](https://twig.symfony.com/doc/3.x/functions/dump.html) - dumps info about a template variable

- [html_classes()](https://twig.symfony.com/doc/3.x/functions/html_classes.html) - conditionally join class names

- [include()](https://twig.symfony.com/doc/3.x/functions/include.html) - include another template

  - Better than include tag
  - Allow you to pipe other functions `{{ include('template.html')|upper }}`

- [max()](https://twig.symfony.com/doc/3.x/functions/max.html) - returns biggest value of sequence

- [min()](https://twig.symfony.com/doc/3.x/functions/min.html) - returns lowest value of sequence

- [parent()](https://twig.symfony.com/doc/3.x/functions/parent.html) - render the contents of the parent block

- [random()](https://twig.symfony.com/doc/3.x/functions/random.html) - random value depending on the supplied parameter type

- [range()](https://twig.symfony.com/doc/3.x/functions/range.html) - returns a list containing an arithmetic progression of integers

- [source()](https://twig.symfony.com/doc/3.x/functions/source.html) - returns the content of a template w/o rendering it

- [country_timezones()](https://twig.symfony.com/doc/3.x/functions/country_timezones.html)

- [country_names()](https://twig.symfony.com/doc/3.x/functions/country_names.html)

- [currency_names()](https://twig.symfony.com/doc/3.x/functions/currency_names.html)

- [language_names()](https://twig.symfony.com/doc/3.x/functions/language_names.html)

- [locale_names()](https://twig.symfony.com/doc/3.x/functions/locale_names.html)

- [script_names()](https://twig.symfony.com/doc/3.x/functions/script_names.html) - returns the names of the scripts

- [timezone_names()](https://twig.symfony.com/doc/3.x/functions/timezone_names.html)

- [template_from_string()](https://twig.symfony.com/doc/3.x/functions/template_from_string.html) - loads a template from a string

#### [Symfony Functions](https://symfony.com/doc/5.4/reference/twig_reference.html)

- [absolute_url()](https://twig.symfony.com/doc/3.x/functions/absolute_url.html) - absolute version of `asset()`

- [asset()](https://twig.symfony.com/doc/3.x/functions/asset.html) - get the relative path of a static CSS, JavaScript or Image asset
  - if a template needs to link a static asset, use `asset()`
  - need `$ composer require symfony/asset`
  - Building, Versioning and Minifying in a modern way w/ [Symfony's Webpack Encore](https://symfony.com/doc/5.4/frontend.html)
  - Useful when switching to a CDN (adding one line to one config file) wont need to update all the static file links

- [asset_version()](https://twig.symfony.com/doc/3.x/functions/asset_version.html) - current version of the package

- [controller()](https://twig.symfony.com/doc/3.x/functions/controller.html) - returns instance of ControllerReference

- [csrf_token()](https://twig.symfony.com/doc/3.x/functions/csrf_token.html)

- [expression()](https://twig.symfony.com/doc/3.x/functions/expression.html) - symfony expression

- [form()](https://twig.symfony.com/doc/3.x/functions/form.html) - renders HTML of a complete form

- [form_end()](https://twig.symfony.com/doc/3.x/functions/form_end.html) - end tag of a form & outputs form_rest

- [form_errors()](https://twig.symfony.com/doc/3.x/functions/form_errors.html) - renders any errors for the given field

- [form_help()](https://twig.symfony.com/doc/3.x/functions/form_help.html) - renders the help text for the given field

- [form_label()](https://twig.symfony.com/doc/3.x/functions/form_label.html) - renders the label for the given field

- [form_parent()](https://twig.symfony.com/doc/3.x/functions/form_parent.html) - returns the parent form view or null if root

- [form_rest()](https://twig.symfony.com/doc/3.x/functions/form_rest.html) - renders all fields that have not yet been rendered
  - good idea to always have for hidden fields or fields that were forgotten to be rendered

- [form_row()](https://twig.symfony.com/doc/3.x/functions/form_row.html) - renders the row of field (label, errors, help, and widget)

- [form_start()](https://twig.symfony.com/doc/3.x/functions/form_start.html) - renders the start tag of form
  - takes care of printing the configured method and target action of the form.
  - also includes the correct enctype property if the form contains upload fields

- [form_widget()](https://twig.symfony.com/doc/3.x/functions/form_widget.html) - renders the HTML widget of a given field

- Form Functions and Variables Reference

```twig    
    {{ form_start(form) }}
    {{ form_errors(form) }}

    {{ form_row(form.name) }}
    {{ form_row(form.dueDate) }}

    {{ form_row(form.submit, { 'label': 'Submit me' }) }}
    {{ form_end(form) }}    
```

- [fragment_uri()](https://twig.symfony.com/doc/3.x/functions/fragment_uri.html)

- [impersonation_exit_path()](https://twig.symfony.com/doc/3.x/functions/impersonation_exit_path.html) - url to exit user impersonation

- [impersonation_exit_url()](https://twig.symfony.com/doc/3.x/functions/impersonation_exit_url.html) - similar to above but absolute url

- [importmap()](https://twig.symfony.com/doc/3.x/functions/importmap.html) - outputs the importmap & other items using the Asset component

- [is_granted()](https://twig.symfony.com/doc/3.x/functions/is_granted.html) - returns true if the current user has the given role

- [logout_path()](https://twig.symfony.com/doc/3.x/functions/logout_path.html) - generate a relative logout URL

- [logout_url()](https://twig.symfony.com/doc/3.x/functions/logout_url.html) - like above but absolute

- [path()](https://twig.symfony.com/doc/3.x/functions/path.html) - relative path of a route

  `{{ path('route_name') }}`
  - if it has a wildcard, use `{}` (twig associate array)
    ```twig
    <a href="{{ path('app_question_show', { slug: 'reversing-a-spell' }) }}"><h2>Reversing a Spell</h2></a>
    ```

- [relative_path()](https://twig.symfony.com/doc/3.x/functions/relative_path.html) - relative path from the passed absolute URL

- [render()](https://twig.symfony.com/doc/3.x/functions/render.html) - makes a request to the given internal URI or controller and returns the result

  - Used to [embed controller in templates](https://symfony.com/doc/current/templates.html#templates-embed-controllers)

- [render_esi()](https://twig.symfony.com/doc/3.x/functions/render_esi.html) - similar to above and generates an ESI tag

- [t()](https://twig.symfony.com/doc/3.x/functions/t.html) - Translatable object

- [url()](https://twig.symfony.com/doc/3.x/functions/url.html) - absolute path of a route

---

## Filters

`{{ title | upper }}`

changes the content before render

### Filter Block of Data

Use `apply` tag

```twig
{% apply upper %}
    This text becomes uppercase
{% endapply %}
```


### Filters Documentation

#### Twig Filters

- [abs](https://twig.symfony.com/doc/3.x/filters/abs.html) - absolute value
- [batch](https://twig.symfony.com/doc/3.x/filters/batch.html) - get a list of list with given number of items
- [capitalize](https://twig.symfony.com/doc/3.x/filters/capitalize.html)
- [column](https://twig.symfony.com/doc/3.x/filters/column.html) - gets values from a list of list matching a key
- [convert_encoding](https://twig.symfony.com/doc/3.x/filters/convert_encoding.html) -
- [country_name](https://twig.symfony.com/doc/3.x/filters/country_name.html)
- [currency_name](https://twig.symfony.com/doc/3.x/filters/currency_name.html)
- [currency_symbol](https://twig.symfony.com/doc/3.x/filters/currency_symbol.html)
- [data_uri](https://twig.symfony.com/doc/3.x/filters/data_uri.html)
- [date](https://twig.symfony.com/doc/3.x/filters/date.html) - format date like like php date
- [date_modify](https://twig.symfony.com/doc/3.x/filters/date_modify.html) - alter date with string
- [default](https://twig.symfony.com/doc/3.x/filters/default.html) - if var undefined use default value
- [escape](https://twig.symfony.com/doc/3.x/filters/escape.html) - {{ var|e }}
- [filter](https://twig.symfony.com/doc/3.x/filters/filter.html) - {{ sizes|filter(v => v > 38)|join(', ') }} # 40, 42
- [first](https://twig.symfony.com/doc/3.x/filters/first.html) - first element of a sequence
- [format](https://twig.symfony.com/doc/3.x/filters/format.html) - sprintf
- [format_currency](https://twig.symfony.com/doc/3.x/filters/format_currency.html)
- [format_date](https://twig.symfony.com/doc/3.x/filters/format_date.html)
- [format_datetime](https://twig.symfony.com/doc/3.x/filters/format_datetime.html)
- [format_number](https://twig.symfony.com/doc/3.x/filters/format_number.html)
- [format_time](https://twig.symfony.com/doc/3.x/filters/format_time.html)
- [html_to_markdown](https://twig.symfony.com/doc/3.x/filters/html_to_markdown.html)
- [inline_css](https://twig.symfony.com/doc/3.x/filters/inline_css.html) - makes every element have inline css
- [inky_to_html](https://twig.symfony.com/doc/3.x/filters/inky_to_html.html)
- [join](https://twig.symfony.com/doc/3.x/filters/join.html) - array to string
- [json_encode](https://twig.symfony.com/doc/3.x/filters/json_encode.html)
- [keys](https://twig.symfony.com/doc/3.x/filters/keys.html) - returns keys of array
- [language_name](https://twig.symfony.com/doc/3.x/filters/language_name.html)
- [last](https://twig.symfony.com/doc/3.x/filters/last.html) - last element of sequence
- [length](https://twig.symfony.com/doc/3.x/filters/length.html) - length of sequence
- [locale_name](https://twig.symfony.com/doc/3.x/filters/locale_name.html)
- [lower](https://twig.symfony.com/doc/3.x/filters/lower.html) - lower case
- [map](https://twig.symfony.com/doc/3.x/filters/map.html) - apply arrow function to the elements of a sequence
- [markdown_to_html](https://twig.symfony.com/doc/3.x/filters/markdown_to_html.html)
- [merge](https://twig.symfony.com/doc/3.x/filters/merge.html) - combine arrays
- [nl2br](https://twig.symfony.com/doc/3.x/filters/nl2br.html) - inserts HTML line breaks before newlines in a string
- [number_format](https://twig.symfony.com/doc/3.x/filters/number_format.html) - php number_format()
- [raw](https://twig.symfony.com/doc/3.x/filters/raw.html) - marks value as being "safe"
- [reduce](https://twig.symfony.com/doc/3.x/filters/reduce.html) - reduces sequence to a single value using arrow function
- [replace](https://twig.symfony.com/doc/3.x/filters/replace.html) - replace a substring with a value
  - `{{ "I like %this% and %that%."|replace({'%this%': fruit, '%that%': "oranges"}) }}`
- [reverse](https://twig.symfony.com/doc/3.x/filters/reverse.html) - reverse a sequence
- [round](https://twig.symfony.com/doc/3.x/filters/round.html) - round
- [slice](https://twig.symfony.com/doc/3.x/filters/slice.html) - like substring(start,length)
- [slug](https://twig.symfony.com/doc/3.x/filters/slug.html) - transforms string into an ASCII safe string
- [sort](https://twig.symfony.com/doc/3.x/filters/sort.html) - sort array
- [spaceless](https://twig.symfony.com/doc/3.x/filters/spaceless.html) - remove whitespace between HTML tags
- [split](https://twig.symfony.com/doc/3.x/filters/split.html) - string to array
- [striptags](https://twig.symfony.com/doc/3.x/filters/striptags.html) - remove HTML and PHP tags from string
- [timezone_name](https://twig.symfony.com/doc/3.x/filters/timezone_name.html) -
- [title](https://twig.symfony.com/doc/3.x/filters/title.html) - title case
- [trim](https://twig.symfony.com/doc/3.x/filters/trim.html) - strips whitespaces or other characters from beginning and end of a string
- [u](https://twig.symfony.com/doc/3.x/filters/u.html) - wrap text in unicode object
- [upper](https://twig.symfony.com/doc/3.x/filters/upper.html) - upper case
- [url_encode](https://twig.symfony.com/doc/3.x/filters/url_encode.html) - url percent encodes string or array

#### [Symfony Filters](https://symfony.com/doc/current/reference/twig_reference.html#humanize)

- [abbr_class](/doc/3.x/filters/abbr_class.html) - generates an < abbr> element with the short name of a PHP class
- [abbr_method](/doc/3.x/filters/abbr_method.html) - generates an < abbr> element of the class::method
- [file_excerpt](/doc/3.x/filters/file_excerpt.html) - generates an excerpt of a file around the given line number (1st arg).
  - 2nd arg is total number of lines to display around the given line number (use -1 to display the whole file)
- [file_link](/doc/3.x/filters/file_link.html) - generates a link to the provided file and line number using a preconfigured scheme
- [file_relative](/doc/3.x/filters/file_relative.html) - transforms absolute path to relative
- [format_args](/doc/3.x/filters/format_args.html) - generates a string with the arguments and their types
- [format_args_as_text](/doc/3.x/filters/format_args_as_text.html) - like format_args w/o using HTML tags
- [format_file](/doc/3.x/filters/format_file.html) - generates a file path inside an < a> tag
- [format_file_from_text](/doc/3.x/filters/format_file_from_text.html) - use format_file to improve the output of default PHP errors
- [humanize](/doc/3.x/filters/humanize.html) - makes technical name readable (underscores to spaces, camelCase to camel case)
- [sanitize_html](/doc/3.x/filters/sanitize_html.html) - sanitizes the text using the HTML Sanitizer
- [serialize](/doc/3.x/filters/serialize.html) - accept any data that can be serialized by the Serializer component
- [trans](/doc/3.x/filters/trans.html) - translate
- [yaml_dump](/doc/3.x/filters/yaml_dump.html) - php yaml_encode()
- [yaml_encode](/doc/3.x/filters/yaml_encode.html) - input into YAML syntax

---

## Tags

Logic function / statement on a block of data

### Tags Documentation

#### Twig Tags

- apply
    ```twig
    {% apply lower|escape('html') %}
        <strong>SOME TEXT</strong>
    {% endapply %}
    {# outputs "&lt;strong&gt;some text&lt;/strong&gt;" #}
    ```
- set
  ```twig
  {% set foo, bar = 'foo', 'bar' %} {# multi assignment #}
  {% set foo = [1,2] %}
  {% set foo = {'foo': 'bar'} %}
  {% set foo = 'foo' ~ 'bar %}
  {{ foo }}
  ```
  - Assignment can be any valid twig expression
  - variables are scoped
  - Capture chunks of text
      ```twig
      {% set foo %}
          <div id="pagination">
              ...
          </div>
      {% endset %}
      ```

- block
  ```twig
  {% block name %}
  
  {% endblock %}
  ```
  - "holes" that a child template can put content into.
  - name is optional on endblocks for readability
  - one line a block `{% block title page_title|title %}`
  - Blocks defined in conditionals will still be defined even if the conditional is false ?
    - Render parent block
  - Get parents contents
    - `{{ parent() }}`
  - When using `extends`, all code must be in a `block` tag

- extends
  - extend a template and fill it in by defining blocks with the same name
    ```twig
    {% extends "base.html" %}
    ```   
  - Inheritance Options
    - Twig templates can only extend one other template.
    - can't define multiple block tags with the same name in the same template
    - You can provide a list of templates to extend and are checked for existence.
      - The first template found will be used as a parent.
      - `{% extends ['layout.html', 'base_layout.html'] %}`
    - When using `extends`, all code must be in a `block` tag
  - Parent and Child Example
    - **parent**
        ```twig
        {# base.html #}
        <!DOCTYPE html>
        <html>
            <head>
                {% block head %}
                    <link rel="stylesheet" href="style.css"/>
                    <title>{% block title %}{% endblock %} - My Webpage</title>
                {% endblock %}
            </head>
            <body>
                <div id="content">{% block content %}{% endblock %}</div>
                <div id="footer">
                    {% block footer %}
                        &copy; Copyright 2011 by <a href="http://domain.invalid/">you</a>.
                    {% endblock %}
                </div>
            </body>
        </html>
        ```
    - **child**:
      - **Redefine** `{% block title %}Index{% endblock %}`
      - **Extend** the head
          ```twig
          {% block head %}
              {{ parent() }}
              ...
          {% endblock %}
          ```
      - **Leave undefined** so the parents footer is used
  - Dynamic Inheritance
    - Assign a twig template to a php variable and pass it to the template so it can be extended
    - `{% extends some_var %}` and if `some_var` = a \Twig\Template or \Twig\TemplateWrapper, it will be the parent template
        ```php
        $layout = $twig->load('some_layout_template.twig');

        $twig->display('template.twig', ['layout' => $layout]);
        // {% extends layout %}
        ```
  - Conditional
    - The template name for the parent can be any valid Twig expression.
    - `{% extends mobile ? "minimum.html" : "base.html" %}`

- use
  - The `use` statement tells Twig to import the blocks defined in `blocks.html` through Horizontal reuse.
    - It's a way to have multiple inheritance.
    - since only one extend statement allowed
    ```twig
    {% extends "base.html" %}

    {% use "blocks.html" %}

    {% block title %}{% endblock %}
    {% block content %}{% endblock %}
    ```
  - The `use` tag can only import a template that doesnt extend another template, doesnt define macros, and if the body is empty. But it can `use` other templates.
  - `use` can not be an expression
  - Current template's blocks can override use imported blocks
  - You can use `parent()` to inside a block that is the same name as a block from a `use` block

- includes
  - Includes a template and outputs the rendered content
    ```twig
    {% include 'header.html' %}
        Body
    {% include 'footer.html' %}
    ```
  - It is recommended to use the include function
  - Ignore if Missing
    - `{% include 'sidebar.html' ignore missing %}`
    - Will ignore if the file does not exist

- [embed](https://twig.symfony.com/doc/3.x/tags/embed.html)
  - Combines include and extends

- escape
  ```twig
  {% apply escape('html') %}
      <strong>SOME TEXT</strong>
  {% endapply %}
  {# outputs "&lt;strong&gt;SOME TEXT&lt;/strong&gt;" #}
  ```

- [cache](https://twig.symfony.com/doc/3.x/tags/cache.html)
  ```twig
  {% cache "cache key" %}
      Cached forever (depending on cache implementation)
  {% endcache %}
  ```
  - The `cache` tag is part of the `CacheExtension` (not installed by default)
  - Expire
    - `{% cache "cache key" ttl(300) %}` (300 seconds)
  - Cache Key
    - Any string that does not use the reserved characters `{}()/\@:;`
    - Good practice:
      - Embed useful info in the key that allows the cache to auto expire when it must be refreshed
      - Give each cache a unique name and namespace it like templates
      - Embed an integer that you increment whenever the template code changes (to auto invalidate all current caches)
      - Embed a unique key that is updated whenever the variables used in the template code changes
  - Example: `{% cache "blog_post;v1;" ~ post.id ~ ";" ~post.updated_at %}`
  - Cache a blog content template fragment where blog_post describes the template fragment, v1 represents the first version of the template code, post.id represent the id of the blog post, and post.updated_at returns a timestamp that represents the time where the blog post was last modified.
  - This allows us to avoid using a `ttl`, instead we are using a "validation" strategy instead of "expiration"
  - Cache Tags
  - If your cache implementation supports tags, you can also tag your cache items
  - `{% cache "cache key" tags('blog') %}`
  - The tag creates a new "scope" for variables

- deprecated
  ```twig
  {# base.twig #}
  {% deprecated 'The "base.twig" template is deprecated, use "layout.twig" instead.' %}
  {% extends 'layout.twig' %}
  ```
  - Dont use the `deprecated` tag to deprecate a `block`

- do
  `{% do 1 + 2 %}`
  - Works like variable expression but nothing is printed

- for
  ```twig
  {% for index, user in users %}
  {% endfor %}
  ```
  - Common uses:
    - iterate sequence of numbers
      - `{% for i in 0..10 %}`
    - iterate keys
      - `{% for key in users|keys %}`
    - iterate subset
      - `{% for user in users|slice(0, 10) %}`
  - loop variable: Inside of a `for` tag you can access the `loop` variable
    - loop.index 	   current iteration of the loop. (1 indexed)
    - loop.index0 	   current iteration of the loop. (0 indexed)
    - loop.revindex    number of iterations from the end of the loop (1 indexed)
    - loop.revindex0   number of iterations from the end of the loop (0 indexed)
    - loop.first 	   True if first iteration
    - loop.last 	   True if last iteration
    - loop.length 	   The number of items in the sequence
    - loop.parent 	   The parent context

  - These variables are only available for PHP arrays, or objects that implement the Countable interface:
    - loop.length
    - loop.revindex
    - loop.revindex0
    - loop.last

- if
  ```twig
  {% if online == false %}
    ...
  {% elseif mobile == true %}
    ...
  {% else %}
    ...
  {% endif %}
  ```
  - if an array is populated
    - `{% if users %}`
  - if a variable is defined
    - `{% if users is defined %}`
  - if false
    - `{% if not user.subscribed %}`

- macro

  - like a function

  ```twig
  {# forms.html.twig helper template #}
  {% macro input(name, value, type = "text", size = 20) %}
      <input type="{{ type }}" name="{{ name }}" value="{{ value|e }}" size="{{ size }}"/>
  {% endmacro %}

  {% macro textarea(name, value, rows = 10, cols = 40) %}
      <textarea name="{{ name }}" rows="{{ rows }}" cols="{{ cols }}">{{ value|e }}</textarea>
  {% endmacro %}

  <p>{{ _self.input('password', '', 'password') }}</p>

  ```
  - importing

    ```twig
    {% import "macros.twig" as macros %}

    {% if macros.hello is defined -%}
        OK
    {% endif %}


    {% from "macros.twig" import hello %}

    {% if hello is defined -%}
        OK
    {% endif %}
    ```

  - Tips:
    - Arguments
      - always optional.
      - extra positional arguments end up in the special `varargs` variable as a list of values.
    - They dont have access to the current template variables
      - Pass the whole context as an argument by using the special `_context` variable
    - You need to import inside `embed` tags separately
    - If defined inside `block` tags, they are only defined in the current block and override macros defined at the template level with the same names.
    - If defined inside `macro` tags, they are only defined in the current macro and override macros defined at the template level with the same names.
    - `{% endmacro input %}` add name to end tag for readability

    - Importing macros

      1. import complete template into local variable
      - `{% import "forms.html.twig" as forms %}`
      - `<p>{{ forms.input('username') }}<p>`
      - `<p>{{ forms.input('password', null 'password') }}<p>`
      2. import specific macros into the current namespace
      - `{% from 'forms.html.twig' import input as input_field, textarea %}`
      - `<p>{{ input_field('password, '', 'password') }}</p>`
      - `<p>{{ textarea('comment') }}<p>`

    - Check if Defined

- [sandbox](https://symfony.com/blog/twig-sandbox-information-disclosure)

  - sandbox an include tag for safety

  ```twig
  {% sandbox %}
      {% include 'user.html' %}
  {% endsandbox %}
  ```

- verbatim
  - mark sections as raw text that should not be parsed
  - ex. show twig code without having being rendered into html
    `{% verbatim %}{% endverbatim %}`

- with
  - create a new inner scope

  ```twig
  {% with %}
      {% set foo = 42 %}
      {{ foo }} {# foo is 42 here #}
  {% endwith %}

  {% with { foo: 42 } %}
      {{ foo }} {# foo is 42 here #}
  {% endwith %}

  {% set vars = { foo: 42 } %}
  {% with vars %}
      ...
  {% endwith %}

  {% set bar = 'bar' %}
  {% with { foo: 42 } only %}
      {# only foo is defined #}
      {# bar is not defined #}
  {% endwith %}
  ```

- [autoescape](https://twig.symfony.com/doc/3.x/tags/autoescape.html)
  - choose language to escape for a block of text that wont affect varaibles with the raw filter\

#### Symfony Tags

- form_theme

  `{% form_theme form resources %}`
  ```text
  form
      type: FormView 
  resources
      type: array | string 
  ```
  - Sets the resources to override the form theme for the given form view instance. You can use _self as resources to set it to the current resource. More information in How to Customize Form Rendering.

- stopwatch

  `{% stopwatch 'event_name' %}...{% endstopwatch %}`
  - This measures the time and memory used to execute some code in the template and displays it in the Symfony profiler. See how to profile Symfony applications.

- trans

  - renders the translation of the content

- trans_default_domain

  - set the default domain in the current template.

---

## Debug

### Debug Toolbar

The toolbar inserts itself in before terminating `</body>` tag

### Linting

`$ php bin/console lint:twig`

Checks that your Twig templates dont have syntax errors

```terminal
# check all the application templates
$ php bin/console lint:twig

# check directories and individual templates
$ php bin/console lint:twig templates/email/
$ php bin/console lint:twig templates/article/recent_list.html.twig

# show deprecated features used
$ php bin/console lint:twig --show-deprecations templates/email/
```

- When run inside GitHub Actions, the output is automatically adapted to the format required by GitHub, but you can force that too
  `$ php bin/console lint:twig --format=github`

### List Twig Info

```terminal
# list general information
$ php bin/console debug:twig

# filter output by any keyword
$ php bin/console debug:twig --filter=date

# pass template path to show the physical file which will be loaded
$ php bin/console debug:twig @Twig/Exception/error.html.twig
```

### Dump Twig 

`$ composer require --dev symfony/debug-bundle`
```twig
{# contents of variable are sent to the Web Debug Toolbar #}
{% dump articles %}

{# contents of variable are dumped inside the page contents (ONLY IN DEV) #}
{{ dump(articles) }}
```

## Global

### Global App Variable

`app` = context object Symfony injects into every Twig template automatically

#### app.user

The current user object or null if the user is not authenticated. 

#### app.request - Request Info

The Request object that stores the current request data (depending on your application, this can be a sub-request or a regular request). 

```twig
{% set route_name = app.request.attributes.get('_route') %}
{% set route_parameters = app.request.attributes.get('_route_params') %}

{# use this to get all the available attributes (not only routing ones) #}
{% set all_attributes = app.request.attributes.all %}
```

#### app.session

The Session object that represents the current user's session or null if there is none. 

#### app.flashes

An array of all the flash messages stored in the session. You can also get only the messages of some type (e.g. app.flashes('notice')). 

#### app.environment

The name of the current configuration environment (dev, prod, etc). 

#### app.debug

True if in debug mode. False otherwise. 

#### app.token

A TokenInterface object representing the security token. 

### Global Variables

Inject a variable into all templates 
- by defining variable in the `twig.globals` option 
- inside the Twig configuration file (`config/packages/twig.yaml`)

### Global Service

Will be loaded whether used or not

`'@Namespace\Of\Service'`

## Security

### [Output Escaping](https://twig.symfony.com/doc/3.x/api.html#escaper-extension) | [Prevent Cross-Site Scripting (XSS) Attack](https://symfony.com/doc/5.4/templates.html#output-escaping)

Symfony automatically output escapes

- Set in [Twig autoescape option](https://symfony.com/doc/5.4/reference/configuration/twig.html#config-twig-autoescape)

#### Disable Auto Escaping

`{{ product.title|raw }}`

Use the raw filter

## Configuration

- Configure the [twig configuration](https://symfony.com/doc/5.4/reference/configuration/twig.html) to customize
  - how to default format numbers and dates with filter
  - how to cache

### Add to the template namespace

[Add locations for templates to be stored](https://symfony.com/doc/5.4/templates.html#template-namespaces)
- Namespaces: group several templates under a logic name unrelated to their actual location

#### Bundles' Templates

Installed bundles may include their own Twig templates
- Installed under `Resources/views/`
- Symfony automatically adds bundles' templates under a namespace created after the bundle name
- Example
  - AcmeFooBundle = `@AcmeFoo`
  - `<your-project>/vendor/acmefoo-bundle/Resources/views/user/profile.html.twig`
  - `@AcmeFoo/user/profile.html.twig`

## Twig Extensions

### Twig Official Extensions

- [twig-extra-bundle](https://github.com/twigphp/twig-extra-bundle)
  - allows use of *all* extra extensions w/o any configuration
- [intl-extra](https://github.com/twigphp/intl-extra)
- [cssinliner-extra](https://github.com/twigphp/cssinliner-extra)
- [cache-extra](https://github.com/twigphp/cache-extra)
  - `{% cache %}` [cache tag](https://twig.symfony.com/cache)
- [string-extra](https://github.com/twigphp/string-extra)
  - Provides integration w/ Symfony String component
  - `|u` wraps text in a `UnicodeString` object to give access to [methods of the class](https://symfony.com/doc/current/components/string.html)
  - `|slug` wrapper for the `AsciiSlugger`'s `slug` method
- [markdown-extra](https://github.com/twigphp/markdown-extra)
  - `|markdown_to_html`
  - `|html_to_markdown`
- [inky-extra](https://github.com/twigphp/inky-extra)
- [html-extra](https://github.com/twigphp/html-extra)

### [Custom Twig Extension](https://twig.symfony.com/doc/3.x/advanced.html#creating-an-extension)

Twig Extensions allow the creation of custom functions, filters, tags, and more

#### Example

Create a filter called price

```twig
{{ product.price|price }}

{# pass in the 3 optional arguments #}
{{ product.price|price(2, ',', '.') }}
```

1. Create class that extends `AbstractExtension` and fill in the logic
    ```php
    // src/Twig/AppExtension.php
    namespace App\Twig;

    use Twig\Extension\AbstractExtension;
    use Twig\TwigFilter; //for creating a filter
    use Twig\TwigFunction; //for creating a function

    class AppExtension extends AbstractExtension
    {
        public function getFilters() //for creating a filter
        {
            return [
                new TwigFilter('price', [$this, 'formatPrice']),
            ];
        }

        public function formatPrice($number, $decimals = 0, $decPoint = '.', $thousandsSep = ',')
        {
            $price = number_format($number, $decimals, $decPoint, $thousandsSep);
            $price = '$'.$price;

            return $price;
        }

        public function getFunctions() //for creating a function
        {
            return [
                new TwigFunction('area', [$this, 'calculateArea']),
            ];
        }

        public function calculateArea(int $width, int $length)
        {
            return $width * $length;
        }
    }
    ```
2. Register class as a service and tag it with `twig.extension`
   - if you're using default services.yaml configuration, Symfony will automatically know about your new service and add the tag
3. Check that the filter was added
   - `$ php bin/console debug:twig --filter=price` (shows info about a specific filter)

#### Creating Lazy-Loaded Twig Extensions

##### Explanation

Extension's logic being in the Twig extension class is the simplest way to create extensions.

However, Twig will initialize *all* extensions before rendering any template, even if it isnt used.

If extensions dont define dependencies (i.e. if you dont inject services in them), they dont affect performance.

However, if extensions define lots of dependencies (e.g. those making database connections), performance will be significantly impacted.

##### Fix

Twig allows decoupling of the extension logic from its implementation with `AppRunTime`

1. In the extension class
2. `use App\Twig\AppRuntime;`
3. Remove `formatPrice()` (extension logic) from the extension
4. Update PHP callable defined in `getFilters()`
    ```php
    return [
      new TwigFilter('price', [AppRuntime::class, 'formatPrice']),
    ];
    ```
5. Create the new AppRuntime class (recommended `Runtime` suffix)
6. Add the previous `formatPrice()` method
    ```php
    // src/Twig/AppRuntime.php
    namespace App\Twig;

    use Twig\Extension\RuntimeExtensionInterface;

    class AppRuntime implements RuntimeExtensionInterface
    {
        public function __construct()
        {
            // this simple example doesn't define any dependency, but in your own
            // extensions, you'll need to inject services using this constructor
        }

        public function formatPrice($number, $decimals = 0, $decPoint = '.', $thousandsSep = ',')
        {
            $price = number_format($number, $decimals, $decPoint, $thousandsSep);
            $price = '$'.$price;

            return $price;
        }
    }
    ```
7. [Create a service](https://symfony.com/doc/5.4/service_container.html#service-container-creating-service) for this class and [tag your service](https://symfony.com/doc/5.4/service_container/tags.html) with `twig.runtime`
   - If using default `services.yaml` the configuration is already done by Symfony

## Modal

<https://symfonycasts.com/screencast/stimulus/modal>
<https://designmodo.com/bootstrap-modal/>
<https://www.jqueryscript.net/lightbox/Mobile-Fullscreen-Bootstrap-Modals-jQuery.html>
<https://www.cssscript.com/mobile-friendly-bootstrap-4-modals-with-jquery-bootstrap4-fs-modal/>
<https://ux.symfony.com/demos/live-component/auto-validating-form>
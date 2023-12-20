# [Components](https://symfony.com/bundles/ux-twig-component/current/index.html)

Alternative way to render templates, where each template is bound to a "component class".

`$ composer require symfony/ux-twig-component`

- Easier to re-use small template "units"
  - alert, markup for a modal, category sidebar
- No importing needed as you just use the components function and use the name of the component

## Boilerplate

Component Class
```php
// src/Components/Alert.php
namespace App\Components;

use Symfony\UX\TwigComponent\Attribute\AsTwigComponent;

#[AsTwigComponent]
class Alert
{
    public string $type = 'success';
    public string $message;
}
```

Component Template
```twig
{# templates/components/Alert.html.twig #}
<div class="alert alert-{{ type }}">
    {{ message }}
</div>
```

Use Component
```twig
{{ component('Alert', { message: 'Hello Twig Components!' }) }}
```

## Live Component

Auto update (via Ajax) as the user interacts with them
# Service

Everything done in Symfony is *actually* done by a service (custom object).
So if you want to do something in Symfony, find the service that already does that work.

- Symfony concept, more flexible (src/Service + config/services.yml)
- Defined in services.yml
- Each service has an id (`markdown.parser.light`)
- Symfony puts all the services inside the "Services Container"

## Dump List of Services

`$ php bin/console debug:container`

## Dump List of Autowireable Services

Most services are low-level service objects that help other more *important* services do their work

`$ php bin/console debug:autowiring`

## Use Services

Use in this order:
1. Symfony stuff at top
2. Libraries
3. App

## Autowire Service

Symfony will pass a Service to a Controller as a type-hint argument

```php
use Psr\Log\LoggerInterface;
...
public function comment(LoggerInterface $logger)
...
```

### How does Autowiring work?

1. Download bundle
2. Bundle adds service to the Service Container (using snake.case)
   - `cache.app     Symfony\Component\Cache\Adapter\TraceableAdapter`
   - cant be autowired while in snake.case
3. Symfony may make it autowireable by creating an alias
    - (`Symfony\Component\Cache\Adapter\AdapterInterface     alias for "cache.app"`)
4. Pass argument to controller with type hint 
   - (ex. `public function action(Symfony\Component\Cache\Adapter\AdpaterInterface $adapter)`)
5. Symfony looks for a service in the container with this *exact* id 
   - (`Symfony\Component\Cache\Adapter\AdapterInterface     alias for "cache.app"`)
6. Symfony passes the service to the Controller

## Serializer | Convert Anything to JSON or XML

Perfect for converting **objects** into JSON or XML

Used by `$this->json()` if installed

## Instantiate

```php 
$scope = $this->get('app.asin_scope_service');
```
`$scope` is an instance of the service with dependencies injected

## Request Info

[Get the request info](https://symfony.com/doc/5.4/service_container/request.html)

## Get List of Autowire Cappable Services

`$ php bin/console debug:autowiring`
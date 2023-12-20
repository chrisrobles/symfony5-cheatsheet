# Service

Everything done in Symfony is *actually* done by a service (custom object).

So if you want to do something in Symfony, find the service that already does that work.

- Symfony concept, more flexible (src/Service + config/services.yml)
- Defined in services.yml

## Autowire to Controller

Symfony will pass a Service to a Controller with a Service type-hint argument

```php
use Psr\Log\LoggerInterface;
...
public function comment(... LoggerInterface $logger)
...
```

## Serializer | Convert Anything to JSON or XML

Perfect for converting **objects** into JSON or XML

Used by `$this->json()` if installed

## Logger

```php
use Psr\Log\LoggerInterface;
public function controllerFunc(LoggerInterface $logger)
{
    $logger->info("Message"); //put Message in the Logs tab of the Profiler
}
```

## Instantiate

```php 
$scope = $this->get('app.asin_scope_service');
```
`$scope` is an instance of the service with dependencies injected

## Request Info

[Get the request info](https://symfony.com/doc/5.4/service_container/request.html)

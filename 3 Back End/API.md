# JSON API Endpoint

[Tutorial](https://symfonycasts.com/screencast/api-platform)

A JSON API endpoint will be hit via AJAX and return JSON

## Returning

```php
use \Symfony\Component\HttpFoundation\JsonResponse;
// WACK return new Response(json_encode($currentVoteCount));
// WACK return new JsonResponse(["votes" => $currentVoteCount]);
return $this->json(["key" => $value]);
```

JsonResponse
- extends Response
- `json_encode()`s the data
- sets `Content-Type` to `application/json`
  - helps ajax libraries understand we're returning JSON data
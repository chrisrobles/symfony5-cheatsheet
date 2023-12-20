# Entity

## How to

### Get Entity

```php
//find by id
$client = $this->getDoctrine()
->getRepository(Client::class)
->findOne($id);

//find by attribute
$client = $this->getDoctrine()
->getRepository(Client::class) //gets namespaces with class
->findOneBy(['name' => $name]);
```

## Repository

the way to search the entity

## Access modifier

If it is not public it MUST have a getter and setter for the form builder to access it

## Accessors

Symfony is smart enough to access private and protected fields of an Entity
- For a regular field, it will use `getTask()`
- For a Boolean field, it can use *isser* or *hasser* (`isPublished()` or `hasReminder`)
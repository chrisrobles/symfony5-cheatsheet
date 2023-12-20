# Doctrine

ORM (Object Relationship Manager)

Class = table
Object Instance = row

## Repository

the way to search the entity

## Common Commands

### Update DB Schema

Run after updating Entity (src/Entity/FILE.php)

`php bin/console doctrine:schema:update -f`

### Clear cache for files

Run after a git pull / file change

- Cache should be disabled on local

`php bin/console cache:clear`

### Push to Production

`deploy`

### Remove property / column

Just delete the property and any reference to it.

## SQL Commands / Entity Management

### CREATE TABLE / Make entity

`php bin/console make:entity`

### SELECT

- `::HYDRATE_ARRAY` compute intensive, not usually necessary

#### By ID

```php
//find by id
$client = $this->getDoctrine()
->getRepository(Client::class)
->findOne($id);
```

#### By Attribute

```php
//find by attribute
$client = $this->getDoctrine()
->getRepository(Client::class) //gets namespaces with class
->findOneBy(['name' => $name]);
```

#### WHERE

```php
->where('cl = :client and a.type=:billing')
->setParameter('client', $order ? $order->getClient() : null)
->setParameter('billing', Address::TYPE_BILLING)
```

### INSERT

Create a new instance of object

`$em->flush();` when done to insert

```php
$adjustment = new Adjustment();
$adjustment->setUser($this->getUser())
    ->setIsManual(0)
    ->setTimestamp(new \DateTime())
    ->setChanges($changes);

// save insert into database    
$em->persist($adjustment);
$em->flush();
```

### UPDATE

Same as insert except fetch the $adjustment from the Repository / QB

### DELETE

Same as update,
1) fetch object from Repository / QB
2) `$em->remove($location);`

Example:

```php
$em = $this->getDoctrine()->getManager();
$location = $em->getRepository('App:Location')->findOneBy(['id' => $id]);

$em->remove($location)
$em->flush();
```

## Transactions / flush()

"Remember to flush" / **ALWAYS REQUIRED**

A transaction is a group of queries that are executed in a batch

If any of them fail, the others are rolled back, ex. delete associated stuff

`$em->flush();`
- marks the end of the transaction
- executes the INSERT / UPDATE / DELETE scripts

<https://akashicseer.com/web-development/symfony-doctrine-remove-column-from-entity-table/>

## Access modifier

If it is not public it MUST have a getter and setter for the form builder to access it

## Accessors

Symfony is smart enough to access private and protected fields of an Entity
- For a regular field, it will use `getTask()`
- For a Boolean field, it can use *isser* or *hasser* (`isPublished()` or `hasReminder()`)
---
title: MongoDB
layout: note
---

# MongoDB

MongoDB is a popular "NoSQL" database. It takes the common approach of treating each record as a "document" rather than a row in a table. Each document can have arbitrary key/value pairs. 

## CRUD in MongoDB

All these examples are run from the command line program `mongo`:

```
> use jeckroth
switched to db jeckroth

// query without a "where" clause
> db.cars.find()
{ "_id" : ObjectId("5829f83a91ecd613cf06c664"), "name" : "Toyota", "price" : 26700 }
{ "_id" : ObjectId("5829f83a91ecd613cf06c665"), "name" : "Audi", "price" : 52000 }

// insert new document
> db.cars.insert({'name': 'Hyundai', 'price': 22000})
WriteResult({ "nInserted" : 1 })

// query again
> db.cars.find()
{ "_id" : ObjectId("5829f83a91ecd613cf06c664"), "name" : "Toyota", "price" : 26700 }
{ "_id" : ObjectId("5829f83a91ecd613cf06c665"), "name" : "Audi", "price" : 52000 }
{ "_id" : ObjectId("5829f8af4c86fcd5db7db020"), "name" : "Hyundai", "price" : 22000 }

// update documents
> db.cars.update({'name': 'Hyundai'}, {$set:{'price': 25000}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

// query again
> db.cars.find()
{ "_id" : ObjectId("5829f83a91ecd613cf06c664"), "name" : "Toyota", "price" : 26700 }
{ "_id" : ObjectId("5829f83a91ecd613cf06c665"), "name" : "Audi", "price" : 52000 }
{ "_id" : ObjectId("5829f8af4c86fcd5db7db020"), "name" : "Hyundai", "price" : 25000 }

// remove documents
> db.cars.remove({'name': 'Hyundai'})
WriteResult({ "nRemoved" : 1 })

// query again
> db.cars.find()
{ "_id" : ObjectId("5829f83a91ecd613cf06c664"), "name" : "Toyota", "price" : 26700 }
{ "_id" : ObjectId("5829f83a91ecd613cf06c665"), "name" : "Audi", "price" : 52000 }

// query with a limit
> db.cars.find().limit(1)
{ "_id" : ObjectId("5829f83a91ecd613cf06c664"), "name" : "Toyota", "price" : 26700 }

// query with a sort
> db.cars.find().sort({'name':1})
{ "_id" : ObjectId("5829f83a91ecd613cf06c665"), "name" : "Audi", "price" : 52000 }
{ "_id" : ObjectId("5829f83a91ecd613cf06c664"), "name" : "Toyota", "price" : 26700 }
>
```

## PHP and MongoDB

```php
<?php

$mongo = new MongoDB\Driver\Manager("mongodb://localhost:27017");

/* example: create a collection in jeckroth db; ignore errors */
$cmd = new MongoDB\Driver\Command(['create' => 'cars']);
try {
    $mongo->executeCommand('jeckroth', $cmd);
} catch(MongoDB\Driver\Exception\RuntimeException $e) {
}

/* example: drop a collection in jeckroth db */
$cmd = new MongoDB\Driver\Command(['drop' => 'cars']);
try {
    $mongo->executeCommand('jeckroth', $cmd);
} catch(MongoDB\Driver\Exception $e) {
    echo $e->getMessage(), "\n";
    exit;
}

/* example: create a collection in jeckroth db (again) */
$cmd = new MongoDB\Driver\Command(['create' => 'cars']);
try {
    $mongo->executeCommand('jeckroth', $cmd);
} catch(MongoDB\Driver\Exception $e) {
    echo $e->getMessage(), "\n";
    exit;
}

/* example: insert/update/delete documents */
$bulk = new MongoDB\Driver\BulkWrite;
$bulk->insert(['name' => 'Toyota', 'price' => 26700]);
$bulk->insert(['name' => 'Audi', 'price' => 0]);
$bulk->insert(['name' => 'Hummer', 'price' => 33900]);
$bulk->update(['name' => 'Audi'], ['$set' => ['price' => 52000]]);
$bulk->delete(['name' => 'Hummer']);
$mongo->executeBulkWrite('jeckroth.cars', $bulk);

/* example: query without a 'where' clause */
$query = new MongoDB\Driver\Query([]);
$rows = $mongo->executeQuery('jeckroth.cars', $query);
foreach($rows as $r){
    echo "ID: ".$r->_id.", Name: ".$r->name.", price: \$".$r->price."<br/>";
}
echo "<hr/>";

/* example: query with a 'where' clause */
$query = new MongoDB\Driver\Query(['name' => 'Audi']);
$rows = $mongo->executeQuery('jeckroth.cars', $query);
foreach($rows as $r){
    echo "ID: ".$r->_id.", Name: ".$r->name.", price: \$".$r->price."<br/>";
}
```


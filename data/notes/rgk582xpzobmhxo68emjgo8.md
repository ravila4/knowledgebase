
## Installation

### Docker:

`docker run -d -p 27017-27019:27017-27019 --name mongodb mongo:latest`

## Common navigation commands

Use mongo CLI to connect to host:

```
mongo "mongodb://hostname_or_ip:port"
```

List databases
>`show dbs`

Select database
> `use <database_name>`

List collections
> `show collections`

Get number of documents in collection
> `db.collection.count()`

Get example document from a collection
> `db.collection.findOne()`

## Searching

Search by attribute:

```
db.collection.find({'field': 'value'})
```

Find documents with field:

```
db.collection.find({'field': {$exists: true}})
```

Special operators for filters:

* `$exists`
* `$gt`, `$gte`, `$lt`, `$lte`
* `$type`
* `$and: [{expr}, {expr}]`
* `$eq`

Iterate through search results:

```
myCursor = db.collection.find({'field': 'value'});
myCursor.count();
while (myCursor.hasNext()){
    print(tojson(myCursor.next()));
}
```

Limit to top `n` results:

`.limit()`

## Indexing

### Create an index from a collection

Hash-based sharding. Supports equality matches, but not range-based queries.

`db.chembl.createIndex({'chembl.inchi': 'hashed'})`

Ascending index:

`db.chembl.createIndex({'chembl.inchi': 1})`

## Updating documents

Update single document:

`db.collections.update({'_id': 'value'}, {update})`

Update multiple documents:

`db.collection.updateMany({filter}, {update})`

For modifiacations on specific attributes, use `$set` operator:

`{$set: {'field': 'new_value'}}`

## Inserting new documents

Insert single document to a collection:

`db.collection.insert({doc})`


## Delete a collection

`db.collection.drop()`

## Rename a collection

`db.collection.renameCollection("new_name")`

## Clone a collection

`db.collection1.aggregate([{ $match: {} }, { $out: "collection2" }])`

## Dumping and restoring a collection

This requires installing the mongodb [database tools package](https://www.mongodb.com/try/download/database-tools?tck=docs_databasetools).


```
mongodump --db=<old_db_name> --collection=<collection_name> --out=data/

mongorestore --db=<new_db_name> \
  --collection=<collection_name> data/<db_name>/<collection_name>.bson
```

## Conditionals



### Javascript conditionals

```js
if({condition}){
    // code
}
else {
    //code
}
```

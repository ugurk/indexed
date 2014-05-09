# indexed
=======

Query IndexedDB like MongoDB.
This project derived from @Fluidbyte 's [Riggr](https://github.com/Fluidbyte/Riggr) repository.

See the source:
https://github.com/Fluidbyte/Riggr/blob/master/src/indexed.js


## Indexed

This repository provides IndexedDB management.  Controllers
using this must explicitly pass it in using `define`, then can access the methods
provided:

#### Create A Datastore

The first thing needed when working with IndexedDB is a datastore. Creating this can simply be done with:

```
indexed('myDB').create();
```

#### Insert Records

The `insert()` method can be used to add a single object or array of objects:

```javascript
indexed('myDB').insert({
    name: 'John Doe',
    email: 'jdoe@email.com'
}, function (err, data) {
    if (err) {
        console.log('Nope.');
    } else {
        console.log(data);
    }
});
```

The above would insert the single record and (on success) return the new record as `0` index of an array.

Records automatically receive an `_id` property as their UID, so the output would be:

```javascript
{
    '_id': 928376488383,
    'name': 'John Doe',
    'email': 'jdoe@email.com'
}
```

To insert multiple records, simply supply an array:

```javascript
indexed('myDB').insert([
    {
        name: 'John Doe',
        email: 'jdoe@email.com'
    }, {
        name: 'Jane Smith',
        email: 'jsmith@email.com'
    }
], function (err, data) {
    // Handle response...
});
```

The above would insert the records and return an array of the records.

#### Find Records

The `find()` method can return all, or matching, records from the data store.

```javascript
indexed('myDB').find(function (err, data) {
    if (err) {
        console.log('Nope.');
    } else {
        console.log(data);
    }
});
```

The above would return all results from the datastore.

To query specific records the `find()` method supports object-based queries:

```javascript
indexed('myDB').find({
    _id: 28972387982
}, function (err, data) {
    if (err) {
        console.log('Nope.');
    } else {
        console.log(data);
    }
});
```

The above would return the record matching the `_id`.

Additionally, comparison queries can be made as objects with `$gt` (greater than), `$lt` (less than), `$gte` (greater than or equal), `$lte` (less than or equal), `$like`, and `$ne` (not equal). For example:

```javascript
indexed('myDB').find({
    someNumber: { $gt : 25 }
}, function (err, data) {
    // Handle response...
});
```

The above would return all records where the property `someNumber` is greater than `25`.

#### Update Records

The `update()` method allows for updating individual records, only matching or the entire datastore. It uses the same querying pattern as the `find()` method. For example:

```javascript
indexed('myDB').update({
    _id: 893897389789
}, {
    name: 'New Name'
}, function (err, data) {
    // Handle response...
});
```

The above would update the record with matching `_id` to set the `name` property to `New Name`.

By leaving of the first (query) object argument, all records in the datastore can be updated.

#### Delete Records

The `delete()` method allows for removing individual records, only matching, or the entire datastore. Again, this uses the same querying pattern as `find()`. For example:

```javascript
indexed('myDB').delete({
    _id: 838973897879
}, function (err, data) {
    // Handle response...
});
```

The above would delete the record matching the `_id`.

By leaving off the first (query) object argument, all records in the datastore will be deleted.

#### Dropping

The `drop()` method allows for completely removing the datastore:

```javascript
indexed('myDB').drop();
```

The above would remove the datastore from IndexedDB storage.
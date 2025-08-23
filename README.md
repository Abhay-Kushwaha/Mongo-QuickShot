# Mongo-QuickShot
### Create
- use Database
- db.coll **OR** db.createCollection("coll") **OR** db.coll.insertOne(obj)
- show collections

### Read
- db.coll.find()               // find few of top rows as array/list through which we can iterate
- db.coll.findOne()            // gives top row
- db.coll.findOne({'name':"Ram"})
- db.coll.find({}, {title: 1, date: 1})                //second param called **projection** that is an object that describes which fields to include in the results.
- db.coll.find({}, {_id: 0, title: 1, date: 1})        // exclude the _id field.
- db.coll.find({}, {category: 0})                      // everything except category
```
$eq:           Values are equal
$ne:           Values are not equal
$gt:           Value is greater than another value
$gte:          Value is greater than or equal to another value
$lt:           Value is less than another value
$lte:          Value is less than or equal to another value
$in:           Value is matched within an array
$and:          Returns documents where both queries match
$or:           Returns documents where either query matches
$nor:          Returns documents where both queries fail to match
$not:          Returns documents where the query does not match
$regex:        Allows the use of regular expressions when evaluating field values
$text:         Performs a text search
$where:        Uses a JavaScript expression to match documents
$currentDate:  Sets the field value to the current date
$inc:          Increments the field value
$rename:       Renames the field
$set:          Sets the value of a field
$unset:        Removes the field from the document
$addToSet:     Adds distinct elements to an array
$pop:          Removes the first or last element of an array
$pull:         Removes all elements from an array that match the query
$push:         Adds an element to an array
```
- db.coll.find().count()
- db.coll.find().limit(2)
- db.coll.find().forEach((x)=>{printjson(x)})
- db.coll.find().toArray()

### Create
- db.coll.insert()      // old version, not used currenntly
- db.coll.insertOne({ })
- db.coll.insertMany([ { } , { } ])

### Update
- db.coll.updateOne( { title: "1" }, { $set: { likes: 2 } } ) 
- db.coll.updateMany( { }, { $set: {age:12} } ) 
- db.coll.updateMany( { }, { $set:{age:12} }, { upsert:true } ) // Update the document but insert it if not found
- db.coll.updateMany( { }, { $inc: {age:12} })   // $inc (increment) operator:

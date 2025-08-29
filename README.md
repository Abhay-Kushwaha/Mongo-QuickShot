# Mongo-QuickShot

### Import file in mongo
- mongoimport <path> -d database -c collections --jsonArray --drop      // providing json array and drop the previous collection if created

### Create
- use Database
- show collections
- db.coll **OR** db.createCollection("coll") **OR** db.coll.insertOne(obj)
- Schema set
```
  db.createCollection("students", {validator:{
      $jsonSchema:{
          required:["name","age"],
          properties:{
              name:{bjsonType:"string" , description: "must be string"},
              age:{bjsonType:"number" , description: "must be number"}
          }
      }
  },
  validationAction:'warn'})        // can be error by default
```

### Read
- db.coll.find()               // find few of top rows as array/list through which we can iterate
- db.coll.findOne()            // gives top row
- db.coll.findOne({'name':"Ram"})
- db.coll.find({}, {name:1, _id:0})                //second param called **projection** that is an object that describes which fields to include in the results.
- db.coll.find({}, {_id: 0, title: 1, date: 1})        // exclude the _id field.
- db.coll.find({}, {category: 0})                      // everything except category
```
$eq:           Values are equal
$ne:           Values are not equal
$gt:           Value is greater than another value
$gte:          Value is greater than or equal to another value
$lt:           Value is less than another value
$lte:          Value is less than or equal to another value
$mod:          Performs modulo operation
$in:           Value is matched within an array
$nin:          Value is matched that are NOT in array
$all:          Ignore order and find all
$and:          Returns documents where both queries match
$elemMatch:    match elements in array ($and do not check both condition in same array)
$or:           Returns documents where either query matches
$nor:          Returns documents where both queries fail to match
$not:          Returns documents where the query does not match
$regex:        Allows the use of regular expressions when evaluating field values /* /
$expr:         Allows aggregation expressions within query
$text:         Performs a text search (only in text indexed, createIndexx({key:"text"}))
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
$addToSet:     No redundency if the data already exists (better than push)
$size:         To get the size of element selected in array
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
- db.coll.updateOne( { name: "Ravi" }, { $set:{age:22} } ) 
- db.coll.updateMany( {age:13}, { $set: {age:12} } ) 
- db.coll.updateMany( { }, { $set:{age:12} }, { upsert:true } ) // Update the document but insert it if not found
- db.coll.updateMany( { }, { $inc: {age:12} })   // $inc (increment) operator:
- db.coll.updateMany( {filter}, { $set:{ "arr.$.newfield": value} })   // add new field in first match inside an array object
```
>>> db.coll.updateMany( {filter},
    { $set: { "arr.$[].newfield": value } }
    )             // add new field in ALL MATCHES inside an array object
>>> db.coll.updateMany( {filter},
    { $set: { "arr.$[e].newfield": value } },
    { arrayFilters: [ { "e.age": { $lt: 10 } } ] }
    )             // add new field in ALL MATCHES over certain condition inside an array object
```

### Delete
- db.coll.deleteOne({name:"Ravi"})
- db.coll.deleteMany({age:13})
- db.coll.deleteMany({})          // Empty the collections
- db.dropDatabase()               // delete complete collection
- db.coll.drop()                  // delete single collection

## Indexing
- db.coll.createIndex({name:1})                // indexing in name to search fast (use -1 for descending)
- db.coll.createIndex({name:1}, {expireAfterSeconds: 3600})                // indexing expire after 1 hr, on date and single field index
- **Covered Query** serach in BTREE if indexing is done so it takes less time
- In multiple indexes on same field, it searches on best one by running and testing on some sample cases
- **Multi Key Index** indexing on array, created for each element so consume a lot of space
- db.coll.createIndex({name:1,bio:1}, {weights:{name:10,bio:5}})                // indexing on multiple field index with priority using weights
- **Text Index** only one text index per collection (can add mutiple fields like "name" and "bio" together)
- db.coll.find({$text:{$search:"Youtube"}})
- db.coll.find({$text:{$search:"Youtube"}}, {Score:{$meta:"textScore"}})        // Gives the score acording to which text indexing is segregated for output
- db.coll.getIndexes()                         // show indexing
- db.coll.dropIndex({name:1})                  // deleting indexing in name

### Transaction
- **Atomicity-** at document level

### Additional
- foundedOn: new Data()
- Timestamp: new Timestamp()
- typeof db.coll.findOne().name      // to check datatype
- db.books.insertmany([{ },{ }] , {ordered:false})      // will continue after if error occured in mid
- Write concern specification {w:<value>, j:<value>, wtimeout:<value>}  //w wait for acknowledgement, j forms the journal
- db.coll.find({hasAadhar: {$exists:true, $type:'number' }})          // check if somthing exists
- db.coll.find().sort({age:1, name:1})         // First by age and then by name
- db.coll.find().sort({age:-1})                // descending
- db.coll.find().sort({age:-1}).skip(5)        // descending and skip top 5
```
$min: db.coll.updateOne({name:"Sita"} , {$max:{age:20}})  // set 20 if her age is less than 20
$max: db.coll.updateOne({name:"Sita"} , {$min:{age:15}})  // set 15 if her age is greater than 15
$inc: db.coll.updateMany({ } , {$inc: {age+1} })
$mul: db.coll.updateOne({name:"Sita"} , {$mul: {age:2} })     // multiply by 2
$unset: to remove some field
```

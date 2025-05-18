# My Mongodb Tutorial

## mongodb setup

### mongodb local server(mongod)
* systemctl: The command-line tool for interacting with systemd, the init system and service manager on modern Linux systems.
* common uses of systemctl
    * sudo systemctl start <service> //Start a service (e.g., sudo systemctl start nginx)
    * sudo systemctl stop <service> //Stop a service
    * sudo systemctl restart <service> //Restart a service
    * sudo systemctl status <service> //Show the status of a service
    * sudo systemctl enable <service> //Enable a service to start at boot
    * sudo systemctl disable <service> //Disable a service from starting at boot
* start the mongod service `sudo systemctl start mongod`
  
### Mongodb atlas(dbms managed service)
* it is online database management service
* hierarchy in atlas
  1. mongodb account
  2. organization
  3. project
  4. cluster
  5. database
  6. collection (tables)
  7. documents (rows)
  8. fields (columns)

### mongodb compass
* install mongodb-compass for interacting with dbms(mongod) using gui

### mongodb shell
* install mongoShell (mongosh) for interacting dbms in cli
  1. to clear screen `cls`
  2. to exit `exit`
* use mongosh from vs code and also download mongodb extension for vscode

## mongodb shell commands
* creating, deleting dbs and collections 
  * show dbs //display all databases we can also use "show databases", dbs are visible only if they contain one or more collections
  * use school //start using school db, if not exist new one created 
  * db.createCollection("students") //db in "db.create.." refer the currently selected database
  * show collections //or
  * db.getCollectionNames()
  * db.students.drop() //to drop students collection
  * db.dropDatabase()
  * db.createCollection("my_collection", {capped:true, size:1000000, max:100}, {autoIndexId:false}) //set capped: true if you want to specify size in bytes, max specify maximum documents the collection can accommodate

* inserting, deleting documents in collection
  * db.students.insertOne({name: "alice", age: 20, cgpa: 10})
  * db.students.find()
  * db.students.insertMany([{name: "bob", age: 19, cgpa: 9}, {name: "xyz", age: 18, cgpa: 8}])
  * db.students.find()
  * db.students.deleteOne({name: "xyz"}) //similarly deleteMany({filter}) use for deleting many documents
  * db.students.deleteMany({behavior: {$exists: false}})

* different dataTypes in mongodb
  * db.createCollection("dataTypes")
  * db.dataTypes.insertMany([{string: "bob"}, {int: 19}, {boolean: false}, {time: new Date()}, {specific_time: new Date("2025-02-01T00:01:54")}, {nothing: null}, {array: ["money", "gold", "diamond"]}, {object: {street: "fake_street", city: "some_city", planet: "earth"}}])
  * db.dataTypes.find()
   
* displaying, sorting documents(find() method)
  * db.students.find() //display all documents in the students collection
  * db.students.find().sort({name:1}) //sorting in alphabetical order, use {name:-1} for reverse alphabetical order
  * db.students.find().limit(2)
  * db.students.find().sort({cgpa:1}).limit(2)
  * db.students.find({name:"alice", age:20}) //select only if both conditions are satisfied
  * db.students.find({}, {name:true}) //find({query}, {projection}), projection specify how to display documents in output
  * db.students.find({name:"alice"}, {_id:false, age:true})

* updating documents(fields)
  * adding, updating fields
    * db.students.updateOne({name: "alice"},{$set:{cgpa: 0}}) //updateOne(filter, update), $set operator update only cgpa field from document, if not exist it create new field
    * db.students.updateOne({_id: ObjectId('6828abc2f5839a107fc59f35')}, {$set: {cgpa: 5}}) //in every document _id is default field 
    * db.students.updateMany({}, {$set:{behavior: "good"}}) //adding new field to all documents of collection(students)
    * db.students.updateMany({behavior: {$exists: false}}, {$set:{behavior: "bad"}}) //here behavior:{$exists: false} is criteria saying that behavior field existence is false 
  
  * deleting fields
    * db.students.updateOne({name: "alice"}, {$unset: {age: ""}}) //to remove age fields set it as empty string
    * db.students.updateMany({}, {$unset: {behavior: ""}}) //to delete specific field from all documents of a given collection
    * db.students.updateMany({behavior: {$exists: true}}, {$unset: {behavior: ""}})

* operators in mongodb
  * $ne, $lt, $lte, $gt, $gte
  * db.students.find({cgpa: {$gte: 8}})
  * db.students.find({cgpa: {$gte: 3, $lte: 9 }})
  * db.students.find({name: {$in : ["alice","bob", "abc"]}})
  * db.students.find({name: {$nin : ["alice","bob", "abc"]}}) //$nin for not in
  *  $and, $or, $not, $nor, $not
  *  db.students.find({$and: [{name: "alice"}, {age: 20}]})
  *  db.students.find({cgpa: {$not: {$gte: 8}}}) //use not if you want to get null fields in output

* index in mongodb
  *  db.students.find({name: "alice"}).explain("executionStats") //linear search without indexes
  *  db.students.createIndex({name: 1}) //it create indexes for name field over the students collection in ascending order
  *  db.students.find({name: "alice"}).explain("executionStats") //search quickly using index
  *  db.students.find({age: 20}) //not fast because age field is not indexed
  *  db.students.getIndexes() //show indexes
  *  db.students.dropIndex("name_1") //drop index
  

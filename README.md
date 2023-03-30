# nosql_hw
## Create container
```
docker run --name nosql  -e MONGO_INITDB_ROOT_USERNAME=myAdmin -e MONGO_INITDB_ROOT_PASSWORD=P@ssw0rd -d mongo

```
- access MongoDB container
```
docker exec -it nosql bash
```
- Authentication
```
mongosh -u myAdmin --authenticationDatabase admin -p
```
## Create Database
```
use students
```
## Create data
```
 db.student.insertMany([{"name":"Ramesh","subject":"maths","marks":87},{"name":"Ramesh","subject":"english","marks":59},{"name":"Ramesh","subject":"science","marks":77},{"name":"Rav","subject":"maths","marks":62},{"name":"Rav","subject":"english","marks":83},
... {"name":"Rav","subject":"science","marks":71},{"name":"Alison","subject":"maths","marks":84},{"name":"Alison","subject":"english","marks":82},{"name":"Alison","subject":"science","marks":86},{"name":"Steve","subject":"maths","marks":81},{"name":"Steve","subject":"english","marks":89},{"name":"Steve","subject":"science","marks":77}])
```
or use
```
db.student.insertOne({"name":"Jan","subject":"english","marks":0,"reason":"absent"})
```

## Aggregation

1.Find the total marks for each student across all subjects.
```
db.student.aggregate([ {$unwind: '$subject'}, {$group:{_id:'$name', marks :{$sum:'$marks'}}}])
```

2.Find the maximum marks scored in each subject.
```
db.student.aggregate([ { $unwind: "$marks" }, { $group: { _id: "$subject", max: { $max: "$marks" } } }])
```

3.Find the minimum marks scored by each student.
```
db.student.aggregate([ {$unwind: '$subject'}, {$group:{_id:'$name', marks :{$min:'$marks'}}}])
```

4.Find the top two subjects based on average marks.
```
db.student.aggregate([{ $unwind: '$name' }, { $group: { _id: '$subject', marks: { $avg: '$marks' } } }, { $sort: { marks: -1 } },{$limit: 2}])
```

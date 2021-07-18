## Intro

https://university.mongodb.com/mercury/M121/2021_June_22/chapter/Chapter_0_Introduction_and_Aggregation_Concepts/lesson/59c9190e0ddb4c1993b02599/lecture

Using the mongodb enterprise collection publically shared in this course

```
docker run -it mongo /bin/bash

mongo "mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jxeqq.mongodb.net:27017/aggregations?replicaSet=Cluster0-shard-0" --authenticationDatabase admin --ssl -u m121 -p aggregations --norc

show collections
```

## Pipelines

Pipelines are a set of stages through which documents pass through and get processed.

For example, a 3 stage pipeline could be $match, $project, $group. We can use it to filter and transform data.

1. pipelines are composition of stages
2. stages are configurable to get desired transformation
3. documents flow through a pipeline like an assembly line
4. stages can be arranged in any way we like and as many as required (with few exceptions)

## Aggregation framework structure and syntax

`db.collection.aggregate([{ stage1 }, { stage2 }], {options})`

options - like allowDiskUse

operators: 
1. Query ops - $lte, $gt, $eq, $in etc
2. Aggregation ops - $match, $project, $group etc

an example of a stage is:

```
 { 
   $match: {
     atmosphericComposition: { $in: [/O2/]},   
	 meanTemperature: { $gte: -40, $lte: 40 } 
	 } 
 }
 ```

As you can see, as a rule of thumb, the ops are in the "key" side of the js-like expression.

We can also access fields using the $ notation. 

Field Path notation: "$fieldName", e.g "$numberOfMoons"

System Variable: "$$UPPERCASE" e.g. "$$CURRENT"

user variable: "$$lowercase" e.g "$$foo"

## Aggregate Ops

`$match` - Match does filtering. match may be used multipletime and most stages can be used after this. $match can make use of comparisons, logic and arrays. One limitation is it cannot use $where operator. If it is the first stage, then $match can use $test op. $match can also use an index to speed up querying.

```
db.solarSystem.aggregate([
  {
    $match: {
	  type: {"$ne": "Star"}
	}
  },
  {
    $count: "planets"
  }
])
```

Outputs: `{ "planets": 8}`. Here $match takes a prop of the document as arg and a comparison op as its val. $count takes a new prop name as the arg to count all the documents from the previous pipeline and output the count against that prop name.


Atlas db for the coming section

`mongo "mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jxeqq.mongodb.net:27017/aggregations?replicaSet=Cluster0-shard-0" --authenticationDatabase admin --ssl -u m121 -p aggregations --norc`

Here is a problem statement and the solution:

Match all `movies` with:
-   imdb.rating is at least 7
-   genres does not contain "Crime" or "Horror"
-   rated is either "PG" or "G"
-   languages contains "English" and "Japanese"


```
var pipeline = [

 {

 $match: {

 $and: [

 { 'imdb.rating': {$gte: 7} },

 {

 $or: [

 {

 rated: {$eq: "PG"}

 }, 

 {

 rated: {$eq: "G"}

 }

 ]

 },

 {

 $and:[

 { languages: { $elemMatch: { $eq: "English" } } },

 { languages: { $elemMatch: { $eq: "Japanese" } } }

 ]

 },

 {

 $nor:[

 { genres: { $elemMatch: { $eq: "Crime" } } },

 { genres: { $elemMatch: { $eq: "Horror" } } }

 ]

 }

 ]

 }

 }

]
```

## $project

This is projection of data. Think of projection in Dynamodb with `all, keys` etc. Here we can project any fields that are needed and ones that need to be explicitly removed.

`$project: {_id: 0, name: 1, gravity: 1}`
means of all fields only show name and gravity. `_id` is a field that will be present even if not projected, therefore we explicitly remove it by `_id: 0`.

The projected document is called a sub document.

`$project: {_id: 0, name: 1, surfaceGravity: "$gravity.value"}}`

- To use `.` notation, we have to specify fields as strings
- we are not renaming `gravity` field to `surfaceGravity`, rather we are creating a new field here

in project we can use ops like `$multiply`, `$divide` etc to create new fields. Here I assume my weight on earth to be 65kg.

`$project: {_id: 0, name: 1, weightOnPlanet: { $multiply: [ { $divide: ["$gravity.value", 9.8]} , 65] }}`

Takeaways:
- `_id` field is the only exception to be specified for removal, otherwise if a field is not specified in project, they are removed
- project lets us add new fields
- can be used many times in our pipeline
- can be used to rename field names

## $addFields

With project, we specify the fields that should be present in the output (like `{_id: 0, mass: 1}`, we can also specify new fields that are based of transforming exiting fields
`{..., gravity: "$gravity.value"`. What if we just need the entire document, but with added fields? in comes addFields.

`db.solvarSystem.aggregate([
	{
		$addFields: {
		  gravity: "$gravity.value",
		  mass: "$mass.value",
		  weight: { $multiply: [ "$gravity", "$mass" ] }
		}
	}
])`

## Cursor like stages

We can use chained methods like skip, limit, count etc after find. `db.movie.find({"imdb.rating": {$gt: 6}}).skip(5)` - This will skip 5 items and show a different set of results of N items. Note: the results are by default in insert order, you can use `sort` to create a new sort order.

There are similar cursor-like functionality available as stages, $limit, $sort, $skip etc. Here $sort if used as the first stage will make use of indexes and will be fast. Also $sort has a max ram of 100MBs, allowDiskUse: true, can be used to sort larger data.

## $sample

Selects a set of random samples from a collection.

`{ $sample: { size: N } }`

## $group

group documents by an expression and use accumulator expressions to add new fields

```
{
  $group: {
    _id: year,
	num_films_in_year: { $sum: 1 }
  }
}
```

other accumulator expressions are $avg, $stdDevPop, $stdDevSam etc

## $lookup

Effectively a left added join.

$lookup: {

}
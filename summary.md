//6-1 $match , $project aggregation stage
1.Aggregation is a way of processing a large number of documents in a collection by means of passing them through different stages. The stages make up what is known as a pipeline. The stages in a pipeline can filter, sort, group, reshape and modify documents that pass through the pipeline.
2.db.test.aggregate([]) as like as db.test.find([])
3.db.test.aggregate({"_id" : ObjectId("6406ad64fc13ae5a40000088")})
4.db.test.aggregate({$match: {gender:"Male"}})
5.db.test.aggregate({$match: {gender:"Male",age:{$lte:30}}})
6.db.test.aggregate([
{$match: {gender:"Male",age:{$lt: 20}}},
{$project: {gender:1,name:1,email:1}}
])

//6-2 $addFields , $out , $merge aggregation stage
db.test.aggregate([
    //stage-1
    {$match: {gender:"Male",age:{$lt: 30}}},
    //{$match: {gender:"Male"}},

    //stage-2
    //{$match:{age:{$lt: 10}}},

    //stage joto beshi hobe,query toto beshi hobe
    //stage to stage means hand to hand data pass

    //stage-3
    //{$project: {gender:1,name:1,course:1,eduTech:1}}

    //stage-4:akn norton 1t field add korbo
    {$addFields: {course:"LEVEL-2 DEVELOPER",eduTech:"Programming Hero"}}

    //ai $addFields: {} main collection e add hobena.not modify to main collection.only running aitate update dekabe

    //$addFields: {} die noton 1t collection ready korte hole use new stage-->{$out:"collectionName"}

    //stage-4
    //{$out: "test2"}  //create newCollection

    // {$project: {gender:1,name:1,course:1,eduTech:1}} e ja kiso mention takbe sei data golo add hobe.add new $addFields: {} data to main file comment the project stage

    //stage-5:original collection e $addFields: {} er data merge korte hole use $merge:

    // {$merge: "test"}
    ])

    {*

    // db.test.find([]).project({gender:1,name:1,eduTech:1,course:1})
    //db.test2.find([]).project({gender:1,name:1,eduTech:1})
    // db.test2.find([])
    db.test.aggregate()

    *}

//6-3 $group , $sum , $push aggregation stage
db.test.aggregate([
    //group hobe:science group,arts group,commerce group
    //stage-1
    // {$group: { \_id: "$"}}------>syntax;group korte hole _id must takte hobe
    // {$group: { \_id: "$gender"}}   //same gender nie 1t group
    // {$group: { \_id: "$Country"}} //same country nie 1t group

    // {$group: { _id: "$address"}}
    // {$group: { _id: "$address.country"}}
    // {$group: { _id: "$age"}}
    // {$group: { _id: "$age",count: {$sum: 1}}}  //same age er koijon ase seigolo sum kore dekabe

    // {$group: { _id: "$gender",count: {$sum: 1}}}  //same gender er koijon ase seigolo sum kore dekabe

    //to add extra value use $push
    // {$group: { _id: "$gender",amakeDekao:{$push: "$name"}}
    // {$group: { _id: "$address.country",amakeDekao:{$push: "$name"}}
    // {$group: { _id: "$address.country",amakeDekao:{$push: "$$ROOT"}}
    {$group: { _id: "$address.country",fullDocument:{$push: "$$ROOT"}}

    //stage-2--->fullDocument teke frontend e ja kiso dekano hobe
    {$project: {"fullDocument.name":1,"fullDocument.gender":1,"fullDocument.email":1}}

    ])

//6-4 explore more about $group & $project
db.test.aggregate([

    //stage-1
    // {$group: { _id: "$"}}      //syntax
    // {$group: { _id: "null"}}
    {$group: {
        _id: null,
        totalSalary:{$sum: "$salary"}, //sobar salary sum kore dekabe
        maxSalary:{$max: "$salary"},
        minSalary:{$min: "$salary"},
        avgSalary:{$avg: "$salary"}
    }
    }

    //stage-2
    {$project: {
        totalSalary:1,
        maxSalary:1,
        minSalary:1,

        //avgSalary:1  I want to change name of avgSalary
        averageSalary :"$avgSalary",

        // rangeBtwnnMinMax:{$subtract: ["$maxSalary","$minSalary"]}
        //"errmsg" : "PlanExecutor error during aggregation :: caused by :: can't $subtract int from string",

        rangeBtwnnMinMax:{$subtract: [{"$toInt":"$maxSalary"},{"$toInt":"$minSalary"}]}

    }}
    ])

//6-5 Explore $group with $unwind aggregation stage

db.test.aggregate([
//stage-1
{
// $unwind: "$friends"
$unwind: "$interests" //unwind use kore array er element goloke seperate || partitioning kora hosse,
}

//stage-2
// {
// $group: { _id: "$friends" }
// }

    // {
    //     // $group: { _id: "$friends",count:{$sum: 1} }
    //     // $group: { _id: "$interests",count:{$sum: 1} }
    //     $group: { _id: "$age",interestPerAge:{$push: "$intersets"}}
    // }
    {
        $group: { _id: "$age",interestPerAge:{$push: "$interests"}}
        // $group: { _id: "$age",interestPerAge:{$push: "$intersets"}} here u write intersets
    }

])

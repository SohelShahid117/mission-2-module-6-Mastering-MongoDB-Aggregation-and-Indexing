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

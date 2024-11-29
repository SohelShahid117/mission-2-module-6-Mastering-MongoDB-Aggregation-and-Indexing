//6-1 $match , $project aggregation stage
1.Aggregation is a way of processing a large number of documents in a collection by means of passing them through different stages. The stages make up what is known as a pipeline. The stages in a pipeline can filter, sort, group, reshape and modify documents that pass through the pipeline.
2.db.test.aggregate([])
3.db.test.aggregate({"_id" : ObjectId("6406ad64fc13ae5a40000088")})
4.db.test.aggregate({$match: {gender:"Male"}})
5.db.test.aggregate({$match: {gender:"Male",age:{$lte:30}}})
6.db.test.aggregate([
    {$match: {gender:"Male",age:{$lt: 20}}},
    {$project: {gender:1,name:1,email:1}}
    ])

//6-2 $addFields , $out , $merge aggregation stage
1.
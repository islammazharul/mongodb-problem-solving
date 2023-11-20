01. Find all documents in the collection where the age is greater than 30, and only return the name and email fields.

db.test.find({
    age: { $gt: 30 }
}).project({
    name: 1,
    email: 1
})

02. Find documents where the favorite color is either "Maroon" or "Blue."

db.test.find({
    $or: [
        { favoutiteColor: { $eq: "Maroon" } },
        { favoutiteColor: { $eq: "Blue" } },
    ]
})

03. Find all documents where the skills is an empty array.

db.test.find({skills: $size: 0}).project({skills: 1})

04. Find documents where the person has skills in both "JavaScript" and "Java."




05. Add a new skill to the skills array for the document with the email "aminextleveldeveloper@gmail.com" The skill is {"name": "Python", "level": "Beginner", "isLearning": true}.



06. Add a new language "Spanish" to the list of languages spoken by the person.



07. Remove the skill with the name "Kotlin" from the skills array.

db.test.updateOne({
    {_id: ObjectId("6406ad63fc13ae5a40000066")},
    {$pull: {
        "skills.name": "KOTLIN"
    }}
})



//implicit and, 
// db.test.find({ gender: "Female", age: { $gte: 18, $lte: 30 } }, { age: 1, gender: 1 }).sort({ age: 1})

// db.test.find({
//     gender: "Male",
//     age: {$nin: [20,22,24,26]},
//     interests: {$in: ["Gaming", "Cooking"]}},
//     {age:1, gender:1, interests:1})

// db.test.find({ gender: "Female", age: { $gte: 18, $lte: 30 } }, { age: 1, gender: 1 }).sort({ age: 1})

// db.test.find({
//   "skills.name" : {$in: ["JAVASCRIPT", "JAVA"]}
// }).project({skills:1}).sort({age:1})

// explicit $and

// db.test.find({
    
//     $and: [
//         {gender: "Female"},
//         {age: {$gt:18}},
//         {age: {$lt:30}},
//         ]
// }).project({age:1}).sort({age:1})

// explicit $or

// db.test.find({
    
//     $or: [
//         {gender: "Female"},
//         {age: {$gt:18}},
//         {age: {$lt:30}},
//         ]
// }).project({age:1}).sort({age:1})

// db.test.find({
    
//     $or: [
//         {interests: "Cooking"},
//         {interests: "Travelling"},
//         ]
// }).project({interests:1}).sort({age:1})

// db.test.find({
    
//     $or: [
//         {"skills.name": "JAVASCRIPT"},
//         {"skills.name": "JAVA"},
//         ]
// }).project({skills: 1})


// db.test.find({interests: {$type: "array", $exists: true} })
// db.test.find({friends: {$size: 0}}).project({friends: 1})


// db.test.find({
//     interests: {$all: ["Cooking", "Gaming"]}
// }).project({interests:1})

// db.test.find({
//     skills: {
//         $elemMatch: {
//             name: "JAVASCRIPT",
//             level: "Intermidiate"
//         }
//     }
// }).project({ skills: 1 })

// update operator
// db.test.updateOne(
//   {_id: ObjectId("6406ad63fc13ae5a40000066")},
//  primitive er jonno $set but non primitive er jonno $addToSet use korbo 
//   {$set: {
//       age: 13
//   }}
// )

// db.test.updateOne(
//     { _id: ObjectId("6406ad63fc13ae5a40000066") },
//     {
//         $addToSet: {
//             friends: { $each: ["Robin", "Islam"] }
//         }
//     }
// )
// db.test.updateOne(
//     { _id: ObjectId("6406ad63fc13ae5a40000066") },
//     {
//         $push: {
//             friends: { $each: ["Robin", "Islam"] }
//         }
//     }
// )

// db.test.updateOne(
//   {_id: ObjectId("6406ad63fc13ae5a40000066")},
//   {$unset: {
//       age: ""
//   }}
// )
// db.test.updateOne(
//   {_id: ObjectId("6406ad63fc13ae5a40000066")},
//   {$pop: {
//       friends: 1
//   }}
// )
// db.test.updateOne(
//   {_id: ObjectId("6406ad63fc13ae5a40000066")},
//   {$pop: {
//       friends: -1
//   }}
// )
// db.test.updateOne(
//   {_id: ObjectId("6406ad63fc13ae5a40000066")},
//   {$pull: {
//       languages: "Estonian"
//   }}
// )

// db.test.updateOne(
//   {_id: ObjectId("6406ad63fc13ae5a40000066")},
//   {$pullAll: {
//       languages: [ "Assamese", "Portuguese" ]
//   }}
// )

// db.test.updateOne(
//   {_id: ObjectId("6406ad63fc13ae5a40000066")},
//   {$set: {
//       "address.city": "dhaka",
//       "address.country": "bangladesh",
//   }}
// )

// db.test.updateOne(
//   {_id: ObjectId("6406ad63fc13ae5a40000066"), "education.degree": "Master of Fine Arts"},
//   {$set: {
//       "education.$.degree": "Science"
//   }}
// )


// db.test.updateOne(
//   {_id: ObjectId("6406ad63fc13ae5a40000066")},
//   {$inc: {
//       salary: 5
//   }}
// )

// db.test.deleteOne(
//   {_id: ObjectId("6406ad63fc13ae5a40000066")},
  
// )

// db.testing.drop({ writeConcern: { w: 1 } })



<!-- MongoDB Aggregation -->

db.test.aggregate([
    // stage-1
    { $match: { gender: "Male", age: { $gt: 18, $lt: 30 } } },
    //  stage-2 
    { $project: { gender: 1, age: 1, name: 1 } }
])

db.test.aggregate([
    // stage-1
    { $match: { gender: "Male" } },
    //  stage-2
    {$addFields: {course: "Level-2", eduTech: "Programming Hero"}},
    // stage-3 
    // { $project: { gender: 1, course: 1, eduTech: 1} },
    // stage-4
    // {$out: "newCourse"},
    {$merge: "test"}
])

db.test.aggregate([
    // stage-1
    {$group: {_id: "$address.country", count: {$sum: 1}, fullDoc: {$push: "$$ROOT"}}},
    // stage-2
    {$project: {"fullDoc.name": 1, "fullDoc.age": 1, "fullDoc.email": 1}}
    ])


    db.test.aggregate([
    // stage-1
    {
        $group: {
            _id: null,
            totalSalary: { $sum: "$salary" },
            maxSalary: { $max: "$salary" },
            minSalary: { $min: "$salary" },
            avgSalary: { $avg: "$salary" }

        }
    },
    // stage-2
    {
        $project:
        {
            totalSalary: 1,
            maxSalary: 1,
            minSalary: 1,
            averageSalary: "$avgSalary",
            rangeBetweenMaxAndMin: { $subtract: ["$maxSalary", "$minSalary"] }
        }
    }
])

db.test.aggregate([
    // stage-1
    {$unwind: "$friends"},
    // stage-2
    {$group: { _id: "$friends",  count: {$sum: 1}},}
    ])

db.test.aggregate([
    // stage-1
    {$unwind: "$interests"},
    // stage-2
    {$group: { _id: "$age",  interestPerAge: {$push: "$interests"}},}
    ])


    db.test.aggregate([
    // stage-1
    {
        $bucket: {
            groupBy: "$age",
            boundaries: [20, 40, 60, 80],
            default: "80 uporer bura gula",
            output: {
                count: { $sum: 1 },
                karakaraAse: { $push: "$$ROOT" }
            }
        }
    },
    // stage-2
    {
        $sort: {
            count: -1
        }
    },
    // stage-3
    {
        $limit: 2
    },
    // stage-4
    {
        $project: {
            count: 1
        }
    }
])


db.test.aggregate([
    {
        $facet: {
            // pipeline-1
            "friendsCount": [
                // stage-1
                { $unwind: "$friends" },
                // stage-2
                { $group: { _id: "$friends", count: { $sum: 1 } } }
            ],
            // pipeline-2
            "educationCount": [
                // stage-1
                {$unwind: "$education"},
                // stage-2
                {$group: { _id: "$education", count: {$sum: 1}}}
                ],
                // pipeline-2
                "skillsCount": [
                    // stage-1
                    {$unwind: "$skills"},
                    // stage-2
                    {$group: { _id: "$skills", count: {$sum: 1}}}
                    ]
        }
    }
])


db.orders.aggregate([
    
    {
        $lookup: {
               from: "test", <!-- kon collection e lookup chalabo -->
               localField: "userId",<!-- tomar local field konta -->
               foreignField: "_id",<!-- tomar local field bideshe giye kon field ke refer kortese -->
               as: "user" <!-- ki name e data pabo -->
             }
    }
    ])

    <!-- IXSCAN method -->
    db.test.find({_id: ObjectId("6406ad65fc13ae5a400000c7")}).explain("executionStats")

    db.massive.createIndex({email: 1}) <!-- unique index -->
    db.massive.dropIndex({email: 1}) <!-- delete index -->
    db.massive.createIndex({gender: -1, age: 1}) <!-- compound index -->
    db.massive.createIndex({about: "text"}) <!-- create text index -->
    db.massive.find({ $text: {$search: "dolor"}}).project({about: 1})
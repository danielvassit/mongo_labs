# Task

You have a document collection that looks like below 
```json
{ 
    "_id" : { "$oid" : "50b59cd75bed76f46522c34e" }, 
    "student_id" : 0, 
    "class_id" : 2, 
    "scores" : [ 
        { 
            "type" : "exam", 
            "score" : 57.92947112575566 
        }, 
        { 
            "type" : "quiz", 
            "score" : 21.24542588206755 
        }, 
        { 
            "type" : "homework", 
            "score" : 68.19567810587429 
        }, 
        { 
            "type" : "homework", 
            "score" : 67.95019716560351 
        }, 
        { 
            "type" : "homework", 
            "score" : 18.81037253352722 
        } 
    ] 
}
```

There are documents for each student (student_id) across a variety of classes (class_id). Note that not all students in the same class have the same exact number of assessments. Some students have three homework assignments, etc.

Your task is to calculate the class with the best average student performance. This involves calculating an average for each student in each class of all non-quiz assessments and then averaging those numbers to get a class average. To be clear, each student's average should include only exams and homework grades. Don't include their quiz scores in the calculation.

What is the class_id which has the highest average student performance? Choose the correct class_id below.

# Prerequisites

Docker and mongodb needs to be installed

```
brew install mongodb
brew install docker
```

# How to run lab

Start compose file
```
docker-compose up -d 
```

Import data from grades.json file and connect to mongo container with mongo client

```
mongoimport --db lab4 --collection grades --file grades.json --drop
mongo
```

# Resolution

1. Run following commands and explore output, the class id should be 1
```json
 db.grades.aggregate([ 
    { 
        $unwind: "$scores"
    }, 
    { 
        $match: { "scores.type": { $ne: "quiz" } } 
    }, 
    { 
        $group: { _id: "$class_id", avg: { $avg: "$scores.score" } } 
    }, 
    { 
        $sort: { "avg":1 } 
    } 
])
```

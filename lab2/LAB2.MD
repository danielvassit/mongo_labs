# Task

You have a document collection that looks like below 
```json
{ 
    "number" : "1" 
}
```
1. Create query in mongo cli that will sum all numbers in collection
2. Create query in mongo cli that will show maximum value in collection

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

Import data from data.json file and connect to mongo container with mongo client

```
mongoimport --db lab2 --collection numbers --file data.json --drop
mongo
```

# Resolution

1. Run following commands and explore output, you should see 8
```json
db.numbers.aggregate([ 
    { 
        $match: {}
    }, 
    { 
        $group: { 
            _id: '', 
            "sum": { $sum: "$number" } 
        } 
    } 
])
```

2. Run following commands and explore output, you should see 3
```json
db.numbers.aggregate([ 
    { 
        $match: {}
    }, 
    { 
        $group: { 
            _id: '', 
            "sum": { $max: "$number" } 
        } 
    } 
])
```
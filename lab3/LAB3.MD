# Task

For companies in our collection founded in 2004 and having 5 or more rounds of funding, calculate the average amount raised in each round of funding. Which company meeting these criteria raised the smallest average amount of money per funding round? You do not need to distinguish between currencies. Write an aggregation query to answer this question.

As a check on your solution, Facebook had the largest funding round average.

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

Import data from companies.json file and connect to mongo container with mongo client

```
mongoimport --db crunchbase --collection companies --file companies.json --drop
mongo
```

# Resolution

1. Run following commands and explore output, you should see that Nimbit is the answer.
```json
db.companies.aggregate([ 
    { 
        $match: { "founded_year": 2004 }
    }, 
    { 
        $project: { _id:1, "name":1, num_rounds: { $size: "$funding_rounds" }, "funding_rounds":1 } 
    }, 
    { 
        $match: { "num_rounds": { $gte: 5 }}
    }, 
    { 
        $unwind: "$funding_rounds" 
    }, 
    { 
        $group: { _id: "$name", avg: { $avg: "$funding_rounds.raised_amount" } } 
    }, 
    { 
        $sort: { "avg":1 } 
    } 
])
```

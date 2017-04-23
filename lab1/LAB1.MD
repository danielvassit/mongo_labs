# Task

You have a document collection that looks like below 
```json
{ 
    "firstname" : "Daniel", 
    "middlename" : "Patryk", 
    "lastname" : "Kozlowski" 
}
```
Create index on firstname key and check that when looking for a name index is used.

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
mongoimport --db lab1 --collection people --file data.json --drop
mongo
```

# Resolution

Run following commands and explore output, you should see "stage" : "IXSCAN" in output.
```json
db.people.createIndex({"firstname":1})
db.people.explain().find({ "firstname" : "Daniel" })
```
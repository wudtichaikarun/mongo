# 10 WORKING WITH INDEXES

- [ ] 1. Why indexes
- [ ] [2. Adding a Single Field Index](#2)
- [ ] 3. Mongo command
- [ ]

---

## 1. Why indexes

> Why do we use Indexes

![why-indexes](./images/10-working-with-indexes/why-indexes.png)

<br/>

> Don't Use Too Many Indexes

Because when we add a new document, we also have to add a new element to the index.

![don't-use-many-indexes](./images/10-working-with-indexes/many-indexes.png)

---

## <a name="2">2. Adding a Single Field Index</a>

1.connect mongo-local-db

Terminal-1

```
> mongod --dbpath /usr/local/var
```

2.Add data from file persons.json to mongo-local-db db name contactData document name contacts run command at the root project directory

Terminal-2

```
> mongoimport persons.json -d contactData -c contacts --jsonArray
```

## 3. Mongo command

> Mongo command (after 1,2) at Terminal-3 run command `mongo`

1. `show dbs`

Terminal-3

```
> show dbs
admin        0.000GB
config       0.000GB
contactData  0.003GB
local        0.000GB
>
```

2. `use contactData`

```
> show dbs
admin        0.000GB
config       0.000GB
contactData  0.003GB
local        0.000GB
> use contactData
switched to db contactData
>
```

3. `show collections`

```
> show dbs
admin        0.000GB
config       0.000GB
contactData  0.003GB
local        0.000GB
> use contactData
switched to db contactData
> show collections
contacts
>
```

4. findOne `db.contacts.findOne()` return random data

```JSON
> db.contacts.findOne()
{
	"_id" : ObjectId("5e0c6b63b9525e53802ea74e"),
	"gender" : "male",
	"name" : {
		"title" : "mr",
		"first" : "victor",
		"last" : "pedersen"
	},
	"location" : {
		"street" : "2156 stenbjergvej",
		"city" : "billum",
		"state" : "nordjylland",
		"postcode" : 56649,
		"coordinates" : {
			"latitude" : "-29.8113",
			"longitude" : "-31.0208"
		},
		"timezone" : {
			"offset" : "+5:30",
			"description" : "Bombay, Calcutta, Madras, New Delhi"
		}
	},
	"email" : "victor.pedersen@example.com",
	"login" : {
		"uuid" : "fbb3c298-2cea-4415-84d1-74233525c325",
		"username" : "smallbutterfly536",
		"password" : "down",
		"salt" : "iW5QrgwW",
		"md5" : "3cc8b8a4d69321a408cd46174e163594",
		"sha1" : "681c0353b34fae08422686eea190e1c09472fc1f",
		"sha256" : "eb5251e929c56dfd19fc597123ed6ec2d0130a2c3c1bf8fc9c2ff8f29830a3b7"
	},
	"dob" : {
		"date" : "1959-02-19T23:56:23Z",
		"age" : 59
	},
	"registered" : {
		"date" : "2004-07-07T22:37:39Z",
		"age" : 14
	},
	"phone" : "23138213",
	"cell" : "30393606",
	"id" : {
		"name" : "CPR",
		"value" : "506102-2208"
	},
	"picture" : {
		"large" : "https://randomuser.me/api/portraits/men/23.jpg",
		"medium" : "https://randomuser.me/api/portraits/med/men/23.jpg",
		"thumbnail" : "https://randomuser.me/api/portraits/thumb/men/23.jpg"
	},
	"nat" : "DK"
}
>
```

5. run a query and let's find all people who are older than 60 `db.contacts.findOne({"dob.age": {$gt: 60}})`

```JSON
> db.contacts.findOne({"dob.age": {$gt: 60}})
{
	"_id" : ObjectId("5e0c6b63b9525e53802ea755"),
	"gender" : "female",
	"name" : {
		"title" : "mrs",
		"first" : "madeleine",
		"last" : "till"
	},
	"location" : {
		"street" : "im winkel 166",
		"city" : "villingen-schwenningen",
		"state" : "baden-württemberg",
		"postcode" : 32227,
		"coordinates" : {
			"latitude" : "83.3998",
			"longitude" : "-172.3753"
		},
		"timezone" : {
			"offset" : "+5:30",
			"description" : "Bombay, Calcutta, Madras, New Delhi"
		}
	},
	"email" : "madeleine.till@example.com",
	"login" : {
		"uuid" : "e70766b6-9d4f-419f-85c5-efc3c42db023",
		"username" : "goldencat450",
		"password" : "panthers",
		"salt" : "mHOsDM43",
		"md5" : "a97539f5d7b6c7302d3f3c1dd1d389b5",
		"sha1" : "a991dd190a83856b04d41a5e64bc41b5728b6903",
		"sha256" : "bd123b2b2aae61c6e8d3449bb14c6a032b4860275da781f8ffc73f5500e667bd"
	},
	"dob" : {
		"date" : "1954-05-01T02:34:40Z",
		"age" : 64
	},
	"registered" : {
		"date" : "2008-06-14T03:14:37Z",
		"age" : 10
	},
	"phone" : "0209-9573743",
	"cell" : "0173-1226290",
	"id" : {
		"name" : "",
		"value" : null
	},
	"picture" : {
		"large" : "https://randomuser.me/api/portraits/women/27.jpg",
		"medium" : "https://randomuser.me/api/portraits/med/women/27.jpg",
		"thumbnail" : "https://randomuser.me/api/portraits/thumb/women/27.jpg"
	},
	"nat" : "DE"
}
```

5. find count `db.contacts.find({"dob.age": {$gt: 60}}).count()`

```
> db.contacts.find({"dob.age": {$gt: 60}}).count()
1222
>
```

## 4. Tool to analyze

6.  Mongodb gives use a Tool to analyze how it executed the quey just add `explain()` keyword after document and ex `db.contacts.explain().find({"dob.age": {$gt: 60}})`

```JSON
> db.contacts.explain().find({"dob.age": {$gt: 60}})
{
	"queryPlanner" : {
		"plannerVersion" : 1,
		"namespace" : "contactData.contacts",
		"indexFilterSet" : false,
		"parsedQuery" : {
			"dob.age" : {
				"$gt" : 60
			}
		},
		"winningPlan" : {
			"stage" : "COLLSCAN",
			"filter" : {
				"dob.age" : {
					"$gt" : 60
				}
			},
			"direction" : "forward"
		},
		"rejectedPlans" : [ ]
	},
	"serverInfo" : {
		"host" : "wudtichais-MBP",
		"port" : 27017,
		"version" : "4.0.3",
		"gitVersion" : "7ea530946fa7880364d88c8d8b6026bbc9ffa48c"
	},
	"ok" : 1
}
>
```

7 Tool to analyze. arguments `.explain("executionStates").`

```JSON
> db.contacts.explain("executionStats").find({"dob.age": {$gt: 60}})
{
	"queryPlanner" : {
		"plannerVersion" : 1,
		"namespace" : "contactData.contacts",
		"indexFilterSet" : false,
		"parsedQuery" : {
			"dob.age" : {
				"$gt" : 60
			}
		},
		"winningPlan" : {
			"stage" : "COLLSCAN",
			"filter" : {
				"dob.age" : {
					"$gt" : 60
				}
			},
			"direction" : "forward"
		},
		"rejectedPlans" : [ ]
	},
	"executionStats" : {
		"executionSuccess" : true,
		"nReturned" : 1222,
		"executionTimeMillis" : 4,
		"totalKeysExamined" : 0,
		"totalDocsExamined" : 5000,
		"executionStages" : {
			"stage" : "COLLSCAN",
			"filter" : {
				"dob.age" : {
					"$gt" : 60
				}
			},
			"nReturned" : 1222,
			"executionTimeMillisEstimate" : 0,
			"works" : 5002,
			"advanced" : 1222,
			"needTime" : 3779,
			"needYield" : 0,
			"saveState" : 39,
			"restoreState" : 39,
			"isEOF" : 1,
			"invalidates" : 0,
			"direction" : "forward",
			"docsExamined" : 5000
		}
	},
	"serverInfo" : {
		"host" : "wudtichais-MBP",
		"port" : 27017,
		"version" : "4.0.3",
		"gitVersion" : "7ea530946fa7880364d88c8d8b6026bbc9ffa48c"
	},
	"ok" : 1
}
>
```

# A (Gentle) Introduction to MongoDB

**What is MongoDB?**

MongoDB is made up of *databases* which contain *collections*. A *collection* is made up of *documents*. Each *document* is made up of *fields*. *Collections* can be *indexed*, which improves lookup and sorting performance. Data is retrieved through a *cursor* whose actual execution is delayed until necessary.

* * *

**How do we get MongoDB?**

    brew install mongodb

* * *
   
**How do we start MongoDB?**

    sudo mongod | tail -f /usr/local/var/log/mongodb/mongo.log 

* * *
  
**How do we insert data?**

    db.blog.insert({title: 'I am a title', content: 'Yadda, yadda, yadda', date: new Date(), tags: ['tag1', 'tag2'], upvotes: 1})

* * *

**Where's the schema?**

Implicitly defined in the application(s).

* * *

**How do we list the collections in the DB?**

    db.getCollectionNames()

* * *

**How do we list the documents in a collection?**

    db.coll.find()

* * *

**What's up with that `_id` field?**

* `db.system.indexes.find()`

* `ObjectId("505bd76785ebb509fc183733").getTimestamp()`

* * *

**Where is some test data?**

Here you go:

```javascript
(function() {
	var titlePrefix = ['Some', 'More', 'About', 'Consider The'],
			titleSuffix = ['Stuff', 'Things', 'Problems'],
			tags = ['omg', 'humm', 'wtf'],
			upvoteDelta = 16,
			maxDocuments = 10,
			rand = function(max) { return Math.floor(Math.random() * max); },
			decide = function(what) { return rand(2) > 0; },
			i;

	for (i = 0; i < maxDocuments; ++i) {
		  var doc = {
		  	title: titlePrefix[rand(titlePrefix.length)] + ' ' + titleSuffix[rand(titleSuffix.length)],
		  	content: 'Yadda, yadda, yadda',
		  	date: (function(d) { return new Date(d.setDate(d.getDate() + rand(maxDocuments))); })(new Date()),
		  	upvotes: rand(upvoteDelta) * (decide('if negative') ? 1 : -1),
		  	tags: (function() {
	  			var n = rand(tags.length),
	  		    	    ts = tags.length;
	  			return n > 1 ? randInts(n, ts).map(function(i) { return tags[i]; }) : tags[rand(ts)];
		  	})()
  		};

  		if (decide('has comments')) 
  			doc.comments = ['Some Comment', 'Another Comment'];

			db.blog.insert(doc);
  }

  function randInts(howMany, maxValue) {
  	var values = new Array(howMany),
  			i;

		for (i = 0; i < howMany; ++i) 
			values[i] = rand(maxValue);

		return values;
  }
})();
```

* * *

**How do I query a collection?**

In lots of ways:

* `db.blog.find()`
* `db.blog.find().count()`
* `db.blog.find({upvotes: {$gte: 0}})`
* `db.blog.find({upvotes: {$gte: 0}}).count()`
* `db.blog.find({comments: {$exists: true}})`
* `db.blog.find({upvotes: {$gte: 0}, comments: {$exists: true}})`
* `db.blog.find({upvotes: {$gte: 0}, $or: [{tags: 'omg'}, {tags: 'wtf'}]})`
* `db.blog.find({_id: ObjectId("TheObjectId")})`
* `db.blog.find().sort({upvotes: -1})`
* `db.blog.find(null, {tags: 1, upvotes: 1}).sort({tags: 1, upvotes: -1})`
* `db.blog.find().sort({upvotes: -1}).limit(5).skip(2)`
* `db.blog.find({$where: function() { return this.tags instanceof Array; }})`
* ...

* * *

**How do I update a document?**

    db.blog.update({title: 'I am a title'}, {content: 'Some interesting content'})

* * *

**Oops.**

    db.blog.remove({remove: 'Some interesting content'})
    db.blog.insert({title: 'I am a title', content: 'Yadda, yadda, yadda', date: new Date(), tags: ['tag1', 'tag2'], upvotes: 1})
    db.blog.update({title: 'I am a title'}, {$set: {content: 'Some interesting content'}})

**What more can I do with update?**

* `db.blog.update({title: 'I am a title'}, {$inc: {upvotes: -1}})`
* `db.blog.update({title: 'I am a title'}, {$push: {tags: 'another tag'}})`

* * *

**What about upserts?**

    db.blog.update({title: 'I am not a title'}, {$inc: {upvotes: 3}}, true)

* * *

**Are there multiple updates?**

    db.blog.update({}, {$set: {public: true}}, false, true)
    
* * *

**Cute. Now show us some aggregations!**

```javascript
var map = function() {
	var i;

	if (this.tags === void 0)
		return;

	if (!(this.tags instanceof Array)) {
		emit(this.tags, {count: 1});
		return;
	}

	for (i in this.tags)
		emit(this.tags[i], {count: 1});
}

var reduce = function(key, values) {
	var sum = 0;

	values.forEach(function(value) {
		sum += value.count;
	});

	return sum;
}

db.blog.mapReduce(map, reduce, {out: {inline: 1}})
```

* * *

**Are there indexes?**

Sure:

* `db.blog.ensureIndex({title: 1})`
* `db.blog.dropIndex({title: 1})`

* * *

**Can they be unique?**

No problem: `db.blog.ensureIndex({title: 1}, true)`

* * *

**Do queries actually use them?**

* `db.blog.find().explain()`
* `db.blog.find({title: "I am a title"}).explain()`

* * *

**What about concurrency?**

```java
DB db...;
db.requestStart();
try {
   db.requestEnsureConnection();

   code....
} finally {
   db.requestDone();
}
```

* * *		

**Tell us more!**

* Drivers in lots of languages
* Fire and forget writes: call `db.getLastError()` to be notified about a failed write 
* Capped collections: `db.createCollection('logs', {capped: true, size: 1048576})`
* Bulk inserts
* Geospatial
* Replication  (for availability)
* Durability with `journal = true`
* Consistency *à la carte* (e.g., `db.runCommand({ getlasterror : 1 , w : "majority" })`)
* Configurable read preference
* Auto-sharding
* Stats: `db.stats()`
* [The Manual](http://docs.mongodb.org/manual/contents/ "MongoDb Manual")

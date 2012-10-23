# Mad-Reduce

* Computation closer to the data
* Minimize cluster traffic
* Parallelization
* Based on the same functional ideas
* Map produces key-value pairs
* Reduce combines map outputs with the same key
* By example

```python
from post in posts
from tag in post.Tags
select new { Name = tag.ToString().ToLower(), Count = 1 };
 
from tagCount in results
group tagCount by tagCount.Name into g
select new { Name = g.Key, Count = g.Sum(x => x.Count) }
```

* Reduce should be combinable
* Map-reduce pipeline
* Incremental
* Feeds materialized views
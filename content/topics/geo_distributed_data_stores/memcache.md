---
title: "Memcache"
geekdocDescription: "Scaling Memcache at Facebook"
keywords: ["cs7210", "distributed computing", "omscs", "memcache"]

date: 2021-10-15T06:31:31+05:30
draft: false
---

# Scaling Memcache at Facebook


Content taken from paper and presentation at <a href="https://www.usenix.org/conference/nsdi13/technical-sessions/presentation/nishtala" target="_blank">Scaling Memcache at Facebook</a> NSDI '13


# Basics
###  Writes
* Memcache needs to be invalidated after DB write
* Up to web application to specify which keys to invalidate after database update

![image-20211015080454176](/images/cs7210/geo_distributed_datastores/image-20211015080454176.png)

### Reads
* Two orders of magnitude more reads than writes
* Common case is data is available in the cache
* Demand-filled look-aside cache

![image-20211015080639164](/images/cs7210/geo_distributed_datastores/image-20211015080639164.png)

### Challenges
1. Keeping database and cache in sync
2. Protected database from cache failure. If for some reason, cache fails and the entire read traffic falls on the database, it will not be able to keep up.

# Questions

**Why writers invalidate the cache instead of setting a new value?**
* Deletes are idempotent and commutative
* Allows cache to be filled based on demand

**Why do writers delete at all?**

To support read-your-own-writes semantics

**What are the reasons that a key may not be present in cache?**
1. Key was never queried since the memcache was started
2. Key was evicted to make space for another key
3. Key was invalidated upon update

# Summary of Problems

### Scale: Single Memcache Server
  1. **Stale Sets**
    * Case 1: reader-reader: Assign "Lease" to a webserver which will own the update. Other webservers back-off from updating the cache.
    * Case 2: reader-writer: If a writer invalidates a key in memcache, it invalidates any active lease as well
  2. **Thundering Herd** solved by making a webserver wait if lease is issued to another webserver
### Scale: Multiple Memcache Servers (Sharded)
  1. **Incast Congestion at Web Servers:** Put an upper bound on sharding. Instead of adding more shards, replicate clusters.
### Scale: Multiple Frontend Clusters(Frontend Server + Memcache Server)
  1. **Multiple Memcached to be invalidated:** Setup a separate async invalidation pipeline
  2. **Too Many Invalidation Packets**: Make invalidation pipeline hierarchical
  3. **Individual Memcache Failing**: Have a backup memcache (gutter deployment) which acts as backup for all clusters
  4. **High Load on Cold Start:** Warm up the cache from other caches.
### Scale: Geographically Distributed Datacenters
  1. **Race between DB replication and subsequent DB read:** Remote Markers



## Problems

### Stale Sets

* Reader-Writer Race
  1. Reader reads old value
  2. writer update db and deletes key from memcache
  3. Reader writes old value to cache
* Reader-Reader Race
  1. Reader1 reads old

* Extend memcache protocol with “leases”
  * 

![image-20211015070050888](/images/cs7210/geo_distributed_datastores/image-20211015070050888.png)

### Thundering Herds

* If a hot key is invalidated from memcache, a lot of requests from webservers will go to database directly. This increases the load on database suddenly.
* Solution is extension of **Lease** mechanism. If a webserver is shouldering the responsibility to update the memcache (by holding the lease), other webservers wait instead of querying database.

![image-20211015070435807](/images/cs7210/geo_distributed_datastores/image-20211015070435807.png)



### Incast Congestion

* Many simultaneous responses overwhelm sharednetworking resources
* Solution: Limit the number of outstanding requests with a sliding window
  * Larger windows cause result in more congestion
  * Smaller windows result in more round trips to the network

![image-20211015071518959](/images/cs7210/geo_distributed_datastores/image-20211015071518959.png)



### Multiple Memcached to be invalidated

* Tail the mysql commit log and issue deletes based on transactions that have been committed

![image-20211015072233313](/images/cs7210/geo_distributed_datastores/image-20211015072233313.png)



### Too Many Invalidation Packets

* If `McSqueal` has to update all the memcaches individually, it will lead to 
  * too many packets across data centers.
  * poor performance of McSqueal
* Having a hierarchy of Memcache Routers solves this problem. 
* For eg. only invalidation needs to be sent across datacenter.

![invalidation_pipeline2](/images/cs7210/geo_distributed_datastores/invalidation_pipeline2.png)



### High Load on Cold Start

* If a new cluster is starting

### Race between DB replication and subsequent DB read

* When we have multiple datacenters, writes in non-primary datacenter can lead to reace between db replication and subsequent db read.
* Remote markers: a special flag that indicates whether a race is likely
* When a webserver updates a key-value, it sets _Remote Marker_ on that key in memcache.
* Any reading webserver follows following logic

```
If miss
	If marker set
 		read from master DB
	else
 		read from replica DB
```



![image-20211015073829549](/images/cs7210/geo_distributed_datastores/image-20211015073829549.png)



## Lesson Learnt

* Push complexity into the client whenever possible
* Separating cache and persistent store allows them to be scaled independently

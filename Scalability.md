# Scalability: A Summary

Qualities to look for in a hosting website: SFTP, caution of 'unlimited' offers

Shared Web-Host:
VPS: Virtual Private Servers
-Takes

## Clones
*Golden Rule of Scalability*: every server contains exactly the same codebase and does not store any user-related data, like sessions or  profile pictures, on local disc or memory

- sessions should be stored in centralized, accessible database or persistent cache like Redis (better perf. than external database, or db near the data center of app servers)

- in deployment, solving the issue of applying code changes to multiple servers can be solved by Capistrano (requires some learning esp if not into Ruby on Rails)

-when codebases are uniform, you can create an image (Amazon Machine Image for AWS) as a 'super clone' from which to draw from

## Database
-Even with horizontal scaling, MySQL leads to slower performance and eventual breakdown.

There are two paths to solving this issue:

#### Path 1:
Stick to MySQL, hire a databse admin (DBA), do master-slave replication (slave -> master) and add RAM to master server.

DBA will use terms like 'sharding', 'denormalization', and 'SQL tuning'. 

#### Path 2:
denormalize from the beginning and include no Joins in any db query. Use MySQL as NoSQL if staying, or switch to an easier NoSQL i.e. MongoDB, CouchDB. Joins in app code (sooner the better). Even so, db requests will again be slower and slower and need a cache.

## Cache
-Cache means in=memory caches like Memcached or Redis. Never do file-based caching, as it makes cloning and auto-scaling of servers a pain

-is a simple key-value store and should reside as a buffering layer between app and data storage 

-1st app should retrieve from cache, only then from the data source, since CACHE is fast and holds every dataset in RAM. 

2 data caching patterns:
#### Cached Dataase Queries
-most common
-query db, store result dataset in cache
-hashed ver. of query is cache key
-issues: expiration, it is hard to delete cache if it is a complex query, and changing one table cell would require deleting all cached queries including that table cell

#### Cached Objects
-preferable pattern
-let class assemble dataset from db and store complete instance in cache

-Product
    -data
        -prices, texts, pictures, reviews
        -filled by several methods doing several req that are harder to cache

-once class is ifnished assembling the data array, store the array or the complete instance of the class, in the cache. 
-allows you to get rid of the object whenever something did change
-**allows asynch processing** possible. 
-objects to cache: user sessions (never in db), fully rendered blog articles, activity streams, user <-> friend relationships
-Two ways to cache via Redis and Memcached.
-Redis: has extra db features like persistence, built-in data structures i.e. sets and lists
Memacached: if only caching, and scales easily

## Asynchronism
-imagine buying bread and being told to wait for 2 hours before it's ready--asynch avoids this

### Paradigms of Asynchronism:
#### Async #1
-doing time consuming work in advance and serving the finished work at low request time
-dynamic --> static
-pages of a website built by a framework of CMS, are rendered and stored locally as static HTML w/ each change
-Often these computing tasks are done on a regular basis, possibly by a script called /hr by cronjob. 
-pre-computing makes websites and apps performant and scalable
-imagine the scalability of your website if the script would upload these pre-rendered HTML pages to AWS S3 or Cloudfront or another Content Delivery Network!

#### Async #2
What if there is something that can't be pre-loaded, or a query that is custom?

-front end sends signal for a job, backend signal back it is being worked on, and front end tells a user to continue browsing while querying the back if the job is done. When the job completes, it is sent back up the chain and the user is notified.
-reccom: first 3 tutorials on RabbitMQ, one of many systems which help to implement async processing. ActiveMQ or Redis lsit. 
-Have a queue of tasks for worksers
-time consuming? do it asynchronously
# Scalability: A Summary
![CS75 (Summer 2012) Lecture 9 Scalability Harvard Web Development David Malan](https://www.youtube.com/watch?v=-W9F__D3oY4)

Qualities to look for in a hosting website: SFTP, caution of 'unlimited' offers

VPS: Virtual Private Server (Like AWS), A virtual private server is a virtual machine sold as a service by an Internet hosting service

Shared Web-Host:
VPS: Virtual Private Servers
-Takes

# Scalability for Dummies: Notes
## Clones
*Golden Rule of Scalability*: every server contains exactly the same codebase and does not store any user-related data, like sessions or  profile pictures, on local disc or memory

- Sessions should be stored in centralized, accessible database or persistent cache like Redis (better perf. than external database, or db near the data center of app servers)

- In deployment, solving the issue of applying code changes to multiple servers can be solved by Capistrano (requires some learning esp if not into Ruby on Rails)

- When codebases are uniform, you can create an image (Amazon Machine Image for AWS) as a 'super clone' from which to draw from

Summary: updating the codebase uniformly requires access to a central db or persistent cache, using a tool liek Capistrano, then forming a 'super clone' to propogate further.

## Database
- Even with horizontal scaling, MySQL leads to slower performance and eventual breakdown.

There are two paths to solving this issue:

#### Path 1:
Stick to MySQL, hire a databse admin (DBA), do master-slave replication (slave -> master) and add RAM to master server.

DBA will use terms like 'sharding', 'denormalization', and 'SQL tuning'. 

#### Path 2:
denormalize from the beginning and include no Joins in any db query. Use MySQL as NoSQL if staying, or switch to an easier NoSQL i.e. MongoDB, CouchDB. Joins in app code (sooner the better). Even so, db requests will again be slower and slower and need a cache.

Summary of 2 paths: (1) Use MySQL and apply master-slave replication and expanding RAM on the master server, and (2) remove joins in db query and convert the MySQL to a NoSQL in usage or migrate to MongoDB, CouchDB and place joins in app code.

## Cache
- Cache means in-memory caches like Memcached or Redis. Never do file-based caching, as it makes cloning and auto-scaling of servers a pain

- Is a simple key-value store and should reside as a buffering layer between app and data storage 

- 1st app should retrieve from cache, only then from the data source, since CACHE is fast and holds every dataset in RAM. 

Summary: Cacheing is faster, but never do file-based. It is stored as a key-value between app and storage, and any query should first go down the chain from the cache, then and only then to the source.

2 data caching patterns:
#### Cached Dataase Queries
- Most common
- Query db, store result dataset in cache
- Hashed ver. of query is cache key
- issues: expiration, it is hard to delete cache if it is a complex query, and changing one table cell would require deleting all cached queries including that table cell

Summary: hashed version of query is stored as cache key for the dataset in the cache, but maye have issues with complex queries or changing a table cell.

#### Cached Objects
- Preferable pattern
- Let class assemble dataset from db and store complete instance in cache

- Product
    - Data
        - Prices, texts, pictures, reviews
        - Filled by several methods doing several req that are harder to cache

- Once class is finished assembling the data array, store the array or the complete instance of the class, in the cache. 
- Allows you to get rid of the object whenever something did change
- **Allows asynch processing** possible. 
- Objects to cache: user sessions (never in db), fully rendered blog articles, activity streams, user <-> friend relationships
- Two ways to cache via Redis and Memcached.
- Redis: has extra db features like persistence, built-in data structures i.e. sets and lists
Memacached: if only caching, and scales easily

Summary: Cached Objects is the preferred pattern, where the class stores complete instance in cache after assembly. If change occurs, discard object, and allows asynch. Redis is more feature rich in caching. 

## Asynchronism
- Imagine buying bread and being told to wait for 2 hours before it's ready--asynch avoids this

### Paradigms of Asynchronism:
#### Async #1
- Doing time consuming work in advance and serving the finished work at low request time
- Dynamic --> static
- Pages of a website built by a framework of CMS, are rendered and stored locally as static HTML w/ each change
- Often these computing tasks are done on a regular basis, possibly by a script called /hr by cronjob. 
- Pre-computing makes websites and apps performant and scalable
- Imagine the scalability of your website if the script would upload these pre-rendered HTML pages to AWS S3 or Cloudfront or another Content Delivery Network!

Summary: Preloading dynamic pages into static HTML allows serving at low request times, and computing tasks done regularly.

#### Async #2
What if there is something that can't be pre-loaded, or a query that is custom?

- Front end sends signal for a job, backend signal back it is being worked on, and front end tells a user to continue browsing while querying the back if the job is done. When the job completes, it is sent back up the chain and the user is notified.
- Reccom: first 3 tutorials on RabbitMQ, one of many systems which help to implement async processing. ActiveMQ or Redis list. 
- Have a queue of tasks for worksers
- Time consuming? do it asynchronously

Summary: Second method of asynch is to queue tasks, and using systems to implenet asynchronous processing like ActiveMQ or Redis list
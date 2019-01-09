Most materials from https://www.hiredintech.com/classrooms/system-design/lesson/52

We can give a few examples of such questions:

- Design a URL shortening service like bit.ly.
- How would you implement the Google search?
- Design a client-server application which allows people to play chess with one another.
- How would you store the relations in a social network like Facebook and implement a feature where one user receives notifications when their friends like the same things as they do?

The idea of these questions is to have a discussion about the problem at hand. What’s important for the interviewer is the process, which you use to tackle the problem. The typical outcome of such a discussion is a high-level architecture addressing the goals and constraints in the question. Perhaps the interviewer will choose one or more areas where they will want to discuss bottlenecks and other common problems.

Finally, keep in mind that the discussion about the same system design problem could go in different directions depending on the goals of the interviewer. They may be willing to see how you create a high-level architecture covering all aspects of the system. Or rather, they could be more interested in looking at a few specific areas and diving deeper into them. In any case, you should have a strategy for how to approach the different situations.

Some concepts involved with system design with big data or traffic:
SOA: service oriented architecture
distributed system: 
relational database vs non-relational database
n-tiered software architecture
scalability
availability: minimize down time, need all backup mechanism to prevent single point failure.
caching: put in memory to speed up queries
load balance
process and threads
storage
microservice
partition: group the database into different machine
horizontal vs vertical scaling
top down design: 
sharding:a single database breaking into pieces across multiple machines.
database replication
basic knowledge in internet, os and some other common sense knowledge.
Every design shall meet the current requirement and leaving space for scaling. The system shall be scalable.

construtive way to approach a system design question
### requirement and use cases
The very first thing you should do with any system design question is to clarify the system's constraints and to identify what use cases the system needs to satisfy. Spend a few minutes questioning your interviewer and agreeing on the scope of the system. Many of the same rules we discussed while talking about algorithm design apply here as well.

Usually, part of what the interviewer wants to see is if you can gather the requirements about the problem at hand, and design a solution that covers them well. Never assume things that were not explicitly stated.

ask and write down the requirement and use cases
example: shortening the URL.
1 the basic task
- shortening the url
- redirect the minURL to original URL

2 use cases and check with the requirements
x custom urls
x statistics
x link expiration
x UI or API
v high availability

3 constraints
url: up to 500 chars
request to shorten: 10%
request to redirect: 90%
400 requests per second
the storage for 5 year: url and hash table is 3TB and 40GB each
data per second ~200KB
You need do some estimations on the problem size based on your knowledge.

### abstract system design
this is the high level system design. You shall use diagrams and connection to display the architecture.
this sort of high-level design is a combination of well-known techniques, which people have developed. You have to make sure you are familiar with what's out there and feel comfortable using this knowledge.
for example, n-tiered, SOA. 
URL shortening
- application layer
  - shortening service
  - redirection service
- data storage layer
  - a hash table
  - use 0-9a-zA-Z total 62 chars to hash %62
  - to support the billion url address 6 chars are enough 62^6
  
### understanding the bottle neck
Perhaps your system needs a load balancer and many machines behind it to handle the user requests. Or maybe the data is so huge that you need to distribute your database on multiple machines. What are some of the downsides that occur from doing that? Is the database too slow and does it need some in-memory caching?

usually each solution is a trade-off of some kind. Changing something will worsen something else. However, the important thing is to be able to talk about these trade-offs, and to measure their impact on the system given the constraints and use cases defined.

url shortening
- traffic is moderate and data is moderate 
- query in a 4TB database is slow
- hashtable could be indexed in the memory

### The most important: scalable system
professor David Malan from Harvard's video course on the scaling.
Briefs:
vertical scaling: upgrade the machine, more ram, more disk, more cpu power, more cores.
horizontal scaling: add more machine nodes with balance loader.
multiple servers: such as google, nslookup and we get a list of machines
balance loader:
client browser:
option 1: return the node ip for the load balance. Other servers then can be private. Single point.
need to communicate with servers to determine the load for each server.

option 2: dns return a sever ip each time different. using TTL (time to expire) or balance load send cookies to keep the session alive.
option 3: dedicated server for different functions, no redundancy. and some servers may be overloaded.

sticky sessions: need to keep the session data and use the same server connections.

redundancy: to prevent single point failure.
RAID tech to backup data. (simultaneously write)
iSCSI

backup: disk, single point data base, balance loader, regional data center
query cache: put it in memory, some mechanism to remove oldest data (expiration), compression
replication: master-master, master-slave (write to master, read from slave)
n-tier architecture
master-slave: communication with heart beat
partion: some rule to partition
sharding
database layer also buffered with balance load to eliminate cross-connection between layers
switch: connecting all to switch and switch shall be duplicated.

more reading:
“Scalability for Dummies” 
horizontal scaling
servers behind load balance: servers shall contain same code base so different server gets same results and does not store any user-related data, like sessions or profile pictures, on local disc or memory. Sessions need to be stored in a centralized data store which is accessible to all your application servers. It can be an external database or an external persistent cache, like Redis. An external persistent cache will have better performance than an external database.

denormalize database
when database gets bigger, query becomes slower. You can stay with MySQL, and use it like a NoSQL database, or you can switch to a better and easier to scale NoSQL database like MongoDB or CouchDB. Joins will now need to be done in your application code. The sooner you do this step the less code you will have to change in the future.

Cache:
cached query
cached object: object binds user sessions (never use the database!)
fully rendered blog articles
activity streams
user<->friend relationships 

asynchronous:
return immediately and do not block

sharding:
flickr: now handles more than 1 billion transactions per day, responding in less then a few seconds and can scale linearly at a low cost.
High availability. If one box goes down the others still operate.
Faster queries. Smaller amounts of data in each user group mean faster querying.
More write bandwidth. With no master database serializing writes you can write in parallel which increases your write throughput. Writing is major bottleneck for many websites.
You can do more work. A parallel backend means you can do more work simultaneously. You can handle higher user loads, especially when writing data, because there are parallel paths through your system. You can load balance web servers, which access shards over different network paths, which are processed by separate CPUs, which use separate caches of RAM and separate disk IO paths to process work. Very few bottlenecks limit your work.

* data are denormalized
* data are small
* query is fast
* parallized
* high availability
* no replication: master-slave structure needs replication, write to master needs serialization and is a single point fault.

cons:
* rebalance data
* join shards
* how to partition
* not well supported, use at own risk

examples:
When it comes to system design, it's incredibly useful to review real-life architectures. As you do this, make sure you:

- Pay attention to what technologies are used. Go ahead and research each new technology and see what problem it solves, what its alternatives are, where it excels, and where it fails.
- Take note of the common patterns you see, and how they relate to the scalability theory you learned in the previous section.
- Read through the lessons learned - they are a very quick way to learn from other people's battle scars.

http://highscalability.com/flickr-architecture

uber uses up to thousand microservices
- It’s great to break up all your monoliths into smaller things. Even the name monolith sounds bad. But microservices have their bad side.

- The time when things are most likely to break is when you change them. Uber is most reliable on the weekends when engineers aren’t making changes even though that’s when Uber is the busiest.

- Everytime you go to change something you risk breaking it. Does it make sense at some point to never touch a microservice? Maybe.

The Good

It’s worth questioning, why are we throwing so much effort into microservices? It’s not by accident, there are a lot of good results.

Microservices allow teams to be formed quickly and run independently. People are being added all the time. Teams need to be formed quickly and put to work on something where they can reason about the boundaries.

Own your own uptime. You run the code that you write. All the service teams are on call for the services they run in production.

Use the best tool for the job. But best in what way? Best to write? Best to run? Best because I know it? Best because there are libraries? When you dig into it, best doesn’t mean a lot.

everything is a RPC call, everything is a tradeoff

Fanout Issues - Tracing
many repos: one vs more
all kinds of languages

logging
load testing
balance testing

facebook:
livestream to 800k users simultaneously
use cases:
- Needs to be able to serve up millions of simultaneous streams without crashing.
- Need to be able to support millions of simultaneous viewers on a stream, as well as seamless streams across different devices and service providers around the world.

  - start with http
  - rtmp: small latency, but need new architecture
  
 constraints:
 - user requests increases sharply and when ending, decrease like a rock, spiky!
 - a lot of people want to watch the same video Thundering herd problem. 
  - video are split into 1s segments and stored in cache
  - pressure on caching
 - global balance loading
 
 Big Picture Architecture
This is how a live stream goes from one broadcaster to millions of viewers.

A broadcaster starts a live video on their phone.

The phone sends a RTMP stream to a Live Stream server.

The Live Stream server decodes the video and transcodes to multiple bit rates.

For each bit rate a set of one-second MPEG-DASH segments is continuously produced.

Segments are stored in a datacenter cache.

From the datacenter cache segments are sent to caches located in the points of presence (a PoP cache).

On the view side the viewer receives a Live Story.

The player on their device starts fetching segments from a PoP cache at a rate of one per second.

How to scale:
Within the PoP there are two layers: a layer of HTTP proxies and a layer of cache.
Viewers request the segment from a HTTP proxy. The proxy checks if the segment is in cache. If it’s in cache the segment is returned. If it’s not in cache a request for the segment is sent to the datacenter.

Different segments are stored in different caches so that helps with load balancing across different caching hosts.

Protecting The Datacenter From The Thundering Herd
PoPs Are Still At Risk - Global Load Balancing To The Rescue

You already know how to apply it, so we'll be brief. Don't skip steps, don't make assumptions, start broad and go deep when asked.

Second, keep in mind that system design questions serve as an idea exchange platform. Be prepared for discussions about tradeoffs, about pros and cons. Be prepared to give alternatives, to ask questions, to identify and solve bottlenecks, to go broad or deep depending on your interviewer's preferences.

Don't get defensive: whenever your interviewer challenges your architectural choices, acknowledge that rarely an idea is perfect, and outline the advantages and disadvantages of your choice. Be open to new constraints to pop up during the discussion and to adjust your architecture on the fly.

for the url shortening:
discuss the bottle neck
data or database

1. n-tiered 
application layer
--one machine first
- loadbanlace and a a cluster of servers

data layer
database becomes large
mysql: why choose mysql
- fast 
- replica
- caching

create the table:

scaling the database:
vertical scaling:
horizontal scaling:

load testing
profiling

You need be familiar with some core technology, some known products

 

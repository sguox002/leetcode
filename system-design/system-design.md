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





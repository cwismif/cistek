## System Design
---
Problems with IT systems cost tremendous damage to both revenue and reputation.  As an engineering discipline, failures can cost lives. High level system design exists to meet non-functional requirements (NFRs) of a system. It’s important to note that every design is a compromise, there will always be trade-offs and these must be weighed alongside the functional expectations of the system itself.  For example, durability is a key NFR for financial systems.

* CAP Theorem: for distributed database systems, a loss of connectivity means data becomes inconsistent between the nodes or the nodes will no longer be available for write or read requests  
* Environmental constraints: must run on a mobile phone  
* Scalability: can the system continue to perform acceptably as the workload increases  
* Latency: how quickly a system responds to a request. High enough latencies can make some systems unavailable.  
* Durability: this concerns data and its likelihood of loss or corruption, importance depends on the system   
* Security: this depends on the system. How sensitive is the data? How much damage could a compromised system cause?  
* Fault tolerance: components will fail, but can the failure occur without degradation of availability, latency, durability?    
* Compliance: is the system compliant with legal or regulatory standards. Data protection etc.  
* Some common patterns used in system designs:  
* Horizontal scaling: Adding more servers to a system, and that additional having a proportional increase in performance.  Not everything can be scaled like this without a redesign, but it has a much higher ceiling for scaling  
* Vertical scaling: Simpler than horizontal will scale to a limit.  Although, some modern specialised servers are very powerful exceptionally powerful (2Tb Redis Cache)  
* Databases: Analysis of data storage and access patterns determines database technology choice:  
* Relational databases (eg. MySQL): ACID compliant but more complex to scale horizontally  
* NoSQL databases: More flexible schema and easier horizontal scaling, but with various trade-offs  
* Caching: many variants (read/write aside/around/through) of cache as well as their eviction policies \- what data should be prioritised for deleting? They can also reside anywhere. Access patterns and data consistency matter  
* Load balancers: Two types exist: L4 LBs are Faster but have less information for forwarding decisions and L7 LBs are slower but can use client request details in decisions  
* Message Queues: Provide two key benefits: improves performance through asynchronous workload delegation and by adding system resilience by buffering temporary processing fluctuations and replaying the message later.  
* Content Delivery Networks: Improves performance by using geographical proximity to serve data quicker.  Static data is the usual data served, but edge computing is allowing dynamically created data to be served from edge locations.  
* Pub/Sub: A single push event to a topic/channel fans out data to multiple consumers/subscribers.  A great solution used in many modern high volume apps such as Twitter where one post by someone followed by millions results in 99% receiving the post within 5 seconds (these are estimations)  
* UDP/TCP: TCP provides reliable data transmission even when the data is split into packets, some of which may be routed differently, and reassembles the data as it was sent by the sender.  Used for more visiting a banking website for example.  UDP on the other hand is fire-and-forget, a packet loss is acceptable and ordering isn’t a problem.  Tend to be used for video streaming and other data streaming.

---
title: "How Consistent Hashing Works (Step-by-Step Guide with Examples)"
datePublished: 2026-05-25T20:19:38.099Z
cuid: cmplngwky00ie2cjvalnu458x
slug: how-consistent-hashing-works-step-by-step-guide-with-examples

---

***We'll explain how consistent hashing works using a real‑world example: a load balancer assigning requests to multiple servers. In a distributed system, the load balancer needs an algorithm to decide which server should handle each request.***

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/0c5c6ce4-e0fb-4bc7-bd18-6f7d8a1d0957.png align="center")

We will take a simple example of modulo 5.  
For request number 34 its modulo 5 is 4. So this request will be handled by server 4.  
For the simple problem above its fine till now using the modulo algorithm. But then think about these issues: a new server came; Or one of the server got destroyed.; may be every 5th request is an heavy input/output requests.  
  
Consistent Hashing is an algorithm that helps in dealing with the above issues.  
Lets understand how this algorithm work step by step.  
  
First choose a hashing function. Now what is a hashing function?  
According to wikipedia: "A **hash function** is any [function](https://en.wikipedia.org/wiki/Function_\(mathematics\)) that can be used to map [data](https://en.wikipedia.org/wiki/Digital_data) of arbitrary size to fixed-size values, though there are some hash functions that support variable-length output.[\[1\]](https://en.wikipedia.org/wiki/Hash_function#cite_note-1) The values returned by a hash function are called *hash values*, *hash codes*, (*hash/message*) *digests*,[\[2\]](https://en.wikipedia.org/wiki/Hash_function#cite_note-2) or simply *hashes*. The values are usually used to index a fixed-size table called a [*hash table*](https://en.wikipedia.org/wiki/Hash_table). Use of a hash function to index a hash table is called *hashing* or *scatter-storage addressing*."

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/ad621656-d309-48f6-9f10-0be35b2fb3df.png align="center")

We can arrange the hash values and create a hash ring. Now what is a hash ring. When I arrange the hash table values into clockwise ring according to certain order, it creates a ring.  
We will assign a hash value to each of our server. We will fetch the hash value for the request as well. Add moving clockwise the closest server will be assigned the request.

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/15b1875c-cb66-4c15-8261-60f7120af7c2.png align="center")

Now how is this algorithm going to solve all the above mentioned issues:  
What if a new server came. We just need to move anti clockwise from the server to the closest available server then assign them to the new server. Same happens when a server stops working. This algorithm takes an approximate of (K/N) changes where K is the total number of requests or keys and N is the number of server or nodes.  
How can we use consistent hashing be used from overburdening a particular server?

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/8804a196-f7dc-48dd-b7ea-b035cb386312.png align="center")

The solution to the above problem is creating virtual nodes. We will take random hash values and assign to to our real nodes. Any key or request assigned to a virtual node will actually be handled by the real node.

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/1c76daf4-a686-48e5-968a-06e4857fe8c0.png align="center")

Now look at the above setup. All the nodes will get uniform requests or keys.  
This is how we overcame the problem of one server getting overburden while the other is idle.

  
In short:  
Consistent hashing replaces rigid mapping schemes (like simple modulo) with a ring-based hashing approach that maps both servers and keys into the same hash space. This design minimizes remapping when servers join or leave, improving stability and scalability for load balancers, caches, and distributed storage. Practical enhancements—most notably virtual nodes and replication—help smooth load distribution and increase fault tolerance, while careful choice of hash function and monitoring mitigate hotspots and uneven load.

Key takeaways

*   Maps keys and servers to a circular hash space so only nearby keys move when membership changes.
    
*   Virtual nodes improve uniformity; replication improves availability.
    
*   Ideal for dynamic, large-scale systems where minimal data reshuffling and smooth scaling are required.
    

Try implementing a small ring (with virtual nodes and simple replication) to see how node joins/leaves affect key placement—it's the best way to internalize the behavior and trade-offs of consistent hashing.
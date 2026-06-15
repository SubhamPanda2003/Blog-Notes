---
title: "Bloom Filters in Distributed Systems: Speed Through Probability
"
datePublished: 2026-06-15T17:44:18.015Z
cuid: cmqfi615100000bhzckp70ptc
slug: bloom-filters-in-distributed-systems-speed-through-probability

---

Have you ever wondered how Instagram almost immediately hits backs with the “Username already exists!” message when you try to create a new account? Lets make a quick estimate - 

Suppose the number of usernames stored in Instagram is around 5 billion (active+inactive+suspended+bots). In the worst case scenario there will be 5,000,000,000 comparisons to find if a username exists if searched sequentially. Lets assume 1 comparison takes 100 ns (optimistic). So 5 billion comparisons will take around 8.3 minutes! This is highly impractical for for a website that must respond in milliseconds.

This is where bloom filters shine. Bloom filter is a space efficient probabilistic data structure that can give two possible answers - 

1.  Element is **definitely** not present
    
2.  Element is **probably** present
    

Traditional approaches for lookups use hash sets. This requires massive memory if we are to store millions or billions of records. Bloom filters can store compact representations of the values although they are prone to false positives (will say an element exists when it does not). False negatives are impossible (will say an element does not exist when it does).

**How bloom filters work?**

A bloom filter is essentially an array of m bits with k independent hash functions. Lets understand with an example - 

Suppose we want to store the username “dodo”. 

The bit array is initialised to all 0s at first. Assume m = 10 and k = 3

Step1: Compute hash of the username.

Step2: Set the above positions to 1 in the bit array.

Try to insert the username “momo”.

Step1: Compute hash.

Step2: Set the above positions to 1 in the bit array.

Now we query an username - “panda”.

Step1: Compute hash

Step2: Check the corresponding positions in the bit array.

Since position \_ is set to 0, “panda” is definitely not present.

Alternatively if all checked positions are set to 1, then the element is probably present. It might produce false positives in case the bit positions are set by other words and not the username we query. Hence, it is important to choose the values of m and k efficiently to reduce the false positive rate. If the size of the filter is too small or number of elements to be inserted is larger then the rate of false positive will increase rapidly. A properly sized Bloom Filter can achieve false positive rates below 1%.

You cannot delete an element from a standard bloom filter as multiple elements can share the same bit and deleting one will impact other elements as well.

There are many special forms of standard bloom filters to mitigate such problems. 

<table style="min-width: 50px;"><colgroup><col style="min-width: 25px;"><col style="min-width: 25px;"></colgroup><tbody><tr><td colspan="1" rowspan="1"><p><strong>Method</strong></p></td><td colspan="1" rowspan="1"><p><strong>Working</strong></p></td></tr><tr><td colspan="1" rowspan="1"><p>Counting Bloom filters</p></td><td colspan="1" rowspan="1"><p>Store counts rather than bits. Supports safe deletion(increments and decrements of the counts) but requires more storage memory.</p></td></tr><tr><td colspan="1" rowspan="1"><p>Scalable Bloom Filters</p></td><td colspan="1" rowspan="1"><p>Create a larger bloom filter if the existing one fills up. Redistribution of values is impossible of the existing filter so one has to check all filters before performing an operation on the new one.</p></td></tr><tr><td colspan="1" rowspan="1"><p>Cuckoo Filters</p></td><td colspan="1" rowspan="1"><p>Most modern systems use cuckoo filters. It provides lower false positive rates, supports deletion and use comparable memory.</p></td></tr></tbody></table>

Questions worth exploring:

1.  How to mathematically derive the optimal value for m and k?
    
2.  How does cuckoo filters work and why is it preferred by modern systems?
    
3.  How to distributed systems synchronise bloom filters across all nodes?
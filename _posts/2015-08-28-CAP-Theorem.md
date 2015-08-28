---
layout: post
title: "CAP Theorem"
description: "Trying to Understand CAP theorem"
category: Distributed System
tags: [distributed systems, CAP Theorem]
---
{% include JB/setup %}

## CAP Theorem - Logical Analysis

Let us analyse CAP Theorem with a simple example. From the example we shall get into CAP theorem gradually.
Let us say we have two servers one in India and one in US. It is kept in such a distributed
way so that when person can from US access he can get data by querying US server and a person from India can be routed
to Indian server.

[Note: This example assumes the following

1) there is a coordinator which will route the request to the nearest server.]

Initially each has a key value pair stored in it. Say a : 2 be the key value pair.

So now the state is ...
![img1](https://cloud.githubusercontent.com/assets/5524260/9529291/115bfc46-4d17-11e5-9911-21cba6db36a3.png)

Now someone from India accesses this cluster and wants to change a to 3 

![img2](https://cloud.githubusercontent.com/assets/5524260/9529303/1be00c48-4d17-11e5-957f-ede4014c821e.png)

This operation is successfull and the Indian server is updated to have a = 3 ...

The new state is

![img3](https://cloud.githubusercontent.com/assets/5524260/9529415/c0d4c306-4d17-11e5-9286-909b607b2855.png)

Now after this write the normal procedure is to synch data between India and US server.
Meanwhile before this synch could happen someone from US is trying to read data, and as he is in US , the request goes to US server .....

![img4](https://cloud.githubusercontent.com/assets/5524260/9529570/a78adfba-4d18-11e5-9c71-bd1d4abf0f7a.png)

Now we see that the two servers are not in synch so how are we going to handle it .... 


We can say one of the below

***
1) I want the users of my service who are requesting for 'a' to be happy with a value . I should never say 'I am sorry ! I am not avalibale'. When US user requests for 'a', I will try to check if the values of a in India and US are same , if not I will update the older one to latest and give the user the latest.
But what if the checking with Indian server cannot happen due to network disconnectivity. so US server cannot talk to Indian server ?

Now I have taken a decision that even when Indian server is not reachable, I am not ready to give 'Not available' info to user as he is a valuable client, so I will manage with the value in US server itself.

So we have sacrificed <b>C</b>ONSISTENCY for the sake of being <b>A</b>VAILABLE all the time and being tolerant to Network <b>P</b>ARTITION.

Such a system is called <b>AP</b> system .

***
2) I want the users to get the latest value of 'a'. I am not ready to fool the user. This fooling is not fair. I want consistency across the globe. When US user requests for 'a', I will try to check if the values of a in India and US are same , if not I will update the older one to latest and give the user the latest.
But what if the checking with Indian server cannot happen due to network disconnectivity. so US server cannot talk to Indian server ?

Now I have taken a decision that when I am not able to guarantee that I am giving the client requesting for 'a' that I am giving the correct latest value, I will better say 'Not Available' instead of giving a incorrect value .

So we have sacrificed <b>A</b>VAILABLITY for the sake of being <b>C</b>ONSISTENT all the time and being tolerant to Network <b>P</b>ARTITION.

Such a system is called <b>CP</b> system .

***

Ok , what about CA ?

Whenever the nodes are distributed, there is a requirement that nodes need to communicate with each other or some master server. so there is compulsory communication . So when there is communication becomes a must, the chances of it getting disrupted also becomes something that cannot be ignored.

So I am not sure if a pure CA system that is distributed can exist.

Ok this is just the top level analysis.
Let us go further deep to see if CA systems are possible.

Now comes point 3

3) I want the users to get the latest value of 'a'. I am not ready to fool the user. This fooling is not fair. I want consistency across the globe. When US user requests for 'a', I will try to check if the values of a in India and US are same , if not I will update the older one to latest and give the user the latest.But what if the checking with Indian server cannot happen due to network disconnectivity. so US server cannot talk to Indian server ?

This is a problem. Unless I communicate with Indian server I cannot confirm that the value of 'a' is correct. But due to network disconnectivity they cannot communicate. I am unable to think of a solution as there is another condition "I am not ready to give 'Not available' info to user as he is a valuable client".

so I cannot show user "Un Available" info but at the same time I am not able to make the CORRECT/LATEST value available to him .. This is like "cannot show unavailable when the correct/latest value is actually unavailable".
So this state proves that we cannot have a CA system that is distributed.


***
Now having seen examples let us see what is the actual definition of CAP's theorem.

Wikipedia says 


        In theoretical computer science, the CAP theorem, also known as Brewer's theorem, states that it is  
        impossible for a distributed computer system to simultaneously provide all three of the following    guarantees:
        1. Consistency (all nodes see the same data at the same time)
        2. Availability (a guarantee that every request receives a response about whether it succeeded or failed)
        3. Partition tolerance (the system continues to operate despite arbitrary partitioning due to network failures)



So our analysis slightly varies from actual CAP theorem in the CA aspect. CAP theorem seems to say that CA system is possible. But our analysis says CA is meaningless.
But our analysis is supported in [stackoverflow](http://stackoverflow.com/a/12347673/1597944) .
Please get back to me in case there is a better explanation you have to show CA system is possible.

**Personal 'May be' Opinion to support CAP theorem's CA system:**

One thing we can surely say is CA system is possible when there is no network partition.
The problem is that in a distributed system network partition is something unavoidable.
One way which I can think of where CA system is possible(even when there is a network partition) is when we make the write guarantee that it makes write available to all systems in a distributed system. If the system provides that guarantee during WRITE then each node can guarantee that even when there is network failure it can give the correct/latest value.
If this understanding of mine is correct then it leads to some other understanding too : If WRITE is allowed without the guarantee that it will be written to all systems in a cluster, then READ needs to be careful enough to check with other systems in order to guarantee CONSISTENCY. If WRITE guarantees that it will be written to all systems in cluster, then READ can be casual without confirmation from other systems in the cluster. 
If during WRITE some nodes are not reachable then WRITE should be rolled back. But the inner truth is that there should be communication among nodes in at least one phase among READ/WRITE to enable distrubted system to work with CONSISTENCY.


**References :**

1. [http://www.cs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf](http://www.cs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf)
2. [http://markburgess.org/blog_cap.html](http://markburgess.org/blog_cap.html)
3. [https://blogs.oracle.com/MAA/entry/the_cap_theorem_consistency_and](https://blogs.oracle.com/MAA/entry/the_cap_theorem_consistency_and)
4. [http://stackoverflow.com/a/12347673/1597944](http://stackoverflow.com/a/12347673/1597944)
5. [http://codahale.com/you-cant-sacrifice-partition-tolerance/](http://codahale.com/you-cant-sacrifice-partition-tolerance/)

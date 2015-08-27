---
layout: post
title: "CAP Theorem"
description: "Trying to Understand CAP theorem"
category: Distributed System
tags: [distributed systems, CAP Theorem]
---
{% include JB/setup %}

## CAP Theorem

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

1) I want the users of my service who are requesting for 'a' to be happy with a value . I should never say 'I am sorry ! I am not avalibale'. When US user requests for 'a', I will try to check if the values of a in India and US are same , if not I will update the older one to latest and give the user the latest.
But what if the checking with Indian server cannot happen due to network disconnectivity. so US server cannot talk to Indian server ?

Now I have taken a decision that even when Indian server is not reachable, I am not ready to give 'Not available' info to user as he is a valuable client, so I will manage with the value in US server itself.

So we have sacrificed <b>C</b>ONSISTENCY for the sake of being <b>A</b>VAILABLE all the time and being tolerant to Network <b>P</b>ARTITION.

Such a system is called CA system .





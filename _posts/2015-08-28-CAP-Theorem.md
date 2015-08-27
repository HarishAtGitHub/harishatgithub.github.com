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
Before this could happen there is a internet disconnectivity between India and US so they cannot synch data.
(a network partition has happened)
Meanwhile  someone from US is trying to read data, and as he is in US , the request goes to
US server .....

![img4](https://cloud.githubusercontent.com/assets/5524260/9529570/a78adfba-4d18-11e5-9c71-bd1d4abf0f7a.png)

Now we see that the two servers are not in synch so how are we going to handle it .... 








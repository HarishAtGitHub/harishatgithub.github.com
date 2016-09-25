---
layout: post
title: "why the heck is my virtual machine not accessible from my host machine ?"
description: "NAT"
category: utf8
tags: [Network Address Translation, NAT,Networking]
---
{% include JB/setup %}

## why the heck is my virtual machine not accessible from my host machine ?

I have a problem with my virtual machine I am running using virtual box. The problem is that, my vm, despite having its own ip
'10.0.2.15', is not accessible from my host machine. Why ? Why ? Why ?

To test if this is a valid ip, I just tried to access the ip 10.0.2.15 from within my virtual machine. And it worked.

Why the heck am I not able to access it from the host in which the virtual machine is working ?

Then went and saw the network tab in virtual box settings and it looked like the following.

![nat](https://cloud.githubusercontent.com/assets/5524260/18813132/e405dec6-82a1-11e6-8694-f5b8836f02eb.png).

And that was the culprit.

# NAT :

I know a little about NAT. It is a way of mapping one IP address to another. But why is this needed ?
One reason I know is to save ipv4 addresses from exhaustion. This big world with population nearling 7.4 billion people,
with each one having 2 to 3 devices, cannot be accomodated within IPV4 address which is just 32 bits (so 2^32, which is nearly 
4,294,967,296 ~ 4.2 billion). So people thought that instead of each device having a globally accessible network IP, why not
give certain devices, private IP's, which are only accessible within a local network. But when they later feel that their
machine needs to be accessible globally from anywhere(in other words, to make the IP Public), they can claim a public IP from
the pool.

But there is a problem. The private ip devices can call a public IP in internet and ask for details. But after that 
server (public) processing the request, it needs to send it back to the private IP. But unfortunately the private IP 
is not accessible from the network. So how will the server return data ? what can be the solution ?

So why not have a mediator that has public IP, and that gets the requests from the private IP's and sends it to the 
destination public server as if it was the one who is asking for it. Now the server after processing the request, prepares the 
response and sends it this publicly accessible mediator, which hands it over to the private IP.

This is called network address translation




Then comes the question:  **how will the mediator know who requested it ?**

So the logical answer is that the mediator needs to keep track of this. The tracker for this is called the 
Network address translation table. And so from the table it can figure out whom to return packet to.
This mediator is normally the router.

In summary this is what happens :

```
private ip(client)   --->    public ip (mediator) ---->  public ip (destination server)
public ip (destination server) --> public ip (mediator) ---> private ip(client) 

```


# Returning back to the original problem 

So this kind of NAT setup seems to be there in the virtual box too. [It does the address translation to make the local vm's
use the public network](https://www.virtualbox.org/manual/ch06.html). So each vm is a private ip. That is the reason
we were not able to access it from the host system which was outside this private network set up by the virtual box.










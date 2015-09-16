---
layout: post
title: "Concurrency and Parallelism discussion - 2"
description: ""
category: Concurrency and Parallelism
tags: [Operating System, OS, Concurrency, Parallelism]
---
{% include JB/setup %}

### Concurrency and Parallelism - Part 2

Ok having discussed what is process and thread. Lets look at the two ways of multi processing

1) having multiple processes run in parallel

2) having multiple threads run in parallel

In methodology 1 , the processes have different address space ,so no one is going to conflict the other so there are no(strictly speaking less) problems.

In methodology 2 , the threads share the same address space, there are a number of problems that can arise if they don't communicate with each other properly.

Why do we have problems ?

Let us try to understand that problems will arise, with the help of an analogy . Let us consider a bit dirty example so that we get the concept deep into our heart.

There is a single bathroom in a house of 2 people A and B. Person A goes to take bath without informing Person B. so oblivious of the fact that Person A is taking bath, Person B gets inside .... Ahhhhhhhhhhhhhh...... Oooooppppss .... What I meant by Ahhhhhhhhhhhhhh...... Oooooppppss is that something undersirable has happened. In programming environment this can be anything(we will this discuss later).
so this is the problem ** when sharing resources and still there is lack of communication between the sharers**. Had Person A communicated through some mechanism to Person B that he is taking bath , this embarassing incident would not have happened. How to communicate ? There are several ways ... 

1) Put a lock for the door when ever a person is using the Bath room. (LOCK). So when Person B sees the lock he will not get in because he does not have the key to open the door.

2) Make person A literally go to Person B and tell him that he is going to take bath for some time so that Person B will not use that bath room for that time period.(MESSAGE PASSING)

Ok Fine I think this is enough to understand the problems that will rise when we share resources and there is no communication between the entities sharing the resource. We will link the two solutions proposed above in technical terms and see what is the solution in computer science to solve such a problem.


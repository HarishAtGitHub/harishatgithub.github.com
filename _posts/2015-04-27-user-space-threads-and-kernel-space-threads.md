---
layout: post
title: "User space Threads and Kernel space Threads"
description: ""
category: Concurrency and Parallelism - Discussion
tags: [Operating System, OS, Concurrency, Parallelism]
---
{% include JB/setup %}


### User Space & Kernel Space Threads

We have learnt about <a href="http://harishatgithub.github.io/concurrency%20and%20parallelism%20-%20discussion/2015/04/26/concurrency-and-parallelism---disqus/">threads</a> in the previous posts. 

Now to implement multi processing with threads there are two ways

1) real parallelism - Where threads really run parallely , may be each in a different core.

Analogy: This can be compared to having a teacher for each student.

2) Quasi parallelism - This gives a outer feel that each thread is running parallely. But it reality only one thread is active and executing instructions in CPU . 

Analogy: This can be compared to having a teacher manageing a class of say 10 students. So what the teacher does is at an instance she takes care of a single student. But in order to satisfy everyone she gives time slots for each student to get her help. 

The situation above is called time multiplexing . The same thing happens in OS . The OS handles a number of threads by giving time slot for each by a scheduling algorithm. The context switch between threads is so fast that the user will not feel that the threads are run sequentially .




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

2) Pseudo parallelism - This gives a outer feel that each thread is running parallely. But it reality only one thread is active and executing instructions in CPU . So it is actually sequential.

Analogy: This can be compared to having a teacher managing a class of say 10 students. So what the teacher does is at an instance she takes care of a single student. But in order to satisfy everyone she gives time slots for each student to get her help. 

The situation above is called time multiplexing . The same thing happens in OS . The OS handles a number of threads by giving time slot for each by a scheduling algorithm. The context switch between threads is so fast that the user will not feel that the threads are run sequentially .


If the threads are managed by kernel , it will take care of a number of threads and make concurrency(Psuedo parallelism) or even Parallelism(real parallelism) possible depending on the resources available. If resources are available then reall parallelism is even guaranteed. If not it will at least give at least Psuedo parallelism .
These threads in program controlled by kernel are called **kernel space threads**.

But if the threads are managed by just user space program and are not mapped to kernel threads then this means the program will need to have its own scheduling algorithm to switch between threads. The downside is that whatever be the number of threads in the userspace, if it is mapped to only one kernel thread on the whole , then real parallelism is not at all possible. Because as there is only one kernel thread, only one instruction can be executed in the CPU at a time. So this is a case of Pseudo parallelism. These threads are called **user space threads** or **green threads**. 

Examples:

1) In java(1.7 atleast) each green thread is mapped to kernel thread so real parallelism is possible here.

2) Ruby(MRI) 1.8 has only <a href="https://www.igvita.com/2008/11/13/concurrency-is-a-myth-in-ruby/">green threads</a>. But ruby 1.9 has <a href="http://www.csinaction.com/2014/10/10/multithreading-in-the-mri-ruby-interpreter/">green threads mapped to kernel threads</a>. But the problem is that there is GIL(<a href="http://en.wikipedia.org/wiki/Global_Interpreter_Lock"> Global Interpreter Lock</a>).So because of this lock only one thread is possible to execute instructions in the interpreter. So even though the green threads are mapped to kernel threads only one instruction can be executed at a time in CPU . So only psuedo parallelism is possible here. (But this is only for instructions that deal with CPU, if it is IO then real parallelism is possible).

3) The above holds good even for <a href="http://jessenoller.com/2009/02/01/python-threads-and-the-global-interpreter-lock/">python</a> which even has a GIL. So here too psuedo parallelism is only possible. (Doubt ???? Guido Van Rossum has a <a href="https://mail.python.org/pipermail/python-3000/2007-May/007414.html">justification</a>.)


Other interesting links I found

1) http://stackoverflow.com/questions/10225838/what-use-can-i-give-to-ruby-threads-if-they-are-not-really-parallel

2) http://www.jeffknupp.com/blog/2012/03/31/pythons-hardest-problem/

3) http://www.advancedlinuxprogramming.com/alp-folder/alp-ch04-threads.pdf

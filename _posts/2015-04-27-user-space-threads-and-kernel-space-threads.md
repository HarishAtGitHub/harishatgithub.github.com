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

**Personal Stand:(Feel free to override)**

May be the GIL is done so as to have the programming model simple. We know the care we need to take in java to handle multithreading using synchronized blocks or so.Yes thats really difficult in java. So Yes I accept that GIL makes programming simple. Nevertheless, it makes things slower as parallelism is not possible and also due to the check of locks that consumes time. 
Even Javascript in single threaded. For this very reason of simple programming model and the support for asyn IO by default in javascript engines like V8 made <a href="https://www.youtube.com/watch?v=SAc0vQCC6UQ"> Ryan Dahl</a> pursue developing node in javascript. But how is the delegated work done if there is only one thread ????
Node has pool of worker threads running behind (not sure if V8 even does it by having internal threads). <a href="https://www.youtube.com/watch?v=8aGhZQkoFbQ">So once the work is done and if call stack is clear the call backs are executed .</a>. 

**What is the downside of this single threaded model ?**
If you have a CPU intensive operation running, thats it... it will block everything else as there is one and only one thread.

Other interesting links I found

1) http://stackoverflow.com/questions/10225838/what-use-can-i-give-to-ruby-threads-if-they-are-not-really-parallel

2) http://www.jeffknupp.com/blog/2012/03/31/pythons-hardest-problem/

3) http://www.advancedlinuxprogramming.com/alp-folder/alp-ch04-threads.pdf

---
layout: post
title: "Concurrency and Parallelism   discussion - 1"
description: ""
category: Concurrency and Parallelism - Discussion
tags: [Operating System, OS, Concurrency, Parallelism]
---
{% include JB/setup %}

## Concurrency and Parallelism - Part 1
When we speak about Concurrency and Parallelism, the first thing that immediately comes to mind is
'Thread' and 'Processes'.
Let us understand this first and then dive deep into understanding Concurrency and Parallelism.

### Process
**Process is just an instance of an executing program(or in other words abstraction of a running program), including current values of the program counter, registers, and variables.** - (Andrew. S. Tanenbaum - Author of the book 'Modern Operating Systems')

**But How to verify the above definition ?**

Let us think logically with respect to unix OS (I am using ubuntu).
Execute command pstree (a commands to list all the running processes in os)
$pstree 

init --------entry1 ----

        |
        
        -----entry2 ----
        
        |
        
        -----gnome-terminal 
and so on .
Now open a terminal  and run a java program(with a sleep for 50 seconds so that we can sustain the process)(say)

now run
$pstree

init --------entry1 ----

        |
        
        -----entry2 ----
        
        |
        
        -----gnome-terminal --- java ...
        
        
so it means that program that you ran is represented as a process java . 

So this means that any program executed is represented as a process in the operating system.So this verifies the definition given by Tanenbaum .

Now lets get little deeper .

**How is any process created ?**

Clue : One thing to note from the above experiment is the java was created as a child of gnome-terminal.

Every process is created a child of some existing process (here gnome-terminal). Whenever a program is executed
a system call called **fork** is made to kernel and what it does is it clones(copy of memory image, environment strings, file descriptors) the process that creates this new process . Then this new child process executes the execve or similar system call to change its memory image and run a new program.
Now finally the child and parent process both have different memory address space so that they can act independetly.
**So fork creates a new child process with a new address space different from the address space of the parent that creates it.**



### Thread
So now what if we want to do multi processing. If our computer had one core then it makes sense to use have one process alone. If we have two cores then it makes sense to run another process in parallel to do the tasks faster.This is called multi processing. So the same program can me relaunched again as a separate process and made to run in the other core. So now each process has its own address space. Ya, so far its good . Because we were able to use both the cores and do more work. But in many scenarios 

1) there is no need for new process to have a new address space. It can share it with the other. It is just waste of memory .

2) there is  compulsion that the two processes need to share address space , if the 2 processes needs to co-operate and work.

This scenarios give rise to the need for facility for processes to share the same address space.

This gives birth to concept of threads.

Note: Any process (in strict sense is said to be single threaded or in other words it has a single thread of execution) has its own address space. When it starts sharing the address space with another it is no longer called a process but instead called a thread.This is just a way of calling it . You are free to call it simply as process that don't share address space and processes that share address space.

As threads share address space with that of another , they are called light weight processes , mainly because of less overhead involved. 

Read more about Kernel Threads and User space threads <a href="http://harishatgithub.github.io/concurrency%20and%20parallelism%20-%20discussion/2015/04/27/user-space-threads-and-kernel-space-threads/">here</a>.

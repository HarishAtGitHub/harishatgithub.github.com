---
layout: post
title: "Concurrency and Parallelism   disqus"
description: ""
category: Concurrency and Parallelism - Discussion
tags: [Operating System, OS, Concurrency, Parallelism]
---
{% include JB/setup %}

## Concurrency and Parallelism - Part 1
When we speak about Concurrency and Parallelism, the first thing that immediately comes to mind is
'Thread' and 'Processes'.
Let us understand this first and then dive deep into understanding Concurrency and Parallelism.

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
Now open a terminal  and run a java program(say)

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

**How is the process created ?**

Clue : One thing to note from the above experiment is the java was created as a child of gnome-terminal.

Every process is created a child of some existing process (here gnome-terminal). Whenever a program is executed
a system call called **fork** is made to kernel and what it does is it clones(copy of memory image, environment strings, file descriptors) the process that creates this new process . Then this new child process executes the execve or similar system call to change its memory image and run a new program.
Now finally the child and parent process both have different memory address space so that they can act independetly.
**So fork creates a new child process with a new address space different from the address space of the parent that creates it.**


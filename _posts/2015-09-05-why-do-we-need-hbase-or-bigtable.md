---
layout: post
title: "Need for HBase"
description: "Trying to Understand the need for HBase"
category: Distributed System
tags: [distributed systems, HBase]
---
{% include JB/setup %}

## Why do we need HBase if we already have Hadoop and HDFS ?

Hadoop distributed File system (HDFS) (modelled after GFS) is a distributed file system which is designed to
handle huge amounts of data. The querying or in other words processing is done with the help of a framework
known as Hadoop Map Reduce framework. So now we have a large scale distributed storage and large scale
processing mechanism to make sense of this huge data.

The above explanation of HDFS is true and correct . I have used HDFS and Mapreduce for processing the huge chunks of github data accumulated in 4 years 
[here](https://github.com/HarishAtGitHub/hadoop-mapreduce-githubdata). The recorded results are also found 
[here](https://github.com/HarishAtGitHub/hadoop-mapreduce-githubdata/blob/master/performance/Hadoop%20job_201408222343_0001%20on%20master.pdf).
. It was really good . With the proper usage of Mapreduce I was able to process data very fast. 
I used nearly 300 GB of data . What Hadoop does is when I put the 300 GB of data in the masters data folder it just
split them into 64 MB chunks and put it in all the three nodes which acted as Data nodes. so now all went fine when
I ran mapreduce. But I did not have answer for the question of how hadoop knows which block is
in which node. Then when reading books on hadoop I found that it is the [NameNode](http://wiki.apache.org/hadoop/NameNode)
which keeps track of the location of files . Ahh... then comes the question if Name node keeps track of the 
files location, what if there are many many files ? what if I had so many 10 KB files?  the problem is that though
the minimum block size is 64 MB in hadoop it cannot concatenate files and have it in one block. so even
10 KB files will occupy a block . so the problem is that keeping track of these many files will exhaust the
memory of Name Node . So this is the problem.  Always we will not have files of such large sizes . There
are many cases where there will be many tiny files of data . So for such systems HDFS is unfit .

And also because it is a system for large files , there is another problem when it comes to random reads of 
specific data as the HDFS only knows where the 64 MB block is and it does not know anything about the data in
 it . So granularity is lost when it comes to randon read of specific data. Not just random random reads, but random reads with minimum number of disk seeks is the problem.
 
These are the areas where HDFS and Mapreduce will not fit.

To solve these problems , Google came up with the solution of [BigTable](http://static.googleusercontent.com/media/research.google.com/en//archive/bigtable-osdi06.pdf).
Then the opensource world came up with HBase which is modelled after BigTable.


**Referrences**:

1. [My own experimentation with hadoop](https://github.com/HarishAtGitHub/hadoop-mapreduce-githubdata)
2. [Hadoop Wiki](http://wiki.apache.org/hadoop/NameNode)
3. [HBase - The Definitive Guide](http://www.gocit.vn/files/Oreilly.HBase.The.Definitive.Guide-www.gocit.vn.pdf)
4. [Big Table research paper published by Google](http://static.googleusercontent.com/media/research.google.com/en//archive/bigtable-osdi06.pdf)

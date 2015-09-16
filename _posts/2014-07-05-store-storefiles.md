---
layout: post
title: "Store and Store Files in HBase"
description: "Store and Store Files in HBase"
category: hbase
tags: [distributed systems, hbase]
---
{% include JB/setup %}

## Store and Store Files in HBase

A  table in HBase is seen by hbase as a combination of **Regions**. Regions are nothing but segments of the table 
based on contiguous row keys .It is the basic unit of horizontal scalability in hbase. 
This can also be thought of as shards.

![hbase-physicalview](https://cloud.githubusercontent.com/assets/5524260/9876535/6e80b1e8-5bd4-11e5-8b49-4f4a29400301.jpg)

HBase by the phenomenon called as autsharding it takes these regions and distributes it among its nodes called 
**Region Server**.

![region server and regions 2](https://cloud.githubusercontent.com/assets/5524260/9878258/5ebdd690-5bdf-11e5-86dc-cf8342ffeda4.jpg)

So now the Regions(or shards) have reached the Region Servers.

Now when the users query the data must be in appropriate form so as to have faster key based navigation possible
in these regions.

Let us take a region (e-h) for example and see how it is served.

**point 1 :**

Firstly, HBase is a NoSQL database or in simple words an aggregate oriented data base. It is done with the idea to reduce
joins as much a possible. So each value corresponding to a key is an aggregate (or in RDBMS terms the aggregate
obtained after 'joins'). Ok now one problem of aggregation(joins) is solved. HBase is not just blind to the structure
of this value corresponding to key. It wants to get one level deep. This is with the intent that users if 
they ask for certain part inside that aggregate hbase should be able to serve it faster. And there comes 
the natural question, "if the aggregate has 3 parts in it and if user just asks for part 2 , why should other parts part 1 and part 3 be even loaded ?" . The answer came in the form of what is called as **column family**. This is way of logically grouping columns, so as to provide that level of granularity in the value corresponing to a  key.

<pre>
  <code class="javascript">

rowkey-e:  {
                column-family-1: {
                                  column1: ....
                                  column2: ....
                                  column3: ....
                                 },
                column-family-2: {
                                  column4: ....
                                  column5: ....
                                 }
            }
            
  </code>
</pre>

In short if the user asks for column-family 1(i.e., group of columns 1,2,3, why should column family (i.e., columns2
4 and 5) be even loaded. So what hbase did is , it took the regions and it took the columns families in
it and started viewing it as the fundamental unit of data inside regions.

**point 2 :**

Secondly, each region is of huge size . So everything cannot be served from memory. We need to use some RAM space for some and rest need to stay in disk(this design should enable faster and efficient searching). 
Based on the key requested the disk files can be loaded and then searched for the key.

Based on the above two points the following is known

              column-family = in-memory-block + on-disk-blocks
              

this column-family (the basic unit of data in region) is at many places in hbase doc is also referred to 
as **Store**.

So hereafter we will also refer to it as Store. So let us modify the above formula(like).

              Store = in-memory-block + on-disk-blocks
              
For each Store there is only one in-memory block and when it becomes full or crosses threshold size it is flushed
to disk to form on-disk-blocks.

in-memory-block : is also called as MemStore
on-disk-blocks : is also called StoreFiles

so

               Store = 1 MemStore + ( Zero or more StoreFiles )
               

So now the region is viewed by HBase as following

![hbase-internal](https://cloud.githubusercontent.com/assets/5524260/9876582/cdf8c05c-5bd4-11e5-9bd5-b28ce50b16d5.jpg)

![equationstore-storefiles](https://cloud.githubusercontent.com/assets/5524260/9877998/e9e106f4-5bdd-11e5-90e8-85632cc85445.jpg)


So overall view is as follows 

![detailed-region-regionserver](https://cloud.githubusercontent.com/assets/5524260/9878290/8c08f44a-5bdf-11e5-86fd-186bd896b351.jpg)


**Code Referrence:**

1. Store (Java doc: Interface for objects that hold a column family in a Region. Its a memstore and a set of zero or more StoreFiles, which stretch backwards over time.)

   Location : [Store.java](https://github.com/apache/hbase/blob/1.0/hbase-server/src/main/java/org/apache/hadoop/hbase/regionserver/Store.java)

   Type : Interface 
   
   Package : org.apache.hadoop.hbase.regionserver
   
   Module : hbase-server
   
   Implementations : [HStore.java](https://github.com/apache/hbase/blob/1.0/hbase-server/src/main/java/org/apache/hadoop/hbase/regionserver/HStore.java)
   
2. MemStore (Java doc: The MemStore holds in-memory modifications to the Store. The MemStore functions should not be called in parallel. Callers should hold write and read locks. This is done in {@link HStore}.

   Location : [MemStore.java](https://github.com/apache/hbase/blob/1.0/hbase-server/src/main/java/org/apache/hadoop/hbase/regionserver/MemStore.java)
   
   Type: Interface
   
   Package : org.apache.hadoop.hbase.regionserver
   
   Module : hbase-server
   
   Implementations : [DefaultMemStore.java](https://github.com/apache/hbase/blob/1.0/hbase-server/src/main/java/org/apache/hadoop/hbase/regionserver/DefaultMemStore.java)
   
3. StoreFile (Java doc: A Store data file.  Stores usually have one or more of these files.  They are produced by flushing the memstore to disk.  To create, instantiate a writer using {@link StoreFile.WriterBuilder} and append data. Be sure to add any metadata before calling close on the
Writer (Use the appendMetadata convenience methods). On close, a StoreFile is sitting in the Filesystem.  To refer to it, create a StoreFile instance passing filesystem and path.  To read, call createReader(). StoreFiles may also reference store files in another Store.
The reason for this weird pattern where you use a different instance for the writer and a reader is that we write once but read a lot more.)

   Location : [StoreFile.java](https://github.com/apache/hbase/blob/1.0/hbase-server/src/main/java/org/apache/hadoop/hbase/regionserver/StoreFile.java)
   
   Type : Class
   
   Package : org.apache.hadoop.hbase.regionserver
   
   Module : hbase-server
   
   Note : StoreFile is a facade over [HFile](https://github.com/apache/hbase/blob/1.0/hbase-server/src/main/java/org/apache/hadoop/hbase/io/hfile/HFile.java)
   
   
   
**Subject Referrence :**

1. [http://hbase.apache.org/book.html](http://hbase.apache.org/book.html)
2. [HBase- The Definitive Guide- O'Reilly](http://www.gocit.vn/files/Oreilly.HBase.The.Definitive.Guide-www.gocit.vn.pdf)
3. [Google Big Table Research Paper](http://static.googleusercontent.com/media/research.google.com/en//archive/bigtable-osdi06.pdf)
3. [mapr.com](https://www.mapr.com/blog/in-depth-look-hbase-architecture#.VfgiNCe1Gkq)
4. [Cloudera](http://blog.cloudera.com/blog/2012/06/hbase-io-hfile-input-output/)
5. [Issue - HBASE-11316](https://issues.apache.org/jira/browse/HBASE-11316)
6. [Sema Text](http://blog.sematext.com/2012/07/16/hbase-memstore-what-you-should-know/)

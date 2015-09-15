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

![region server and regions](https://cloud.githubusercontent.com/assets/5524260/9876028/8bf8c6e2-5bd0-11e5-94c2-2d61257c2aec.jpg)

So now the Regions(or shards) have reached the Region Servers.

Now when the users query the data must be in appropriate form so as to have faster key based navigation possible
in these regions.

Let us take a region (e-h) for example and see how it is served.

**point 1 :**

Firstly, HBase is a NoSQL database or in simple words an aggregate oriented data base. It is done with the idea to reduce
joins as much a possible. So each value corresponding to a key is an aggregate (or in RDBMS terms the aggregate
obtained after 'joins'). Ok now one problem of aggregation(joins) is solved. HBase is not just blind to the structure
of this value corresponding to key. It wants to get one level deep. This is with the intent that users if 
they ask for certain part inside that aggregate hbase should be able to serve it faster. Another there comes 
the natural question if the aggregate has 3 parts in it and if user just asks for part 2 , why should other parts 
part 1 and part 3 be even loaded ? . The answer came in the form of what is called as **column family**. This 
is way of logically grouping columns, so as to provide that level of granularity in the value corresponing to 
a  key.

```javascript

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
```

In short if the user asks for column-family 1(i.e., group of columns 1,2,3, why should columns family(i.e., columns
4 and 5) be even loaded. So what hbase did is , it took the regions and it took the columns families in
it and started viewing it as the fundamental unit of data inside regions.

**point 2 :**

Secondly, each region is of huge size . So everything cannot be served from memory. We need to use some RAM space and rest
need to stay in disk(this design should enable faster and efficient searching). 
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




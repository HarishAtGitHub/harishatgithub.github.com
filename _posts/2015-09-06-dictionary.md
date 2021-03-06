---
layout: post
title: "Dictionary Evolution : Space - Time Trade-Off"
description: "Story behind Dictionary data type"
category: Data Structures
tags: [Data Structures, dictionary, datatype]
---
{% include JB/setup %}

## Dictionary

**Dictionary** is a data type that models a way to store elements in an efficient way so that key based retrieval can be fast. In essence this solves the problem of **SEARCHING**, as what we are trying to do is search for a key and then retrieve the value associated with it.In other
words dictionary provides efficient way to **search a particular key**(and hence to obtain the value in it).

Dictionary can be realized by using

1. Array
2. Linked List
3. Hash Table
4. Binary Trees
5. Red/Black Trees
6. AVL Trees
7. B Trees

**Array Based Implementation:**

There are a number of cases which will give different running times based on the nature of the elements

**case 1: Ordered**

***

Here we can use binary search as the elements in the array is ordered. So searching an element will take O(log(n)).
But insertion and removal will take O(n) as after searching for the location the rest of the elements will have to be shifted.

**case 2: Unordered - (assumes infinite time)**

***

But most of the time elements will not be ordered. So let us talk about a more practical case where elements are unordered.
Here there is no other go but to iterate all the elements. So searching an element will take O(n).
So what if the number of elements is huge . This approach will not just work out as it assumes **infinite time** available for calculation which is practically impossible.
Insertion will take  O(1) as the elements are unordered and no one care where we put the element.
But removal will take O(n) as it involves searching .
So in this practical case, the searching takes worst case running time of O(n) which is bad.

**case 3: Make the key the index of array - (assumes infinite space)**

***

In both the above cases the value we were searching for were values in the array.But the index was just the position
place holder. We know that an array in order to do array[index] it just takes O(1) . So why not use this 
and make the index of the array the key we are searching for ? and value be stored in it as value.

But this has the following problems

1. the key can only be numbers
2. even if we find a way to make any key to a number , there is yet another problem . What if the key is too large and 
sparse. This will lead to a lot of empty spaces and a huge array.

But the idea in case 3 seems good. It is possible to map any key to a number(if you say that the number has integer's maximum limit as the structure is an array, then the other way to do is just map key to memory address and store it in that memory location). So point 1 will not be an issue.
The main issue is point 2 , huge wasted space. This approach assumes **infinite space** which is practically impossible. Had infinite space been available then we would retrieve keys definitely in O(1). 



**Space - Time Trade Off**

***

So now we are caught in a dead lock. case 2 assumes infinite time and case 3 assumes infinite space .
So we cannot achieve either of them despite both guarantee key based retrieval because of the inherent assumptions of infinite time and space. It seems like the following relation will hold good here 

$$ space \varpropto \frac{1}{time} $$

$$ space \times time = k $$

k is now considered as a constant which will mean the factor to denote that the key is rightly identified.It is assumed as a constant as always we want the key to be identified correctly.

so if space is infinite, time taken to find key is zero , and if time is infinite, space consumed is the least .

So the natural solution to this deadlock is got by seeing the above inverse proportionality relation closely. The idea is the trade off between time and space.
Let us see how it solves the problem.

What if instead of giving infinite space in case 2, we try to give a restricted space in a clever way to an extent to which still we don't need infinite time to make key based retrieval.

What if we have array of size equal to number of elements we have but, make the elements(keys) be mapped to
available indexes. so here index is not mapped to keys as in the previous case, but keys are mapped to indexes.
So if someone has key value pairs as follows

williams: "section A",

james: "section B"

williams will be mapped to index 0 and james will be mapped to index 1.
and index 0 of array will hold the value "section A" 
and index 1 of array will hold the value "section B".

So now if someone asks for in which section does james belong to ...
I will pass "james" to our mapping logic and get the output index as 1 and then do array[1].

This function used to do the key to index mapping is called hash function. This function does HASHING .

So with this implementation we just need to instantiate and array equal to number of elements possible.
When inserting an element just hash the key and store it in the index of the array.
When searching for an element just hash the key and just search for that index value.

So this is essentially storing the value associated with the key at the index = hashfunction(key).
With this approach the running time for search, insertion and deletion in a dictionary is O(1).
Such a design is called HashTable.

Did you notice one thing ? Even after reducing the space from an infinite one to a restricted one, we were still able to achieve retrieval time of O(1) . It seems like we have got everything perfectly even after compromise on the ideal assumption. This is impossible without having lost something else. This is like the "Conservation Law" in physics. So by trying to achieve O(1) despite space restriction what have we lost ? The following shows the answer.

**What happens when two keys are hashed to same index in array ?**

This is called **Collision**. This is the implication of us moving away from assumption of infinite space. Had there been infinite space we would not have had collisions.  So because of collision it would increase time for finding a key from O(1), when many keys are at the collided location. So we have lost a little time because of finite space restriction. This fact makes us understand the "Space - Time tradeoff" better.

Ok, We know that this is something that we cannot avoid in finite space systems, so let us think about how we can resolve it .

One way is whenever collision occurs instead of replacing the element in the array why not append to it the new value. So this need the value of an array to be an easily extendable list. May be  a linked list. so values in array are now linked list and when collision occurs the new value is chained to the existing linked list. This way of resolving this problem of collision is called **Chaining**.
But there is a  problem in this approach ... If in worst case all our keys get mapped to same index then the searching time
will again be of order O(n).This is a problem that we need to solve.


So now had we designed the hash function in such a way that there was no collision it would have been ideal. It is now 
in the hands of the hash function to resolve the problem that emerged out of collision.

An ideal hash function will distribute the keys uniformly among the slots in the Hash Table without collision.
If a hashtable has m slots, and there are n elements to be distributed in this hash table, the load factor(denoted by \\( \alpha )\\)
is 

<div>
    $$ \alpha = \frac{n}{m} $$
</div>

**Running Time :**

***

So if n > m, and the hash function does the best job of distributing the n elements in m slots, then
the worst case running time to search for an element is
= time to hash  + worst case running time to search the element in the linked list
= O(1 + \\( \alpha )\\)

The average running time is when the element is found half way through the linked list.
so average running time = O( 1 + \\(\frac{\alpha}{2}\\)) 


for a hash function for a hast table \\(\alpha\\)   < = 1 is the ideal case.
so in this ideal case search will take time of order O(1) .


**Referrences:**

1. [Algorithms in Java - Robert Sedgewick](http://www.amazon.in/Algorithms-Java-Parts-1-4-Edition-ebook/dp/B003KTMWDQ)
2. [Time - Space tradeoff](https://en.wikipedia.org/wiki/Space%E2%80%93time_tradeoff)
3. Nptel IIT lectures
4. [Wikipedia -article on HashTable](https://en.wikipedia.org/wiki/Hash_table)
5. [Wikipedia -article on Hash Function](https://en.wikipedia.org/wiki/Hash_function)

---
layout: post
title: "Hashing and Collision resolution"
description: "Hashing and Collision resolution"
category: Data Structures
tags: [Data Structures]
---
{% include JB/setup %}

## Hash Function and Collision resolution:

What is a Hash function ?
   Wikipedia : A hash function is any function that can be used to map digital data of arbitrary size to digital data of fixed size.
With respect to a hash table , it is one that helps mapping a key to a index in hashtable that is within a finite range [0, N-1].

What is a good Hash function ?
   It is one that distributes keys uniformly throughout the table. It should be designed in such a way that probability
that 2 keys will end up in the same slot in the hashtable is very low.
 
We know that the idea behind hash function is to take your key and then map it to the index of the table. So one way
would be to just take (key mod N) .  But we should note that this will work only when key is an integer.So our first goal is to convert
any key to an integer using some process. Then map that integer to an index.
So to solve the above problem Hash Function in general is said to have two steps

1. Hash Code function -  keys -> integers (hash code) h1 (this integer can be in arbitrary range)
2. Hash Compression Function - integers(hash code) h1 -> integer in range [0, N-1] (hash value) h2


         key ----> Hash Code function ---->  integer in arbitrary range 

         o/p1 ---> Hash Compression function ----> integer in range [0, N-1] 

         where N is the size of the Hash Table.


Hash Code function is not needed when the key is already an integer.

Let us look at some Hash Code functions.

**Hash Code Functions**

***

**1. Integer Cast:**
   Interpret the keys as integer or
   Interpret the memory address as integer.

But this is a problem when the cast results in number greater than 32 bits because
Integer spans to a maximum 4bytes (or 32 bits).

**2. Component Sum:**

   This is done to solve the case where it spans to length greater than 32 bits.
Here if it is greater than 32 bits, we  split it into components of 32 bits and add them.

Component Sum will not work well for strings like "dub" and "bud" where both lead to same integer value.
So this results in collision.
   

**3. Polynomial Accumulation:**

   So what is the way to solve the problem seen in Component sum ? Make the d in 3rd place different from d in first place.
The solution stems from thinking about how our decimal number system(or any) works. 

How 121 differs from 211 ?

121 = 1 * 10^2 + 2 * 10^1 + 1 * 10^0

211 = 2 * 10^2 + 1 * 10^1 + 1 * 10^0

the weightage based on the position of the digits has solved the problem differentiation.
Similarly here give weightage based on the position of the alphabets.

So this is good for now.

**Others:**

Besides these there are also other ways to do like if the key is within range s to t. First convert the key to a number
less than 1 by doing (key - s)/(t-s) , then multiply it by N(size of hash table) so that it gets into range [0, N-1].
Few Others are : [Multiplication method](http://brpreiss.com/books/opus4/html/page213.html#SECTION009230000000000000000),
[Fibonacci Hashing](http://brpreiss.com/books/opus4/html/page214.html#SECTION009240000000000000000)


**Hash compression functions:**

***

Let's say the result of the Hash code function be k.

Now our idea is to map k to integer within range [0, N-1].

one way is as follows

h(k) = k mod m

there is a problem when m is a power of 2.

let us illustrate the problem with a logical analysis.

if k is a binary represented as k    (k>m)

k = abcd

a,b,c,d is 0 0r 1

k in decimal is  a*2^3 + b*2^2 + c*2^1 + d*2^0

if m is a power of 2 < k then m should be either one of 2^2 or 2^1 or 2^0 (reason is because  2^4 < 2^3 + number less than 2^3)

so if m is 2^e, and we do k mod m, we get bits from position e to 0.
so when m is power of 2 say 2^e, we get e least significant bits of k.

so the slot address will be the e least significant bits of k.If it is like this then the probability of collision is more.
It will be best if we have m in such a way that the modulo could result in something depending on all the bits of k
instead of just a portion of k.
It will be a good choice when m is prime number to have less collision.
May be even a power of 2 might help had our case fallen into the case discussed in "others" as the integer was obtained
not by a straight forward method.

so proper choosing of size of hash table will also result in minimum collision hash compression map.

Collision resolution in Hash Table:

1. Chaining
2. Linear probing
3. Double Hashing


**Chaining :**
   If there is collision at a slot then just add the new key too to the same slot which already has a key. This is done by having
the linked list in each slot of the array. Just add the new collided key to the linked list.
here the number of elements to be inserted into hash table can even be greater than the number of slots in hash table as linked
list can store any number of element in itself.

**Linear Probing:**
   Instead of sticking to the collided key as in chaining and then adding it to the pointed linked list at the locations in array,
if the collision occurs find another empty slot.
What happens here is k mod m is found out, then that index is found, if the location is occupied then the next consecutive location is searched for empty state. If not empty it goes till it finds a slot that is having null(empty) and then stores it there.

During search too if key k is searched (k mod m) is calculated, then we navigate to the resulting index and check whether k is there in it,if not it goes to next consecutive location and checks if k is there and this search goes on till a state where

1. either null is found - it means that the element is not found
2. key k is found - so the element is found.

During delet we will do the search first and then delete it. delete key cannot be done by setting it to null, because it will break the
search chain that is after this element. So instead of putting null, place a marker there(called as tombstone) and 
if future search encounters a tombstone then it is ingnored and proceeds through the chain with the search.
In case of insert, the tombstone can be interpreted as null and so insert can happen there in that location.
 
This approach brings in the compulsion that number of elements to be inserted in hash table must be less than or equal to the size of hash table as only one element can be stored in a slot.
This approach might make search slow when the number of hops is high.


**Double hashing:**
    Instead of having a hop distance of always 1, why don't we have a hop distance computed depending on the key(the other way can be have a 
key -> offset map, but that involves storage space so considering this limitation dynamic computation is better), so that 
for every key we need not hop by 1 location. This can reduce the number of hops.
    So the idea is whenever there is a collision the offset is computed and the index + offset location is approached.If it is not
empty then again it hops by the offset and this goes on till an empty location is found.

h(k) = k mod m

offset(k) = function(k)

offset function helps finding offset for a key.

**Referrences**:

1. [Brown University Lectures](http://cs.brown.edu/courses/cs016/docs/old_lectures/HashTables.pdf)
2. [Wiki](https://en.wikipedia.org/wiki/Hash_function)
3. Nptel IIT lectures
4. [brpreiss.com](http://brpreiss.com/books/opus4/html/page210.html)

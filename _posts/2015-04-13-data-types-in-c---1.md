---
layout: post
title: "Facts about range of data types in C - 1"
description: "facts about data types in C"
category: Interesting learnings about data types
tags: [data types, c, interesting c facts and learnings]
---
{% include JB/setup %}

## Range of Data types

### Who decides the range of data types ?

There are two possibilities 

1. Compiler

2. Hardware

Answer would be both .

#### How is it the **compiler** ?
This is based on visible facts .
The fact is that everything is explicitly mentioned in /usr/include/limits.h and
C obeys it .
This file has

```c
#include <bits/wordsize.h>
.
.
.
/* These assume 8-bit `char's, 16-bit `short int's,
   and 32-bit `int's and `long int's.  */

/* Number of bits in a `char'.	*/
#  define CHAR_BIT	8

/* Minimum and maximum values a `signed char' can hold.  */
#  define SCHAR_MIN	(-128)
#  define SCHAR_MAX	127

/* Maximum value an `unsigned char' can hold.  (Minimum is 0.)  */
#  define UCHAR_MAX	255

/* Minimum and maximum values a `char' can hold.  */
#  ifdef __CHAR_UNSIGNED__
#   define CHAR_MIN	0
#   define CHAR_MAX	UCHAR_MAX
#  else
#   define CHAR_MIN	SCHAR_MIN
#   define CHAR_MAX	SCHAR_MAX
#  endif

/* Minimum and maximum values a `signed short int' can hold.  */
#  define SHRT_MIN	(-32768)
#  define SHRT_MAX	32767

/* Maximum value an `unsigned short int' can hold.  (Minimum is 0.)  */
#  define USHRT_MAX	65535

/* Minimum and maximum values a `signed int' can hold.  */
#  define INT_MIN	(-INT_MAX - 1)
#  define INT_MAX	2147483647

/* Maximum value an `unsigned int' can hold.  (Minimum is 0.)  */
#  define UINT_MAX	4294967295U

/* Minimum and maximum values a `signed long int' can hold.  */
#  if __WORDSIZE == 64
#   define LONG_MAX	9223372036854775807L
#  else
#   define LONG_MAX	2147483647L
#  endif
#  define LONG_MIN	(-LONG_MAX - 1L)

/* Maximum value an `unsigned long int' can hold.  (Minimum is 0.)  */
#  if __WORDSIZE == 64
#   define ULONG_MAX	18446744073709551615UL
#  else
#   define ULONG_MAX	4294967295UL
#  endif
#  ifdef __USE_ISOC99
/* Minimum and maximum values a `signed long long int' can hold.  */
#   define LLONG_MAX	9223372036854775807LL
#   define LLONG_MIN	(-LLONG_MAX - 1LL)
/* Maximum value an `unsigned long long int' can hold.  (Minimum is 0.)  */
#   define ULLONG_MAX	18446744073709551615ULL
#  endif /* ISO C99 */
```

So its from here that the information on range in hardcoded .

#### How is it the **hardware** ?
Here you can even see that the size or range varies depending on the hardware machine - word size .
The snippet goes as follows
<pre>
  <code class="c">
#  if __WORDSIZE == 64
#   define ULONG_MAX	18446744073709551615UL
#  else
#   define ULONG_MAX	4294967295UL
#  endif
</pre>

so in 64 bit machines the unsigned long has maximum value of 18446744073709551615 and in others it is 4294967295 .

So this proves that both hardware and software were involved in the design of the range of data types.

#### But what actually influenced the decision of the range ?

I did not find facts anywhere related to this question , except a few surmisals (links can be found in reference section) .
May be it was dependent on the word size .

        By the way what is machine-word size ?
        It is the fundamental unit of data for a processor . For example if it is an n-bit machine , we can infer to an extent that the internal bus used is n-bit size and the basic registers are n-bit .
        

You can see some correlation here in the following (mostly this is common, but there might be exceptions)

in **16 bit(2 byte)** machines 
short int range(2 bytes) = **int range(2 bytes)** < long int range (4 bytes)

in **32 bit(4 byte)** machines
short int range(2 bytes) < **int range(4 bytes)** = long int(4 bytes)

in **64 bit(8 byte)** machines
short int(2 bytes) < **int(4 bytes)** < long int

mostly the commonly used int chimes well with the word size . Except that in **64 bit machines** it is lesser than the word-size .

But Wait !.....
**Why should the value of range for int being lesser than or equal to the word-size and advantage ?**
Yes it is ... It is like this ... If I ask you to draw water from tank A holding 1 liter if the same and pour it in another tank B and you are provided with a mug of capacity 1 litre . So now in one trip you can make this job done.

Now what if your mug was 1/2 liter capacity only . so now you have to get the job done in 2 trips . So finally you got the job done, but with little more pain . So the same analogy holds good even in processors where you provide data which is greater than the word-size of the processor . It can get the job done , but with a pain and decrease in efficiency .

If you give a 8bit-processor that has only 8 bit registers to add two 16 bit numbers , then anyway the processor can add but it has to be done in a different way . 
fist add the first 8 bits of both then store result(resultA) along with the carry if any . Then with the carry add the second 8 bits of both the numbers. this will yield resultB
now the combination of resultA and resultB is the final result .

Was it given to add two 8 bit numbers. only one operation would have been needed .

So if the data size is > than the word-size then it is always a problem .

So may be they have set the int size to match word-size or in best case be less than word-size.

So in a way the word-size would have influenced the decision on the range of the data type.


**References :**

1. http://www.amazon.com/Programming-Modern-Approach-2nd-Edition/dp/0393979504
2. http://stackoverflow.com/questions/29592898/do-long-long-and-long-have-same-range-in-c-in-64-bit-machine
3. http://stackoverflow.com/questions/13403880/how-can-a-16bit-processor-have-4-byte-sized-long-int
4. http://www.bcastell.com/tech-articles/how-does-word-length-affect-the-performance-and-operation-of-a-cpu/
5. http://en.wikipedia.org/wiki/Word_%28computer_architecture%29
6. http://superuser.com/questions/698312/if-32-bit-machines-can-only-handle-numbers-up-to-232-why-can-i-write-100000000
7. http://electronics.stackexchange.com/questions/106933/how-different-are-8-bit-microcontrollers-from-32-bit-microcontrollers-when-it-co



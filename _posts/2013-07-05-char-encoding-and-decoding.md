---
layout: post
title: "Character encoding and decoding"
description: "Character encoding and decoding"
category: utf8
tags: [character encoding, character decoding, utf-8 encoding and decoding]
---
{% include JB/setup %}

## Character encoding and decoding


We know that every things is 1's and 0's in the storage. But then comes the question : 
how any is string represented as binary ?

Each character is converted to its hex code **as per the charset** and then to its equivalent binary and then stored.
this storage may span one or two or more bytes based on the charset chosen.
for eg. in UTF-8 charset (here after we shall base all the explanation based on UTF-8 charset)

the character A will occupy one byte

    String a = “A”

    a.getBytes().length is 1

    byte array is [65]


    String a = "ë"

    a.getBytes().length is 2

    byte array is [-61, -85]


so the first character "A" took one byte.

but the second character "ë" took two bytes.

The System. propety file.encoding(in java) will take care of how it is stored.


**When some characters are 1 byte and some are two bytes and some are more bytes, how will the decoding happen ?
What I am saying is that if in the scheme we say every character is N bytes then it means that to decode every character I should fetch N bytes but now it is a mix of 1 to N bytes. Is the decoding possible ?**

So thinking about this, the logical answer would be that in the first byte itself there should be an indicator of how
many of the subsequent bytes will form the character.

And we are right. [Wikipedia](https://en.wikipedia.org/wiki/UTF-8#Description) says that the rule is as follows

    if the first byte starts with 0 then it is a single byte char

    if the first byte starts with 110 then it is 2 bytes
    
    if the first byte starts with 1110 then it is 3 bytes
    
    if the first byte starts with 11110 then it is 4 bytes
   
    if the first byte starts with 111110 then it is 5 byte
    
    if the first byte starts with 1111110 then it is 6 byte
    
    

so it will read the first byte , 

    if the first byte starts with 0 then it is a single byte char so it decodes only that byte
    
    if the first byte starts with 110 then it is 2 byte so it decodes 2 consecutive  bytes
    
    if the first byte starts with 1110 then it is 3 byte so it decodes 3  consecutive bytes
    
    if the first byte starts with 11110 then it is 4 byte so it decodes 4 consecutive bytes
    
    if the first byte starts with 111110 then it is 5 byte so it decodes 5 consecutive bytes
    
    if the first byte starts with 1111110 then it is 6 byte so it decodes 6 consecutive bytes


after decoding x bytes it will consider the next byte to belong to next character.

Instead of putting it like the rules let us see in decimals the range and the byte change in them


Decimal range           | Hex range           | Number of Bytes
----------------------- | --------------------|----------------
0 - 127                 | 0000 - 007F         | 1
128 - 2047              | 0080 - 07FF         | 2
2048 - 65535            | 0800 - FFFF         | 3
65536 - 2097151         | 10000 - 1FFFFF      | 4
2097152 - 67108863      | 200000 - 3FFFFFF    | 5
67108864 - 2147483647   | 4000000 - 7FFFFFFF  | 6

<table>
<tr><td>hello</td><td>world</td></tr>
<tr><td>jumba</td><td>dance</td></tr>
</table>
ok,  If we say that there are bits indicating how many bytes make a character, then, where are the actual bits equivalent to the hex code mapping to each character be stored ?

It seems like it just packs the binary equivalent of the hex into the rest of the bits(other than the indicator for bytes). 
Keeping in mind the [Wikipedia table](https://en.wikipedia.org/wiki/UTF-8#Description), what I am trying to say is that it will fill the bits in place of the ‘x’.

[ë](http://www.utf8-chartable.de/unicode-utf8-table.pl?utf8=bin)

     110  xxxxx   10  xxxxxx 

     110  00011   10  101011	

          00011       101011  → binary equivalent of hex pointing to ë

[ɟ](http://www.utf8-chartable.de/unicode-utf8-table.pl?start=512&utf8=bin)

     110  xxxxx  10 xxxxxx 

     110  01001  10 011111	 

          01001     011111   → binary equivalent of hex pointing to ɟ

if the following is the set of digits to decode

11100000    10101101    10011111

the first byte starts with 1110 so it means we should decode 3 bytes to decode the character as expected (from the rules of table https://en.wikipedia.org/wiki/UTF-8#Description).

    1110xxxx    10xxxxxx    10xxxxxx

    11100000    10101101    10011111

    so  0000      101101      011111 is the binary to be decoded.

The binary is B5F in  hexadecimal (If you don't know to convert use this [binary to hex converter website](http://www.binaryhexconverter.com/binary-to-hex-converter) )
Now from [map](http://www.utf8-chartable.de/unicode-utf8-table.pl?start=2816&number=1024&utf8=bin) B5F means  ୟ
.

***


##Exercise:



How to decode this entire string (which is a collection of characters) ?

    01000010 01000001 11000011 10110000 11100010 10001011 10110011

**Pseudocode:**

###First Character

Get first byte 01000010

It starts with 0 so the character has only one byte so in 01000010

1000010 is to be decoded.

it is 42 in hex which [maps to B](http://www.utf8-chartable.de/unicode-utf8-table.pl?utf8=bin).

###Second character

Then it takes the next byte 01000001

It again starts with 0 so follow the similar procedure as above and we find that it maps to 
A

###Third character

Then now get next byte 11000011 so it starts with 110 so we should get 2 bytes to decode the character

So  11000011 10110000 will form the character.

00011110000 is the binary to be decoded.

it is F0  in hex  . and this hex [maps to ð](http://www.utf8-chartable.de/unicode-utf8-table.pl?number=1024&utf8=bin) .

###Fourth character

Then now take the next byte 11100010 .

It starts with 1110 so the three byte group will form a character.
so 

11100010 10001011 10110011 is the three byte group

0010001011110011 maps to hex 22F3 .

now see from map that 22F3 [maps to ⋳](http://www.utf8-chartable.de/unicode-utf8-table.pl?start=7936&number=1024&utf8=bin).

###Conclusion

so 01000010 01000001 11000011 10110000 11100010 10001011 10110011 meant BAð⋳.

(Use the [website](https://sites.google.com/site/nathanlexwww/tools/utf8-convert) to cross check your answers)


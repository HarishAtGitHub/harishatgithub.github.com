---
layout: post
title: "Symmetric and Asymmetric Encryption"
description: "Explaining Symmetric and Asymmetric Encryption"
category: cryptography
tags: [cryptography, symmetric encryption, asymmetric encryption]
---
{% include JB/setup %}


## Information Transmission

***

In the world dominated by internet there is common scenario where people need to communicate with each other.
During this communication process information is being transmitted from one person to another or one machine to another or between
machine and person. 

## Requirements while transmitting Information

***

Then came 2 different needs:

### Scenario 1:

When one person A sends information to another person B, he wanted his information to be indecipherable to all others except B.

### Scenario 2:

When one person A sends information to another person B, he wanted his information to be 
[tamper-proof](http://www.thefreedictionary.com/tamperproof) (Designed to prevent tampering or provide evidence of tampering).
He is not worried if the information can be read or not. But only things he is bothered about is that it should not be
manipulated by anyone in the middle.


## Solutions to Solve the needs:

***

### Solution 1:

Let both the parties A and B share a common key to the lock and lock the information using that lock. Ya, this works
well. A will write the information and then lock it with the key k and send it in internet and though evesdroppers will be
in the middle they cannot open the information even as they don't have the key k. There is not even the case of reading so no
possibility of tampering (except replacing the entire info). So this works as intended.

But this too has a problem. So far we sent the information securely but how to send the key securely first ? If the key 
is caught by a middleman he can unlock message.
One way is to meet the person B in person and hand it over. But this will not work as most people will be separated
geographically. 

So this solution has a problem. The problem lies in key sharing.

This kind of common key sharing approach is called symmetric encryption.


### Solution 2:

In solution 1 the problem was the common key sharing part. So lets find an approach to solve this.

what if instead of sharing a common key you have mapping keys. Let the  key pair be (K1,K2).
I have one copy of K1 and multiple copies of K2. and  Let us see how it solves each scenario ..
This is called asymmetric encryption.


**How it solves Scenario 1:**

K1 is with only the person who is intended to read the message and K2 is distributed among people who want to 
send messages to A. Now
when B wants to send message to A, he writes it and locks with K2 and passes it on.
This lock is a unique lock (like a padding lock, which can be pressed and locked by anyone, but can be opened by only the
one who has the key). It can be locked by K2 but opened by K1. 
So eventhough K2 is possessed even by evesdropper he cannot use it to open the lock as it can only be opened by K1. K1 is only
with the person who wants to read the message.

K1 which is kept secret is called private key

K2 which is distributed openly is called public key.

**How it solves Scenario 2:**

K1 is with only the person who is intended to send the message and K2 is distributed among people who want to 
read messages from A. Now A wants to send a message B and he does not want anyone to tamper it.
So what he does is he writes the message and then encrypts it with the key K1 and then sends the encrypted form too.this encrypted
form is called the signature. This process is called signing.
Now B receives it as message.encrypted-signature

the reader B will receive it and then read the message and then unencrypt the encrypted-signature and checks if both
are the same. If not the same it means that someone has changed it.

1. Though the [cracker](http://www.pctools.com/security-news/crackers-and-hackers/) changes the message, as he 
is not in possession of key K1, he will not be able to sign it. 

2. If the [cracker](http://www.pctools.com/security-news/crackers-and-hackers/) changes the signature even ?
Even if he signs it using some other key K3, the
person B when ecrypting the message with key K2, he will not get the new message sent by the cracker as the K1 
is not paired with K3. The equality check between message and the unencrypted form of message from signature will fail,
and so detecting tampering.


So solution 2 solves it perfectly well.

## Is there a way to break solution 2 ?

***

Let us see the simplified form of the original math behind it to answer the question .

$$ EncryptedMessage = (OriginalMessage)^e mod N $$

public key is (e,N)

two ways to break

**1) direct way (to get the message)**

as the equation has only one unknown
(as (e,N) is known as public key, and encrypted-message is known , then the other unknown which is the originalmessage can be known.)
but the problem is that there is [no efficient way to solve this equation](https://en.wikipedia.org/wiki/RSA_problem) .

**2) indirect way (to get the key to open the message)**

with the help of the private key d

as 

$$ OriginalMessage = (EncryptedMessage)^d $$

so if we figure out d then we can get the message 
but d is calculated using the factors of N which is p, q

d = function(p,q)

but the problem here is that the factoring of N (we know through public)  is damn difficult and 
[no efficient way has been found out yet](https://en.wikipedia.org/wiki/Integer_factorization).


## Applications:

***

Scenario 1 with solution 2 is seen in [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell)

Scenario 2 with solution 2 is seen in [Token based authentication - jwt](https://scotch.io/tutorials/the-ins-and-outs-of-token-based-authentication) to have tampering detection of token.


Special Thanks to [Johannes Passing](https://www.linkedin.com/in/johannespassing) for helping me with my doubts that came to my mind while trying to understand this concept.

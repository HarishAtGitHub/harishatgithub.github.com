---
layout: post
title: "Session based and Token based authentication"
description: "Session based and Token based authentication"
category: Token Based Authentication
tags: [Token Based Authentication]
---
{% include JB/setup %}

## Why do we need Session based and Token based authentication ?

***

HTTP is stateless. So by default every request is a new one and it holds no history or info about previous state.
But this cannot be an excuse to ask the user to enter username/password for each and every request. It is a rather
painful task.
So the solution is that once the user logs in with his credentials, we need to remember that the user has logged in.
The remembering can be done by giving him an identity value, with which he can come in next time. From the identity value we
can identify the user and decide if we should acknowledge/reject his request.

The last sentence is the key for the blog post. How to identify the validity of the user from his identity ?

## Session Based Authentication

***

The identity value is just a random string which has very little or no meaning. This value is attached on subsequent requests 
made by client to the server. The server on receiving this request will see if this value exists in database(or in memory).
If it exists, then the corresponding user details is fetched from database and then the request is further processed.

So for each and every request a database call or a network call(if the authentication information is store in some other geographically
separated system) is made. This is a time consuming process.

And as the authentication information is in a single database it is hard to scale. This is because even though
the services in the application can be separated out, the service will still need to rely on a network
call to the authentication service to authenticate each and every request.

So how to fix this ? the source of the problem is that the value or payload of user identity information to which each and
every identity value maps to is stored in database .
The solution to above comes in as a new form of authentication.


## Token Based Authentication

***

What if instead of reaching out to the database for some basic information like user info and expiry time, we store
it in the identity value itself ? Ya, then in this case we need not reach out to the database for authenticating
every request.

This is the idea behind token based authentication. We encode, encrypt and sign basic information like user info and expiry time
and put it in the token itself.

So if the user logs in the next time, the token needs to be decrypted and the validity(authenticity) needs to be checked to further
process the user request. No more database or network call.

**Is it fast than the session based authentication ?**

It is observed that the time spent in decrypting and validating the token is far less than that taken while
making a database or network call.

**How can it help scale ?**

As now we are not confined to a separate authentication information storage, separating the services is
no more a nightmare as each and every service can have the code logic to decrypt and validate the token within
itself.

Had there been a separate authentication storage, then in case of websites with too many requests, the auth storage
device will be bombarded with a lot of requests and hence causing slow down of all services which rely on this auth
storage. So this way of token based authentication definitely helps scaling.

**If the information is there in payload, is it not a security compromise ?**

Only basic inforation which are not sensitive but still enough for validating the user is only stored in token.
So this is not a security compromise.
As the token is out there in public, it is adviced not to put sensitive information in it.

**What if someone tampers the token to his own advantage ?**

This can be avoided by signing the token on the server side. Then, only people who possess the public
key can read data. Even though they have the public key, they cannot add information to the token
as they do not have the private key with which the signing is done. If tampered, the signature will not match
and hence tampering can be detected easily. To know more about this encryption, please read my blog post on
[Symmetric and Asymmetric encryption](http://harishatgithub.github.io/cryptography/2015/12/20/public-key-private-key/).
The blog discusses how this method is safe and secure.

**References:**

***


1. [The Ins and Outs of token based authentication](https://scotch.io/tutorials/the-ins-and-outs-of-token-based-authentication)
2. [Token based architecture design](https://www.youtube.com/watch?v=xgkNe6R4Un0)
3. [Cookies Vs Tokens](https://auth0.com/blog/2014/01/07/angularjs-authentication-with-cookies-vs-token/)
4. [RSA Cryptosystem](https://en.wikipedia.org/wiki/RSA_(cryptosystem))


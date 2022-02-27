---
layout: post
title:  "The Meaning of ACID Consistency"
summary: "ACID consistency depends on the application's notion of invariants"
excerpt: "ACID consistency depends on the application's notion of invariants"
date: 2022-02-27 07:00:00 -0700
tags: mysql
published: true
mathjax: true
image: /assets/kleppmann.jpg
---

For backend/full-stack software engineers and system design interviews, I highly recommend ["Designing Data-Intensive Applications: The Big Ideas Behind Reliable, Scalable, and Maintainable Systems"](https://amzn.to/3IulnU3) by Dr. Martin Kleppmann.

<a target="_blank"  href="https://www.amazon.com/gp/product/1449373321/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1449373321&linkCode=as2&tag=imiguel-20&linkId=08cc53d41e2a038d1cbc333a4eb285b1"><img border="0" src="//ws-na.amazon-adsystem.com/widgets/q?_encoding=UTF8&MarketPlace=US&ASIN=1449373321&ServiceVersion=20070822&ID=AsinImage&WS=1&Format=_SL250_&tag=imiguel-20" ></a>

I want to share how this book explains ACID consistency.  ACID is a set of properties intended to guarantee data validity after any transaction, which can sometimes be composed of multiple database operations.  A transaction satisfies the ACID properties if it

- completely fails, returning the database to its original state by discarding all writes, when any of its statements fail to complete (atomicity),
- transitions the database from one valid state to another (consistency),
- does not conflict with concurrent execution of other transactions (isolation), and
- remains committed regardless of hardware faults, database crashes, etc. (durability).

In my experience, when ACID consistency is explained, the focus is usually on defining integrity contraints in order to prevent database corruption (please see [https://en.wikipedia.org/wiki/ACID](https://en.wikipedia.org/wiki/ACID)).

However, an application's notion of consistency may require enforcing constraints that the database cannot.  Therefore, the application is actually the responsible one for preserving consistency by defining its transactions correctly.  For this reason, Dr. Kleppmann concludes that consistency is actually a property of the application unlike atomicity, isolation, and durability and that "the letter C doesn't really belong in ACID", see page 225. 


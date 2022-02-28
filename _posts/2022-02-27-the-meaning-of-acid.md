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

[![Kleppmann Book Cover](/assets/kleppmann.jpg){: style="margin-bottom: 1em;"}](https://amzn.to/3IulnU3) I want to share how this book explains ACID consistency.  ACID is a set of properties intended to guarantee data validity after any transaction, which can sometimes be composed of multiple database operations.  A transaction satisfies the ACID properties if it

- completely fails, returning the database to its original state by discarding all writes, when any of its statements fail to complete (atomicity),
- transitions the database from one valid state to another (consistency),
- does not conflict with concurrent execution of other transactions (isolation), and
- remains committed regardless of hardware faults, database crashes, etc. (durability).

In my experience, when ACID consistency is explained, the focus is usually on defining integrity constraints in order to prevent database corruption (please see [https://en.wikipedia.org/wiki/ACID](https://en.wikipedia.org/wiki/ACID){: target=":blank"}).

However, an application's notion of consistency may require enforcing constraints that the database cannot.  So, at the end of the day, it is actually the application that is responsible for preserving consistency by defining its transactions correctly.  For this reason, Dr. Kleppmann concludes that consistency is actually a property of the application, unlike atomicity, isolation, and durability, and that "the letter C doesn't really belong in ACID", see page 225.


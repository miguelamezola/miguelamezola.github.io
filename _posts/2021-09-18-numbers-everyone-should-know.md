---
layout: post
title:  "Numbers Everyone Should Know"
summary: "Common performance numbers that everyone should know"
date:   2021-09-18 08:30:00 -0700
tags: system design, performance, estimates
published: true
---

These performance numbers were put out by [Jeff Dean](http://research.google.com/people/jeff/){:target="_blank"} more than 10 years ago.  Since hardware has improved, they are now outdated.  However, they still give us some idea of how the operations compare to one another.  [Simon Eskildsen](https://sirupsen.com/){:target="_blank"} has collected updated numbers in [https://github.com/sirupsen/napkin-math](https://github.com/sirupsen/napkin-math){:target="_blank"}.

|Operation                                  |Latency (ns)   |
|-------------------------------------------|---------------|
|L1 cache reference                         |0.5            |
|Branch mispredict                          |5              |
|L2 cache reference                         |7              |
|Mutex lock/unlock                          |25             |
|Main memory reference                      |100            |
|Compress 1K w/cheap compression algorithm  |3,000          |
|Send 2K bytes over 1 Gbps network          |20,000         |
|Read 1 MB sequentially from memory         |250,000        |
|Round trip within same datacenter          |500,000        |
|Disk seek                                  |10,000,000     |
|Read 1 MB sequentially from disk           |20,000,000     |
|Send packet CA->Netherlands->CA            |150,000,000    |

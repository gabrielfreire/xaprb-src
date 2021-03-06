---
title: "A review of Relational Database Design and the Optimizers by Lahdenmaki and Leach"
url: /blog/2010/09/19/a-review-of-relational-database-design-and-the-optimizers-by-lahdenmaki-and-leach/
date: "2010-09-19"
categories:
  - Reviews
  - Databases
tags:
  - PostgreSQL
---

[Relational Database Index Design and the Optimizers]({{< amz 0471719994 >}}). By Tapio Lahdenmaki and Mike Leach, Wiley 2005.

[![Relational Database Index Design and the Optimizers](/media/book-covers/relational-database-index-design-optimizers.jpg# 3dbook)]({{< amz 0471719994 >}})

I picked this book up on the advice of an Oracle expert, and after one of my colleagues had read it and mentioned it to me. The focus is on how to design indexes that will produce the best performance for various types of queries. It goes into quite a bit of detail on how databases execute specific types of queries, including sort-merge joins and multiple index access, and develops a generic cost model that can be used to produce a quick upper-bound estimate (QUBE) for the execution time of a query. The book focuses on DB2, Oracle, and SQL Server, but applies equally well to MySQL and PostgreSQL.[^1] I learned a lot from this book, and will add it to my list of essential books.

There are too many myths and rules of thumb about index design. This book debunks them pretty thoroughly. It walks the reader through the process of understanding what a database does to execute a query, and how much that costs; and then what a database does to execute a data modification, and how much that costs. Given this knowledge, you can answer questions such as "what is the ideal index for each of these two queries?" and "should the queries have separate indexes, or is it better to find a compromise that will be good for both of them?" and even "how much slower will the compromise be for each query?" In many cases, the results are non-obvious, and often don't agree with the rules of thumb you might have been taught. Generally, the book concludes, we should use indexes much more than we often do, and we should not hold irrational fears about the cost of maintaining indexes.

After reading this book, you'll understand what makes an index good or bad for a query (a three-star ranking system), what makes a query possible or impossible to index ideally, the quick upper-bound estimate of execution time, the Basic Question, finding the cheapest adequate index, difficult predicates, index slices, and a host of other valuable concepts. In addition, there's an entire chapter on a method for finding queries that are not well indexed. Some of the methods in this book are things I already had notes to implement in Maatkit tools, but others are new to me. The method of finding promising culprits is something I learned in this book, and I think it's very valuable for a tool such as mk-query-digest with the Percona enhancements to the slow query log.

There are a few things I'll point out so it doesn't seem like an unqualified endorsement. One, the book is not as easy to read as it could be. The editors should have removed 99% of the places where the authors italicized or otherwise emphasized words; there's a lot of emphasis on relatively unimportant or random words. Barely a sentence is free of italics. Second, the book was written in 2005 and today's machines have much more memory. (This generally makes the book's points more valid, not less valid.) Finally, the cost model is based on spinning disks, and the QUBE method needs slightly different parameters to work correctly on solid-state storage, or indeed even many modern SANs. However, that's not a big deal---just measure your storage system's performance, plug in the correct random versus sequential access time, and the model is still valid.

[^1]: Note that although at the time of writing PostgreSQL did not yet support index-only queries, which is a major focus of the book, the various cost models apply equally well. One must simply account for the cost of the table access, and not assume that the index is the only thing that's touched by the query. In general, you're going to need to know the internals of your database server to apply this book's wisdom.



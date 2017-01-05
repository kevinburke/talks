# Fix your Oncall Problems by Using a Database

## Description

The ORM's in many common application frameworks issue database queries in a way
that can cause concurrency problems, and cause your team to get paged. I'll
show you how using the database's built-in correctness properties helped our
business and made our on-call teams much happier.

## Abstract

Databases have never been better at guaranteeing integrity in the face of
multiple concurrent writes and reads. However, a lot of ORM's and user-level
applications ignore the progress being made at the database level, sacrificing
latency and correctness for flexibility. Pushing this work back down to the db
layer can result in faster, safer, and more reliable integrity guarantees --
everyone wins!

At an on-demand logistics startup, data consistency problems were the number
one cause of oncall problems. At the core of these issues was an ORM that made
it difficult to write correct code and use our database. On top of this, we
had to deal with buggy clients that would frequently make repeat or incorrect
requests.

We'll walk through the fixes we made, and how applying the same techniques can
benefit your oncall team and your company. We'll also discuss why ORM's made
the tradeoffs they did -- and how to push for better ORM's in the future.

This talk includes code samples and a small Rails app to demonstrate how to
make these changes.

## Who is this for?

Anyone who uses an ORM to read/write data from a database.

## Main takeaway

You'll learn how the database queries you write, and the ORM you use can be
unnecessarily slow, and how your queries can lead to data corruption. You'll
learn how to write better queries and how to evaluate an ORM.

## Prerequisite knowledge

Basic knowledge of SQL would be nice.

## Additional notes

I haven't given this talk but I am really excited to share what we did. We went
from an application in a really bad state to something that was really correct,
performant and fast in about 18 months and I would love to share this story.

I am super super open to feedback and tweaking to make this as good as possible
for your audience.

I've previously covered some of this material in a blog post here:
https://kev.inburke.com/kevin/faster-correct-database-queries/

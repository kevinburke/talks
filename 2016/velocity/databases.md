# Faster, More Correct Queries with Your ORM

## Description

ORMs makes it very easy to programmatically access your data. In some common cases ORMs make inefficient, incorrect queries! In this talk we'll discuss situations where your ORM can lead you astray, and how you can write faster, more *correct* database queries. We'll also discuss other strategies for maintaining integrity when the rest of your clients are causing trouble.

## Abstract

Say you define an ActiveRecord class with a uniqueness constraint. What actually happens when you try to create a new user? It turns out Rails will make 4 (four!) roundtrips to the database.

1. BEGIN a transaction.

2. Perform a SELECT to see if any other users have that email address.

3. If the SELECT turns up zero rows, perform an INSERT to add the row.

4. Finally, COMMIT the result.

This is both slower and less correct than adding a uniqueness constraint on the column in MySQL or Postgres, and then attempting to write the value. If a separate value exists, the database will throw an error, which you can handle in your application. Furthermore, the ActiveRecord approach can be *incorrect* at the database's *default transaction isolation level*, this can lead to multiple records being inserted with the same email address!

We'll discuss a similar approach for checking whether a foreign key exists - just try writing the data and let the database tell you whether the foreign key exists or not! I'll show the example latencies doing each of the above.

We'll move on to discuss state machines. Imagine multiple threads are trying to operate on a record, say, two threads are trying to assign a driver to different pickups. How can you handle this situation? One way is to fetch the driver, check the driver's state, then update it in the application, and call save(). Unfortunately there's a race condition between reading the driver and calling save, where two threads can update the driver, and both believe they've succeeded! Instead, [try a WHERE clause][state-machines] - `UPDATE drivers SET status='assigned' WHERE status='unassigned'`, which will only work for one thread. We'll walk through how to implement this in ActiveRecord.

Another hurtful pattern - updating account balances based on stale data! Say you want to charge $20 and you fetch the account, subtract $20 from the balance, and call `save()`. If another thread tries to do this at the same time, you can end up losing one of the writes! Instead you want to issue a *relative update*: `UPDATE accounts SET balance = balance - 20`. I'll discuss how to implement this in Rails, as ActiveRecord makes this a little tricky.

I'll conclude by discussing a *framework* for fast, correct application development. Think about the database query (or queries) you want to issue first, then work backwards to how you want to implement them! This is the path to performant, correct Rails applications. Also - your database can probably do a lot of really cool things!


## Outcomes

Hopefully if you view this talk you'll think slightly differently about your ORM, and some of the problems with ActiveRecord (and many other ORM's) default query patterns. Going forward, you'll be able to write more correct applications. The good news is the talk's lessons are generalizable to pretty much any relational database, and the ORM lessons apply to most ORM's that are out there.

You'll be able to defend against things like rogue clients! Even when things are going wrong outside of your API, you can ensure that your database is consistent and your application is doing the right thing.

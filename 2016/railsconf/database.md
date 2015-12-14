# Write Faster, More Correct Database Queries with Rails

## Abstract

ActiveRecord makes it very easy to ask questions of your data, and add new records. However, in some cases ActiveRecord issues inefficient, incorrect queries! In this talk we'll discuss situations where ActiveRecord leads you astray, and how you can write faster, more *correct* database queries in Rails. We'll also talk about how to maintain the integrity of your database in situations where clients are making competing requests for the same resource, like the state of an item or a bank balance.

## Details

Say you define an ActiveRecord class that looks like this:

```
class User < ActiveRecord::Base
  validates :email, uniqueness: true
end
```

What actually happens when you try to create a new user? It turns out Rails will make 4 (four!) roundtrips to the database.

1. BEGIN a transaction.

2. Perform a SELECT to see if any other users have that email address.

3. If the SELECT turns up zero rows, perform an INSERT to add the row.

4. Finally, COMMIT the result.

This is both slower and less correct than adding a uniqueness constraint on the column in MySQL or Postgres, and then attempting to write the value. If a separate value exists, the database will throw an error, which you can handle in your application. Furthermore, the ActiveRecord approach can be *incorrect* at the database's *default transaction isolation level*, this can lead to multiple records being inserted with the same email address!

We'll discuss a similar approach for checking whether a foreign key exists - just try writing the data and let the database tell you whether the foreign key exists or not! I'll show the example latencies doing each of the above.

This problem was discussed in a recent paper by [Peter Bailis et al, "Feral Concurrency Control: An Empirical Investigation of Modern Application Integrity"][paper]. We'll discuss the paper's conclusions and implications a little bit.

We'll move on to discuss state machines. Imagine multiple threads are trying to operate on a record, say, two threads are trying to assign a driver to different pickups. How can you handle this situation? One way is to fetch the driver, check the driver's state, then update it in the application, and call save(). Unfortunately there's a race condition between reading the driver and calling save, where two threads can update the driver, and both believe they've succeeded! Instead, [try a WHERE clause][state-machines] - `UPDATE drivers SET status='assigned' WHERE status='unassigned'`, which will only work for one thread. We'll walk through how to implement this in ActiveRecord.

Another hurtful pattern - updating account balances based on stale data! Say you want to charge $20 and you fetch the account, subtract $20 from the balance, and call `save()`. If another thread tries to do this at the same time, you can end up losing one of the writes! Instead you want to issue a *relative update*: `UPDATE accounts SET balance = balance - 20`. I'll discuss how to implement this in Rails, as ActiveRecord makes this a little tricky.

We'll also (briefly) discuss transaction isolation levels, another thing touched on in Bailis's paper; this is relevant because you might make decisions based on a user's balance being above 0, but in some transaction levels, the balance update can combine with another transaction to have the balance fall below 0!

If we have time, we might cover some cool features of Postgres that help with correctness - CHECK constraints, and [partial unique indexes][partial-indexes].

I'll conclude by discussing a *framework* for fast, correct application development. Think about the database query (or queries) you want to issue first, then work backwards to how you want to implement them! This is the path to performant, correct Rails applications. Also - your database can probably do a lot of really cool things!

[paper]: http://blog.acolyer.org/2015/09/04/feral-concurrency-control-an-empirical-investigation-of-modern-application-integrity/
[state-machines]: https://kev.inburke.com/kevin/state-machines/
[partial-indexes]: http://www.postgresql.org/docs/current/static/indexes-partial.html

## Outcomes

Hopefully if you view this talk you'll think slightly differently about your ORM, and some of the problems with ActiveRecord (and many other ORM's) default query patterns. Going forward, you'll be able to write more correct applications. The good news is the talk's lessons are generalizable to pretty much any relational database, and the ORM lessons apply to most ORM's that are out there.

You'll be able to defend against things like rogue clients! Even when things
are going wrong outside of your API, you can ensure that your database is
consistent and your application is doing the right thing.

## Intended Audience

Anyone who needs to programmatically access or update data in a data store, and
uses an ORM to do so, should benefit from watching this talk.

## Pitch

Pretty much every app out there has a database component, but a lot of applications and app developers query the database in a way that will fail with concurrency, or clobber UPDATEs, or worse. In some cases this can lead to a lot of pain and a lot of wasted time finding concurrency errors and fixing bad state in the database.

I've been leading the charge for correctness and performance in the API at Shyp. One time we released our iOS app with a defect which issued multiple HTTP requests for every request. If we weren't careful with how we wrote our API code, we would have ended up assigning multiple drivers to a pickup, charging customers multiple times for the same item, creating multiple records for a signup request, or worse! These would have led to slower development times, and unnecessary, increased load on the rest of the company.

I want to share what we've learned and make sure no one else can make the same mistakes.

Furthermore, I feel like NoSQL has grown in popularity in part because people aren't using all of the features of their relational database! I hope to talk a little bit about what databases are good at to educate people about the cool things that MySQL and Postgres can do.

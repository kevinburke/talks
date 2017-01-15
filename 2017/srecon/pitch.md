## Short Description

The ORM's in many common application frameworks issue database queries in a way
that can cause concurrency problems, and cause your team to get paged. I'll
show you how using the database's built-in correctness properties helped our
business and made our on-call teams much happier.

## Full Presentation Description

Databases have never been better at guaranteeing integrity in the face of
multiple concurrent writes and reads. However, a lot of ORM's and user-level
applications ignore the progress being made at the database level, sacrificing
latency and correctness for flexibility. We'll demonstrate the problem with a
simple Rails app, then look at some sample code in a live web application.

Using the database incorrectly caused many headaches for our on-demand startup
- drivers or pickups would get "stuck" in an unrecoverable state, requiring
action from the engineering team.

Fixing this problem took a long time but helped our business - drivers no
longer got stuck, users weren't unhappy - and our oncall team, who weren't
getting paged as often. The fixes we implemented helped correctness a lot, but
ran counter to the way our ORM was "designed" to be used. It also required
a cultural shift on the team in how we thought about writing data to the
database.

We'll walk through the fixes we made, and how applying the same techniques can
benefit your oncall team and your company. We'll also discuss why ORM's made
the tradeoffs they did and how to push for better ORM's in the future.

## Outline

Some background: I worked at Shyp, an on-demand logistics startup. We were
operating in 4 cities with hundreds of drivers. We had a single Postgres
database. This talk will be useful for customers with data of that size - the
advice won't be as useful if you are running Cassandra or Kafka.

The primary objects we'll be talking about are drivers and pickups, each of
which has states like ASSIGNED or AVAILABLE. The pager is a little different
when real people are affected by your errors. A lot of our pager issues were
due to data consistency problems.

#### Enforce database constraints at the database layer

What happens when you insert a user in Rails? Live demo creating a User model
with an email address, creating a User.

Show via the logs that when you create a User, ActiveRecord generates 4 queries
- 4 roundtrips to the database - to insert a user:

- starting a transaction
- checking whether any records match the unique key
- inserting a new record
- committing the transaction

Not only is this slow, it can lead to multiple records being created with
the same ID! This is a solved problem at the database level - use a UNIQUE
constraint. But a lot of ORM's don't do this.

Same problem exists with foreign keys. The database can and should check
whether a foreign key exists; don't do this in your ORM or application.

#### "Try the Write"

You can check whether a record or a file exists, and then try to update it if
it's not in that state. But this leaves you vulnerable to a race condition.

Relevant for our team - we had a lot of code that looked like this:

```ruby
pickup = Pickups.find_by_id("id")
if pickup.state != 'AVAILABLE'
    return
pickup.state = 'ASSIGNED'
pickup.driver_id = 'driver123'
pickup.save()
```

There's a race condition here! If another thread fetches the pickup at the same
time, you'll end up with two pickups assigned to the same driver. Which your
system may not be able to handle correctly - ours assumed that one driver had
one pickup and that was it.

It turned out *our iPhone app had buggy retry logic* and would occasionally
submit the same pickup multiple times in a short time frame! So we hit this
problem a lot.

A better approach is to issue a single UPDATE:

```sql
UPDATE pickups SET state='ASSIGNED' WHERE state='AVAILABLE' AND id='pickup123'
```

We added a StateMachine class, abstracted this UPDATE query, forced every state
transition to go through this class. This solved a lot of the problems.

This is undoubtedly the correct approach, and faster then fetch-and-save. But
it is *not* easy to do this in most ORM's which tend to make it easier to fetch
an object, then modify it and call save(). Don't use save(); we ripped it out
of our codebase.

Note - ORM's are generally not good at handling constraint failures - UNIQUE
constraints or foreign key failures or UPDATE failures. They are much better
at "do a read first and check whether the data is OK, then try the write." But
you should just try the write.

From a ORM writer's perspective, the harder thing about handling "try the
write" is that constraint failures are database specific - each database has a
unique error format.

We had to write a lot of code to handle constraint failures better, since our
ORM did not do a good job at exposing the actual problem.

#### Transactions

Our ORM didn't support transactions. This led to partially successful updates -
we'd update one table, succeed, then fail at updating the second one, and 500
server error without fixing the first table.

(30 second explanation of what transactions are and why you want them and
most Mongo implementations don't support this)

We wrote our own transaction library, hooked it up to work
with the StateMachine, we didn't have partial updates again.

#### Cultural changes

We had to make some cultural changes to make this work. For people implementing
a controller endpoint, we started making people focus first on the database
queries (in SQL) they needed to make, whether those were correct, then figuring
out how to implement those at the application layer. Also:

- Writing/sharing blog posts internally about how to do this better

- Focusing on queries in code review

- Adding/promoting guidelines in internal wiki - don't use save(), use database
constraints like foreign keys, check constraints and unique constraints.

- Teaching everyone how to view the raw database query logs when they ran the
tests or ran a dev server - so they could see what the application was doing
from the database's perspective.

- We modified the Postgres Homebrew recipe so it would set up logging right
away - Postgres doesn't enable query logs by default, and setting this up is a
pain (see https://github.com/kevinburke/enable_pg_logs)

The result - at the same time we were massively scaling up the number of
pickups we were doing, and scaling up the team, we were reducing the number of
pager incidents due to data problems.

#### Conclusion

Your choice of ORM matters a lot. Good ORM's make it easy to write correct code
and handle constraint failures. Bad ORM's make it very difficult and will cause
headaches for your oncall team.

When evaluating an ORM consider:

- Emphasis on UPDATEs over save()

- If you use the ORM's schema builder/migration, it should use the database's builtin constraints instead of implementing those constraints in memory

- Check that your ORM handles constraint failures in a sane way

- Your ORM should support transactions

How you use your ORM can have a big difference on your application's
correctness, your customers' happiness and your oncall team's happiness. It can
also lead to faster queries!

Make it easy for your team to see what is going on at the database layer -
expose query logs and write tools that make it easy to translate between a ORM
query and the queries that are set in the database.

Thanks to Peter Bailis, Charity Majors, Ines Sombra for contributing ideas and
feedback!

## Notes to the Program Committee

I haven't given this talk before, but I am really excited to share what we did.
We went from an application in a really bad state to something that was really
correct, performant and fast in about 18 months and I would love to share this
story.

I am super super open to feedback and tweaking to make this as good as possible
for your audience.

I've previously covered some of this material in a blog post here:
https://kev.inburke.com/kevin/faster-correct-database-queries/

I've given about ten talks at conferences in the past. Here is me giving
a talk at Write the Docs in 2015 - probably the best talk I've given:
https://www.youtube.com/watch?v=sQP_hUNCrcE

## Bio

Kevin Burke (https://kev.inburke.com) likes building great experiences. He
helped scale Twilio and Shyp, and currently runs a software consultancy. Kevin
once accidentally left Waiting for Godot at the intermission.

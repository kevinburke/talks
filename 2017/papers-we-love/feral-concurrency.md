# Feral Concurrency

Paper Link: http://www.bailis.org/papers/feral-sigmod2015.pdf

### Summary

Databases have never been better at guaranteeing integrity in the face of
multiple concurrent writes and reads. However a lot of ORM's and user-level
applications ignore the progress being made at the database level, sacrificing
latency and correctness for flexibility. We'll demonstrate the problem with
a simple Rails app, then look at some sample code in a live web application.
We'll look at what the more correct alternative would be, and discuss the
tradeoffs used by ORM's like ActiveRecord, SQLAlchemy and Waterline. We'll look
at the paper's results on the correctness of popular open-source libraries.
Finally we'll prototype some application-level libraries that help programmers
opt in to the powerful correctness primitives that modern databases offer.

### Audience

This talk is geared at programmers and companies where correctness is more
important than scalability - you have a few databases with a single primary
each, and/or you are using an ORM to interface between your web application
and your datastores. I believe this covers most angel/ series A-B startups,
and should cover a lot of the web traffic (users, user settings, billing) at
larger companies. This talk might not be that useful for people who are used to
high-availability systems like Kafka or Cassandra, where the guarantees offered
by the database are geared more toward availability and it's more difficult to
prove or provide strict guarantees about correctness and consistency.

### Outline

- What happens when you insert a user in Rails? Live demo creating a User
  model with an email address, creating a User. Show everyone where to find the
  database logs and how to tail them (This was really important on my last team
  for helping people figure out what the application was doing)

- Show via the logs that ActiveRecord generates 4 queries - 4 roundtrips to the
database to insert a user:

    - starting a transaction
    - checking whether any records match the unique key
    - inserting a new record
    - committing the transaction.

- Intro the paper. At the default Read Committed isolation level, even with the
  4 query approach, it's possible to end up with duplicate records.

- Demo a better way - use a table constraint, and just try the insert. If it
  fails, report the error to the user.

- Broader principle: _just try a write_ and if it fails handle failure, instead
  of asking permission first.

- Which ORM's do it which way? SQLAlchemy uses the 1 query approach. Django,
  Waterline, ActiveRecord use the 4-query approach.

- Why do ORM's use the 4-query approach? Brief discussion - you get some
flexibility in that your ORM might write to a datastore that doesn't support
database constraints. If you try a write and get a DB failure, you have to
parse/normalize constraint failures from N different databases, vs. if you
query and get 0 rows, you can throw your own custom error in 1 place.
At extreme scales, constraints hurt write throughput.

- Back to the paper. Cover feral concurrency, summarize the paper's findings:
many open source Rails libraries make poor use of concurrency constraints, if
they do at all.

- Show the costs of doing things a bad way - the consequences of allowing
multiple records with the same unique ID, or of fetching a record, updating a
field, then calling `.save()`, while another thread operates on the same record
- causing a lost update.

- Where application layer constraints do OK - catching errors before even
attempting a database write. If you attempt to write a string to an integer
field, or a non-email-address to a email field, most ORM's will catch + error
before you attempt a database write. Which is CRDT-okay!

- Show ORM properties that are harmful to correctness - for example, `.save()`
- and demonstrate some libraries and patterns which are helpful, for building
e.g. state machines or safely updating an account balance. Model the repository
pattern. Discuss config settings - one click or command to enable verbose
database query logs would be helpful for local debugging in many cases.

- Conclusion - we have some education to do! And better libraries to design. If
there's time, here are some other places for improvement between ORM design and
the best practice - storing UUID's as strings, or states as strings instead of
enums.

For more context, I've covered this topic in blog post form here:
https://kev.inburke.com/kevin/faster-correct-database-queries/, though some of
it is orthogonal to the paper and has been omitted here.

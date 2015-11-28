At Shyp, a single Node app powers an API for consumers, drivers, and warehouse
technicians. Generally our server-side code has been pretty reliable, but like
any company, we've shipped outages to production.

This talk will show, with detailed examples, how poor use of callbacks, silent
failures, JSON, and more contributed to outages at Shyp. We'll discuss exactly
how our team of generalists failed to anticipate the problems with the code,
and the ways in which we worked around it. Hopefully they'll help you write
better Javascript, and better libraries.

## Explain the theme and flow of your talk.

Hey! The talk layout should be pretty simple - four or five stories about
production outages at Shyp and the lessons that can be drawn from each of them.
Hopefully these examples will help inform the audience about some ways that
poor design contributed to production errors, and how to

1. Unhandled EventEmitter errors caused a crash. We added a metrics client
   which crashed production when the 3rd party metrics server began returning
   502's.

    - The library's EventEmitter behavior was undocumented
    - We wrapped Metrics code in a try/catch, handled errors, assumed this was
      enough
    - JS runs all code in the process in the same event loop - a crash anywhere
      crashes everything.

2. Callbacks are dangerous

    - It's possible to hit a callback more than once
    - If the caller does not pass all of the arguments, can lead to 'Cannot
      call X of undefined' crashes
    - It's possible to not hit a callback

    We had a Metrics.timing function that accepted a callback. When the
    third party metrics server started crashing, we tried commenting out the
    function body while we investigated another solution. This led to callbacks
    not being hit. Solution was to avoid threading critical code through a
    callback, and add a start() line above the code in question.

3. Passing around null is dangerous

    - Many libraries return null/undefined if the input is invalid, vs.
      throwing an error. Examples: _.findWhere(null), moment(null),
      Model.findOne('unknown') in Waterline
    - Errors can propagate silently. For the moment example, we were
    creating/adding days to/displaying invalid dates without being aware that
    the code was incorrect.
    - Replaced with our own interfaces that throw/error loudly on invalid
    input. Would have crashed the server, but we wouldn't have persisted
    invalid state for days. Similar approach for Waterline

4. JSON is dangerous

    - Have several database columns that store JSON data
    - JSON data in columns has led to several crashes, most often invalid input
      being written to the database
    - Javascript makes it easy to define implicit dictionaries/add more keys
    to dictionaries and return them. We didn't know what keys/values existed
    on objects being returned from our API because they were added on an adhoc
    basis on different endpoints. Clients are confused about which keys can
    exist.
    - We use a tool called Kue for background job processing (invoicing,
    email sending, etc). Kue serializes data by calling JSON.stringify.
    JSON.stringify omits keys if the values are undefined. Led to several
    ReferenceErrors when keys were omitted, we expected them to be there in a
    template.

    Betterments:

    - only pass simple data (strings, objects with 1 key) to Redis, do all
    necessary data fetching in the worker.

    - ensure all objects being sent back via the API are an instance of some
    documented class object - even if they seem dumb. no more arbitrary keys
    and values.
    - avoid using JSON for new database columns. use the database's type
    system, relational data, and required columns to enforce data consistency

I'll just go from one to the next - setting up the failure, walking through the
actual crash that occurred, and then discussing betterments from each one. I'll
conclude with some more general comments - this is fairly different than the
direction that a lot of ORM's, database technologies, and Javascript patterns
would have you go, but we feel like they've been essential to avoid making
further mistakes in the vein of the ones presented above.

## Why is this topic important

- Some Javascript patterns make it difficult to write correct code

- writing correct code helps you get raises

- you may want to consider another language

## Why is this topic pertinent? What is your involvement in this topic?

There are a lot of people who may be considering Node for their new company, or
new server-side project, who could benefit from not making the same mistakes
that we did. Many of the errors above involved our inability to predict how
a library would behave with the inputs we were passing it - library authors
can use this data when they are documenting or designing new libraries for
widespread use.

I think that some Javascript idioms - passing null/undefined to signal an
error, using callbacks when I/O is not involved, implicitly defining/passing
around JSON objects - make it difficult to write correct code, and I would love
to share the things we've done and our approaches to the problem, since I think
they are fairly unique in the Javascript community.

I caused a fair number of the crashes above, and sat in every postmortem where
we diagnosed the root cause and how to debug it.

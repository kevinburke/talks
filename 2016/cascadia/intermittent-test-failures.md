# Detecting and Preventing Intermittent Test Failures

## The story you'd like to tell

Some of our tests pass _most_ of the time, but fail every tenth or hundredth
time you run the test suite. These are particularly pernicious in Javascript
test suites, because everything that hits the network must run asynchronously,
leads to problems when callbacks fire in an unexpected order. This can lead to
a dangerous culture, where developers just hit "Rebuild" when the tests fail,
and legitimate problems with the build are dismissed.

Intermittent test failures usually indicate a deeper problem with your
application code. But they can be really tough to track down, precisely
*because* they happen infrequently! I'll share some strategies for forcing
infrequent problems to become more frequent, so you can reproduce them and fix
them.

I'll walk through some of the most frequent sources of intermittent test
failures. I'll give some examples from our experience at Shyp:

- Examples of code and tests that failed every hundredth build

- How we found and fixed those problems, even if it took us 2-3 months to
identify them.

- How fixing those problems improved the speed and stability of our codebase:

    - We found time-sensitive code that didn't work properly outside of business hours

    - We found database queries that would succeed in part and error in part.

    - Code throwing errors synchronously where an asynchronous error was expected

    - A database query that deadlocked occasionally was also inefficient;
        fixing it sped up the test suite!

- How we made sure those problems wouldn't come up again in the future, or
  would be easier to debug if they did. For example:

    - Enabling database query logs in all test environments, and ensuring
      developers knew how to read them/associate them with a given test.

    - Automatically failing the build if certain dangerous conditions were in
      place - for example, unhandled Promise rejections.

    - Removing or patching libraries which contributed to test instability

    - Checking for open database connections at the end of every test

    - Improving the design of our codebase to facilitate better testing

    - Introducing request/database helpers which automatically checked/reported
      errors.

The same techniques you use to track down intermittent failures in tests will
help you write better tests, faster, and help you figure out what's going wrong
in your Javascript application more quickly. They'll also help you figure out
the stability of your application!

I'll close with two messages: One, a philosophy that every intermittent test
failure needs to lead to some kind of improvement, either in the errors being
asserted, the debugging information that's available, or in the application
code. Two, a message of hope: It might look impossible at first, but you have
the power and ability to fix every problem! You just need to get the right
instrumentation in place.

## Speaker Bio

![](https://kev.inburke.com/photos/squarephoto.png)

Kevin works on engineering success at [Shyp](https://shyp.com). When he's not
trying to make the build more reliable, he's watching sports, reading great
stories or learning how to get better at what he does.

# Taking your test suite from 100 seconds to 100ms

It used to take 100 seconds at Shyp before the first test started running. Now
it takes 100ms. How did we accomplish that?

I will walk through the specific steps we took to improve our test boot speed,
as well as general tools you can use to instrument, investigate, and improve
the performance of your test suite. This will cover the gamut, from:

- Running tests in a performance way in a VM

- What actually happens when you call `require`

- Using Javascript, UNIX/Linux, and database-specific tools to instrument your
build and determine how long individual parts are taking

- Figuring out how to write your tests and application code so that they're
  still testable without needing so much time to run!

I'll also explain *why* you want a fast test suite:

- Developers can write more tests, or iterate on the code more often, in the
same amount of time, which leads to better quality code.

- If you wait for tests to go green before deploying, deployments get faster.

- Developers are more likely to run a fast test suite locally.

- Fixing problems becomes easier.

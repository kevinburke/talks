# How Build and Test Caching Work

### Elevator pitch

Go's build and test caching can speed up compiling, testing and deploying your programs. In this talk you'll learn how Go's build and test caching work, how to make your builds and tests hit the cache more often, and how to write your own caches using the same principles.

### Description

Caching involves saving and reusing the result of an expensive computation, so you don't have to perform it again. The devil is in the details - figuring out whether a cache is still valid, how to store the cached results, and how to retrieve them out again.

We'll talk about the history of build and test caching, and how Go builds on both simple and complex previous approaches. We'll talk about the power of checksums as a way to concisely express a the result of a complex computation, and know instantly whether any of the inputs changed, and how you can apply this approach to other areas of programming.

We'll look specifically at how Go implements caching. How does Go compute inputs to a cached result? How does Go determine whether a cached test run can be reused? How does Go store cached data?

Finally, we'll look at how you can use Go's cache more effectively, by ensuring that your build and test runs are cached, and pointing out pitfalls that could lead Go to incorrectly cache a successful test that should fail. You'll learn how to figure out why Go _can't_ cache a particular test, which often is an indication of brittleness in test design, and how to write more robust (and cacheable) tests.

### Notes

When I learned that Go was implementing build caching, I started using Bazel on my projects, to get a better sense for Go's build caching would work. I've been hired for several jobs with the explicit goal of profiling and improving the performance of build and test systems, and I've had pretty successful results, for example lowering the time of a Ruby on Rails test suite at a newly IPO'd company from 40 minutes to 18. I'm also a contributor to the Go programming language and the golang.org/x/build repository.

I'm deeply passionate about performance and saving people time and would love the opportunity to teach Go programmers how to get the biggest time savings out of Go's cache design. I also hope to inspire them to write better caches in their own programs, by discussing some of the things that make Go's cache successful.

If you don't think this is appropriate for the main stage, I hope you would consider it for a tutorial session. On the main stage, I would spend less time discussing the internals of the caching implementation, and more on the design and the concepts that are applicable to everyone.

Here's an outline of the talk:

Caching is a pretty simple concept to explain:

<pre><code>cachedResult := resultHasBeenComputedBefore(params)
if (cachedResult) {
    return cachedResult
}
return doExpensiveThing(params)
</code></pre>

The devil is in the details:

- How do you look up cached results?
- How do you store the cached result?
- How do you decide you need to throw away/invalidate the cache?

We'll start by talking about two predecessors that influenced Go's cache design - Makefiles and Bazel/Blaze. They represent two different poles of the caching discussion - Make is extremely easy to set up, but caching based on filenames might not capture all inputs, and using file modification times might result in incorrect results. Bazel offers ultimate control over what gets cached, and the ultimate in cache-based speedups, but takes lots of time and effort for (non-Googlers) to configure correctly.

The Go build and cache is designed to avoid *any* manual configuration - it should "just work." Go uses a sha256 *checksum* to determine whether the inputs have changed, and thus whether a result needs to be recomputed.

- How checksums work, briefly

- Why they're better than modification times

- Checksums in other contexts (verifying downloaded files, cache busting for e.g. CSS)

What do you want to use as the input to the checksum? The raw file isn't great - if I change the whitespace, I'll still get the same result when I compile, but I've defeated the cache. Instead, parse the package into Go tokens, and then take a checksum of the tokens.

That's great for individual packages, but how about the whole program? Explain how Go computes checksums for an entire binary.

What if Go decides to compile packages in a different order? if A, B, and C can be compiled in any order, but they are compiled as B-A-C one time and C-B-A another time, the cache should still be smart enough to figure out that it has a match. Generally speaking, your input should be *deterministic.* Go manages this by sorting inputs before passing them to the checksum algorithm, so you always get the same hash for the same set of packages.

What about other inputs? If you build with a different version of Go, or on a different operating system, Go can't use the result. Walk through some of the other cache-breaking inputs quickly:

- OS

- Go version

- Environment variables

- Others

Explain briefly how Go stores cache _outputs_ - the naming scheme and file formats.

Explain how to leverage the Go cache on a build or test server. At the most basic level, your build server should _save_ the Go cache directory after a build, and restore a previous cache directory (if one exists) before builds/tests run.

Testing pitfalls - In some cases, Go can cache a successful test run, even though conditions have changed to the point where the test should fail. Tests that use "random" data might get cached, even though they actually should fail. For example, a test that the stock market is open might pass during business hours Monday-Friday but fail Saturday, and you might not notice it if Friday's test result was cached.

Another example is a test that generates a random day of the year, but fails if February 29 is chosen, or if the same day is chosen twice in a row. Be wary of values that depend on the current time or "randomly" fluctuate between a small (<1000) series of values.

Sometimes Go can't cache your tests. Why? The most common case is when something is read or written from the (non-temporary, non Go controlled) filesystem. We'll walk through some other common no-cache cases, and using GODEBUG=GOCACHEHASH=1 along with the `diff` tool to figure out why Go is failing to cache a particular test.

# Write Fast Tests with Sails? Don't Load Sails

* Name      : Kevin Burke
* Location  : San Francisco, CA, USA
* Email     : kev@inburke.com
* Twitter   : [@derivativeburke](https://twitter.com/derivativeburke)
* GitHub    : [kevinburke](https://github.com/kevinburke)
* Url       : [kev.inburke.com](https://kev.inburke.com)

## The story you'd like to tell

Sails.js is a very popular JS framework, but it takes about five to ten seconds
to load your app during testing. If you're trying to let your tests inform the
design and correctness of your running code, this means you only get a few
feedback cycles per minute, which can be really frustrating, and take you out
of the "flow" you need to write great applications.

<img src="http://wac.450f.edgecastcdn.net/80450F/thefw.com/files/2012/10/breadishardtoo.gif" />

#### There's a better way

It turns out that you can have your cake and eat it, too - in many cases,
you can test the logic of your application separately from your Sails app,
leading to faster tests *and* a better design. We'll discuss some strategies
for writing your app and splitting your code into small, testable pieces,
drawing from Gary Bernhardt's [Destroy All Software][das] videos and other
sources.

Isolated testing only goes so far - sometimes you will have to bite the bullet
and load Sails. There are some ways to make this less painful, and avoid extra
work where you don't need to do it. I will discuss some strategies for making
your test framework fast, and making you a faster test runner.

No matter whether you use Sails or not, you'll come out of this talk with a
better understanding of organizing your code for testability, how to benchmark
test runtime and how to get faster feedback when writing and running your
tests.

<img src="http://i.gyazo.com/6471248496685939e891813760b042bb.gif" />

[das]: https://www.destroyallsoftware.com/screencasts

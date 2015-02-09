# Chase Five Nines: Make Your Javascript Microservices More Reliable

* Name      : Kevin Burke
* Location  : San Francisco, CA, USA
* Email     : kev@inburke.com
* Twitter   : [@ekrubnivek](https://twitter.com/ekrubnivek)
* GitHub    : [kevinburke](https://github.com/kevinburke)
* Url       : [kev.inburke.com](https://kev.inburke.com)

## The story you'd like to tell

Splitting a monolithic application into smaller services can have many
benefits. However, a single API request will trigger many internal HTTP
requests, providing more opportunities for network failure, and more
opportunities for a slow server to gunk up the works for everyone. Even with
non-blocking IO, you can run out of resources in ways that affect your users.

(Mobile devices making requests have the same story. People will use your app
on the bus, in tunnels, and on the freeway going 80 miles per hour, and if your
HTTP client isn't configured properly, your users will get an endless spinning
wheel and a bad experience).

In this talk, we'll look at the many different ways that your clients and
services will fail in the cloud, some stability patterns you can deploy in
clients and servers to protect against failure, and some tools for testing
network issues.

We will walk through the lifecycle of an HTTP request, learning about failure
modes you'll see once a day, and failure modes you'll see once a year, but can
take down your server for hours at a time. I'll show you where the default
configurations of some common networking tools can get you into trouble.
For example, how can a request with a 30 second timeout take 10 minutes to
complete?

Once we've examined failure modes, we'll look at stability patterns, weapons
you can deploy in clients and API's to harden them against instability.
Specifically, we'll look at some code you can implement with the `request`
library to make more reliable HTTP requests, and some stability server patterns
using the `express` library.

Finally, we'll look at some tools you can use to simulate bad clients and
servers in your cluster, and catch errors before your users do.

## Speaker Bio

![](https://kev.inburke.com/photos/squarephoto.png)

Kevin is a developer at Shyp, where he tries to write software that won't
make people mad. Kevin once inadvertently left Waiting for Godot during the
intermission.

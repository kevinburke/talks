# Chase Five Nines: Make Your Microservices More Reliable

## Description

Microservices have many benefits, but unless you are careful, one slow service or one bad-acting client can take down your fleet.

In this talk, we'll look at the many different ways that your clients and services will fail in the cloud, some stability patterns you can deploy in clients and servers to protect against failure, and some tools for testing network issues.

## Abstract

Splitting a monolithic application into smaller services can have many benefits. However, it can mean that a single API request can trigger many internal HTTP requests, providing more opportunities for network failure, and more opportunities for a slow server to gunk up the works for everyone.

We will walk through the lifecycle of an HTTP request, learning about failure modes you'll see once a day, and failure modes you'll see once a year, but can take down your server for hours at a time. I'll also show you where the default configurations of some common networking tools can get you into trouble. For example, how can a request with a 30 second timeout take 10 minutes to complete?

Once we've examined failure modes, we'll look at stability patterns, weapons you can deploy in clients and API's to harden them against instability.

Finally, we'll look at some tools you can use to simulate bad clients and servers in your cluster, and catch errors before your users do.

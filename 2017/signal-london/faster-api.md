# An API Client that's Faster than the API

## Abstract

Logrole is a Twilio log viewer that can let you browse your calls and messages
much faster than the dashboard or the API. How? By being smart about users
browsing patterns, and efficient in storing and querying for information. Learn
how to use these techniques in your own apps!

## Description

[Logrole](https://github.com/saintpete/logrole) is a Twilio log viewer that
supports a few neat features, such as to-the-minute resource filtering, per-user
timezone settings, and customizable permissions for each end user, so the admin
can hide access to SMS message bodies for example.

Most interestingly from a performance perspective, the average page returns
responses in under 100 milliseconds, even though the Twilio API takes a second
or more to return resources in response to common queries. We do this by being
smart about predicting which pages users are likely to browse to, and then
prefetching those pages and caching them in memory. Caching can take up a lot
of memory on the server so we'll discuss techniques for compressing cache items
and evicting unnecessary items. We'll also discuss techniques for prefetching
resources in languages that do and don't allow you to fork execution in the
response handler.

We'll look at some other techniques for being efficient with resources. First,
what happens if two clients want to read the same data from the server (or
the database) around the same time? It can be wasteful (and slow) to make two
separate queries. I'll show a tool that lets the second query piggyback on the
first one, and get the same result back. We'll also look at a tool for canceling
in-progress work (HTTP requests, database queries) when the server decides it's
not necessary anymore.

Finally we'll look at some tips for ensuring high performance in a web service,
from making sure that everyone knows when the site just got slower, to avoiding
slow things in the first place.

Logrole is written in Go, and the libraries and tools we'll discuss will be
Go libraries and Go techniques; Go is a great language for building high
performance, low memory web apps and I'm excited to share what makes it great. I
will take some time to discuss how to implement these changes (and find similar
libraries) in other languages as well.

## Notes

I worked on the API team at Twilio for two and a half years, and I am a
frequent contributor to the Go Programming Language and associated development
tools. Previously I've contributed UI fixes to Jenkins, built a command line
client for CircleCI, and participated in reworking Twilio's API documentation
and helper libraries to make them more user friendly. I also maintain the
[twilio-go](https://github.com/kevinburke/twilio-go) helper library.

I haven't given this talk before, but I'm really excited to give it and share
what makes the tool so great, and makes Go such a great language for high
performance, efficient web development.

## Additional Information

I've spoken at Twiliocon/Twilio Signal three times now. The most recent talk is here: https://www.youtube.com/watch?v=KUBYTcVjp7I. It's the 5th most popular video on Twilio's account and the most popular talk by view count.

I've given this talk previously at Write the Docs, where it was one of the highest rated talks of the weekend. https://www.youtube.com/watch?v=sQP_hUNCrcE

Here are some links to technical blog posts I've written:

- https://kev.inburke.com/kevin/faster-correct-database-queries/
- https://www.twilio.com/engineering/2013/10/16/haproxy
- https://medium.com/shyp-engineering/speeding-up-javascript-test-time-1000x-460c528418e7

I am not underrepresented.

I own a one-person consulting company, so my flights, hotel and meals (from San Francisco) would be coming from my own pocket; anything you are willing to pitch in to help would be great.

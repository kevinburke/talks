Pycon Application
# Making Reliable HTTP Requests with Python

### Description (1 Paragraph, max 400 characters)

HTTP requests can fail in surprising ways, and cause your own app to fail or become slow. We'll discuss mitigation strategies that can prevent your app from falling over when your HTTP requests fail. We'll also look at the urllib3 class that makes it easy to write reliable, beautiful networking code.

(302 chars)

### Audience

Anyone who writes Python code that has to talk to another computer over a network, for example, retrieve data from the Twitter API, send emails using Amazon/Sendgrid/Mailgun, or make a request to another server in your network.

### Python Level

Attendees should have a basic familiarity with `pip` and Python syntax. Knowledge of HTTP is useful, but not necessary

### Objectives

Attendees will learn a lot more about HTTP, how unreliable networks can be, and the best practices for handling network failures, from Netflix and Twilio. Attendees will also learn about idempotence, which is a useful strategy for designing HTTP interactions, as well as a useful tool for writing software (design interactions to be retried).

### Detailed Abstract

HTTP requests can fail in surprising ways. Networks can betray you; the server may be unreachable, slow, or barfing errors. If you don't isolate HTTP request failures, your app can fail or become slow.

In this talk we'll learn more about the HTTP request/response cycle and the ways that requests can fail, from DNS lookup errors to 503 Service Unavailable responses. We'll also discuss mitigation strategies that can prevent your own app from falling over when your HTTP requests fail, helping you push your network toward five nines of reliability.

Writing all this code in Python can cause a try/catch spaghetti nightmare. We'll look at the urllib3 Retry class that makes it insanely easy to write beautiful, high-reliability networking code.

### Outline

### How many ways can HTTP requests fail? For each topic mentioned below, discuss the likelihood of failure & whether it is safe to retry (11 minutes)

*   DNS lookup failure - pesky to catch/timeout in Python due to reliance on system call. Increasing TTL can decrease DNS lookups. Alternatively have a service that returns connection IP addresses (haproxy, ZK, custom). This is safe to retry
*   Connection failure - Remote server might be unreachable. By default, most applications will try to connect forever & your network code will block. Set a connection timeout to avoid perma-blocking. You can retry this request.
*   (Brief discussion of timeouts - they are not the wall clock time, but the time between bytes sent by the server.)
*   Dropped packet - A packet or two may get lost in transit. Fortunately dropped packets are handled by the TCP protocol (so we don't have to worry), but your application might wait 3 seconds by default before re-sending a packet.
*   Slow server - The remote server might be overloaded and serving your requests slowly.  Use a timeout to avoid waiting for the 3rd party server forever. **Idempotent requests** - generally those using GET, PUT, and DELETE - can be safely retried (this sometimes works if only one server is overloaded). Consider using a backoff between requests (sleep for 1, 2, 4, 8 seconds...) to avoid adding pressure to an overloaded server.
*   Erroring server - The server may provide a bad response like "500 Server Error". Sometimes your request succeeded, and sometimes it didn't; this can be tricky to determine. Generally, consider retrying idempotent requests, 429 Too Many Requests and 503 Service Unavailable (as these indicate your request was rejected up front), though these last two depend on the particular server implementation.
*   SSL/TLS-related failures - Can be harder to track because intermediate servers can't read the data. Invalid certificate, invalid host, handshake timeout are examples

### Designing HTTP services - How can you help avoid failures like the above? (3-4 minutes)

*   Design services for idempotence - create an ID and enforce consistency in the database, allowing more requests to be retried
*   Publish information about server-side timeouts - how long will clients wait to hear whether long requests succeed or fail?
*   Log everything - makes it much easier to chase down future failures. HAProxy is a useful intermediary, though note you'll have to configure HAProxy with the same settings as above
*   Benchmark performance and rate limit requests to avoid slow/overloaded servers. There is a very handy nginx module for this.

###  Writing reliable HTTP code in Python (10 minutes)

*   Introduce the [Retry class in urllib3][retry]
*   Walk through several examples of using the Retry class

    *   Setting timeouts
    *   Retrying everything
    *   Retrying only idempotent requests

*   Walk through how to test your code - you can use [dummyserver][dummy], httpbin, or [hamms](github.com/kevinburke/hamms) to simulate various types of request failures.

[dummy]: https://github.com/shazow/urllib3/blob/master/dummyserver/server.py

[retry]: https://urllib3.readthedocs.org/en/latest/helpers.html#module-urllib3.util.retry

### Questions (5 minutes)

### Additional Notes

I'm an expert in this area, having spent years debugging HTTP requests and ensuring high reliability at US telecom startup Twilio.

I know I would do a great job giving this talk for PyCon; I'm really passionate about teaching people how to write more defensive code, especially because networks are so tricky. I spend lots of time designing and practicing my talks to make sure I'm communicating clearly and providing a ton of value.

If you were curious to see me speak, here's a talk on a similar subject (designing client libraries) at Twiliocon in 2013: https://www.twilio.com/conference/2013/videos/designing-maintainable-production-ready-api-client-libraries

Would love to have the chance to speak at PyCon and teach the community more about HTTP!

Thanks,
Kevin

### Additional Requirements

none

### About the Author

Kevin Burke is a contributor to urllib3, the HTTP library underpinning Requests.

He was recently an API engineer at Twilio, where he was responsible for ensuring that hundreds of API calls per second were routed to the correct places without failure. Kevin also worked on ensuring the client libraries that hundreds of customers used could reliably and quickly reach Twilio's API.

# Making Reliable HTTP Requests with Python

### Description (1 Paragraph, max 400 characters)

### Audience

### Python Level

### Objectives

What will attendees get out of your talk? When they leave the room, what will they know that they didn't know before?

### Detailed Abstract

HTTP requests can fail in surprising ways. Networks can betray you; the server may be unreachable, slow, or barfing errors. If you don't isolate HTTP request failures, your app can fail, or become slow.

In this talk we'll learn more about the HTTP request/response cycle and the ways that requests can fail. We'll also discuss mitigation strategies that can prevent your own app from falling over when your HTTP requests fail, helping you push your network toward five nines of reliability.

Finally, writing all this code in Python can cause a try/catch spaghetti nightmare. We'll look at the urllib3 class that you can use to ensure HTTP reliability without littering your codebase with exception handling code.

### Outline

### Additional Notes

I designed/wrote most of the Retry feature for urllib3, contribute regularly to both [urllib3](https://github.com/shazow/urllib3/graphs/contributors) and [requests](https://github.com/kennethreitz/requests/graphs/contributors?from=2011-02-13&to=2014-10-02&type=a), and I spent years debugging HTTP requests and ensuring high reliability at US telecom startup Twilio.

I know I would do a great job giving this talk for PyCon; I'm really passionate about teaching people how to write more defensive code, especially because networks are so tricky. I spend lots of time designing and practicing my talks to make sure I'm communicating clearly and providing a ton of value.

If you were curious to see me speak, [here's a talk on a similar subject (designing client libraries) at Twiliocon in 2013](https://www.twilio.com/conference/2013/videos/designing-maintainable-production-ready-api-client-libraries).

Would love to have the chance to speak at PyCon and teach the community more about HTTP!

Thanks,
Kevin

### Additional Requirements

### About the Author

Kevin Burke is a contributor to urllib3, the HTTP library underpinning Requests.

He was recently an API engineer at Twilio, where he was responsible for ensuring that hundreds of API calls per second were routed to the correct places without failure. Kevin also worked on ensuring the client libraries that hundreds of customers used could reliably and quickly reach Twilio's API.

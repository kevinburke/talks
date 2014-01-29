Designing Maintainable, Production-Ready API Client Libraries

Client libraries that wrap your API can be very useful tools for your customers,
converting the HTTP nitty-gritty of your API into easy-to-use objects. However
there are several common problems you'll run into as a library author.

- Do changes to your API require O(n) updates to your client libraries?
- Do enterprise customers dismiss your client library as "not production-ready" and insist on writing their own?
- Your API has many wrinkles and endpoints. How do you ensure each client library can exercise the entire API?

Using real world examples from Twilio, we'll discuss how to:

- Design your library for maintainability, uniformity and simplicity
- Implement a test harness to ensure all of your libraries exercise your entire API
- Harden your HTTP client to be robust against networking and transient API errors.

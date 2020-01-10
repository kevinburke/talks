
Okay, you're not in that category. How do you introduce Go? Start by finding
a _bottleneck_ that's important to your company. A _bottleneck_ could be:

- Features take too long to write, test and deploy.

- It's hard to write code in the existing codebase without making mistakes.

- It takes too long for new engineers to get up to speed.

- Services use too much RAM or CPU in production.

- Latency/GC pauses are too high and/or you can't debug why that's happening.

(If you can't identify a bottleneck Go can help with, *don't propose using Go!*
Save your bullets.)

Once you've identified the bottleneck, pitch Go as the alternative *to help with
the bottleneck.* So if CPU and RAM are problems, show the blog posts about a Go
service lowering CPU and RAM. If debugging is the problem, talk about pprof.
Etc.

**Try to quantify it** - Back of the envelope numbers can help you make the
case! 100 servers cost X, it would take 2 engineers Y weeks at $Z per week to
reduce that number to 20 servers, so it would pay for itself in W months.

Now it's time to write the thing! Go listen to the other talks to learn how to
do that.

Okay, you wrote the thing. How do you deprecate the old thing? Let's walk
through some strategies for doing that.

- **Evangelize!** Make sure everyone at your company is aware of the new thing
and how cool it is. This is awkward because it involves **talking about your own
work.**

First we'll talk about situations where Go might *not* be a good fit. Many
projects flounder on a staging server or shortly after an engineering blog post
was written, or, worse, survive but don't fully replace the old service. Picking
your spots is extremely important. The incentives facing your company don't
match the incentives facing _you_, personally; a sign of a senior engineer is
someone mature enough to reject a new idea that they _personally_ would enjoy
writing, if it's not a good fit for the company.

Other scenarios where Go might not be a good fit:

- The language and tooling that you have is doing a good job!

- You have 14 languages and Go would be the 15th. (see Dan McKinley's blog post
about *innovation tokens* for more.)

- No one else on your team knows Go or is excited about Go.

If this describes your situation, maybe start by evangelizing for Go - hosting
tutorials or writing non-production tools - instead of trying to run services in
production. Or find a company that likes Go better!

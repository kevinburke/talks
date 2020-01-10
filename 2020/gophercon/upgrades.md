# Getting Your Company to Love Go

### Elevator pitch

_You have 300 characters to sell your talk. This is known as the "elevator
pitch". Make it as exciting and enticing as possible._

You know how to write a Go service. But how do you get your company onboard and deploy it to production? You'll learn how to pick a project, introduce Go, deprecate older projects, and drive Go adoption throughout your company.

### Description

This field supports Markdown. The description will be seen by reviewers during
the CFP process and may eventually be seen by the attendees of the event.

You should make the description of your talk as compelling and exciting as
possible. Remember, you're selling both the organizers of the events to select
your talk, as well as trying to convince attendees your talk is the one they
should see.

Knowing how to write production Go code is only half the battle. How do you get your company on board? A common mistake engineers make is to focus only on the technical case. The best plan counts for nothing if you can't convince other people it's a good idea. Engineers can feel uneasy acting as marketers or salesmen, but that's exactly what you need to do to get buy-in for your new service.

In this talk we'll walk step-by-step through all of the other stuff *besides* writing the Go thing. How do you identify a project that would be a good fit for Go? How do you get your managers and teammates onboard? Finally, how do you convince, motivate or mandate that people to ditch the old thing and start using your new thing?

You'll learn new strategies for approaching each step of this process, and in the end walk away as a more effective engineer.

### Notes

This field supports Markdown. Notes will only be seen by reviewers during
the CFP process. This is where you should explain things such as technical
requirements, why you're the best person to speak on this subject, etc...

I was an independent software consultant for three years. The success of my business depended on convincing companies to make changes I suggested, deploying those changes to production, and ensuring they didn't get reverted after the end of the engagement. I'm also a contributor to Go; I have "code review +2" permissions in Gerrit.

Attendees are going to learn a lot of really awesome stuff about Go at this conference. I want to make sure they can take it back with them and actually use it, which in many cases involves navigating company hierarchies and understanding the incentives facing other people in the organization. Hopefully the things attendees learn during this talk will help them better understand the business and company environment, and be more successful in their technical work because of it.

Here's a rough outline of the talk! Subject to change as I get more feedback from you and others in the community, develop slides, etc.

I know you want to write Go because you're here. And I know you want to be successful in your job because everyone wants to be successful. For 90% of you, to write more Go and be successful in your job you're going to need to get buy in from someone else at your company. Let's talk about how to do that.

Step one is to **identify a problem.**

- If your team has **performance goals** - sell more widgets, reduce costs, hire 30 people - those are a good place to start. What are the things that are making it hard to achieve those goals?

- You can also identify problems by **talking to people, especially managers.** Take them out to coffee and ask them what they're having trouble with - what things are important to them, and what's stopping them from achieving the things that are important to them.

- Finally **look at how you're spending your time.** Do you have to spend half of each sprint fixing errors found in the previous deployment? Do new engineers need a lot of mentoring because it's easy to make mistakes in your existing codebase? Are you waiting for hours for type checking or tests to finish?

Here are some examples of problems you could identify:

- Features take too long to write, test and deploy.

- Several different teams need to access or update the same data, but they don't
coordinate.

- It's hard to write code in the existing codebase without making mistakes.

- It takes too long for new engineers to get up to speed.

- Services use too much RAM or CPU in production.

- Latency/GC pauses are too high, and/or you can't debug why services are slow in
production.

- We can't tell who's accessing sensitive data which means employees can stalk, blackmail customers or worse.

Once you've identified the bottleneck, pitch Go as the alternative *to help with the bottleneck.* So if CPU and RAM are problems, show the blog posts about a Go service lowering CPU and RAM. If debugging is the problem, talk about pprof and benchmarking.

Other factors are going to be more "squishy" - claiming Go will reduce your error rate, or lead to faster onboarding. These are going to be tougher to make a concrete case for. But you can use some strategies:

- **Try to do a cheap experiment.** Can you make something small to demonstrate the concrete benefits?

- **Can you eliminate a whole class of errors?** It can be easier to make this case because you can tie it back to an existing condition. "We make a lot of mistakes that could be caught by static typing, or by accessing the same variable from two different threads. Go would make it impossible to make those mistakes."

    This doesn't need to be something that _only_ Go can do. Some languages
    make it tough to use database transactions, for example. Go isn't the only
    language that has database transactions but it makes them _really easy to
    use_, which can solve a lot of consistency problems. As long as it's solving
    a problem that's hurting the business it's fair game.

- **Can you get people excited about it?** You don't want your Go pitch to come out of nowhere. Share articles, write an internal tool in Go, spend hack day building something with Go, host tutorials for your fellow engineers.

- **Can you estimate?** Combined with the example above and a history of production errors, or features that failed QA, you could estimate the cost of each failure versus the amount of time it would take to rewrite and get a potential estimate of the benefit from using Go.

Even for things like recruiting, you could look at a StackOverflow survey, or estimate the share of candidates who say they dislike the languages you're currently using.

Now it's time to write the thing! Go listen to the other talks to learn how to do that.

Okay, you wrote the thing. How do you deprecate the old thing? Let's walk through some strategies for doing that.

- **Evangelize!** Make sure everyone at your company is aware of the new thing and how cool it is. This can be awkward for some people because it involves **talking about your own work.** If you don't talk about it, you are risking 1) the thing you built being a failure because no one uses it, or 2) someone else taking the credit. The thing you built is good! It well help your coworkers do their jobs better or faster and the more coworkers know about it, the more benefit it will do.

- **Write really good documentation** - Becoming a good writer is one of the most high leverage skills you can attain as an engineer. It's useful in so many different areas - writing about things you made, engineering blog posts, performance reviews, technical specs, reporting errors. Write good docs so people find your new thing pleasant to interact with.

- **Go team by team** - At (large IPO'd telecom company), we had a bad HAProxy configuration that every single team copied because they didn't understand HAProxy. I knew the HAProxy config was bad, but didn't know what each team needed in terms of reliability and consistency. The solution was to schedule meetings with every single team to talk about why the old thing was bad, and why the new thing I made was better. It was time consuming but the results were absolutely worth it.

    If you're struggling to get people to adopt the new thing, meeting team by team will *help you learn why people are not adopting the thing.* You can use this feedback to build a better thing. You are your own startup founder.

- **Become the middleman:** Introduce a library as a new layer in between the codebase and the thing you want to deprecate. Initially, the library just calls the deprecated thing. Over time you can replace the deprecated thing piece by piece, or send 1% of calls to your new service, etc.

- **Make the old thing painful to use:** The disadvantage of any new thing is that it's new and it takes energy to learn how to use it. Tilt the scales in favor of the new thing by making the older thing more painful. If it's a deprecated internal tool, print a warning and add a sleep() call that gets slightly longer each day. Add annoying logging when people use the old thing. Make a whitelist of call sites that are allowed to use the old thing, and don't allow new entries onto that list.

    The easiest way to sell this is by making the new thing faster and better - then everyone should want it! And hopefully since it's Go, it will be.

Before closing, a bit of a sobering note - Go might not be a good fit for every scenario, and it's important to pick your spots. Sometimes choices that would be good for your career might not be the best choices for the company. If the language and tooling you have are doing a good job, Go might not be beneficial on the whole, even if it's better in some respects. A sign of engineering maturity is recognizing the right tool for each job.

You are going to learn a lot of really awesome stuff about Go at this conference. I want to make sure you can not only take it back with you, but understand how to deploy it to make your company more successful.

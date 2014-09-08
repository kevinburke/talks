### Abstract

Why do we make errors while programming? How many tests are enough? How can you and your team catch more errors before you ship code to customers? We'll analyze decades of industry research into the sources of errors, and the effectiveness of tools like unit testing and code review. You'll learn techniques that help cut down on errors and make your team more effective.

### Audience

Engineers who strive to get better at writing error-free code; managers who want to help their teams move faster.

### Objectives

In this talk you'll learn:

- Can I actually decrease my error rate while I write code?
- Which error-finding tools (unit test, regression test, code review, formal inspection) are most effective at finding errors?
- What are formal inspections, and how can they help you write better code?
- How are errors distributed through my codebase? Which smells in my code (for example, unused variables) are correlated with errors?
- How expensive is it to fix defects in development vs. after they go to production?
- Is there any evidence that some programmers are 10x more efficient than others?

### Outline

**Note:** I don't have perfect timing data for this talk, though the last time
I gave it, I landed at 28 minutes.

Introduce myself, outline the talk

Talk Overview:

- How is research conducted into software issues?

- Is it possible for you to make fewer errors while building software?

- Where do errors come from/what do we know about them?

- How effective are common approaches (pair programming, unit testing, code
  review, etc) at finding bugs?

- How can I write better tests?

- How can I do better code reviews?

### Research Methodology

- generally performed at larger companies with critical software (NASA, Raytheon, IBM, Microsoft)
- Several methods to collect data: a) ask engineers how much time they spent last week on various activities, b) interview teams on whether they use tests, design inspections, code review etc., c) automated data collection from bug databases, build servers, etc.
- Some studies gave engineers a sample program with 20 bugs and asked "find as many as you can"
- Others asked engineers to build a 1-day-sized program to spec and measured the number of errors in the output

### Stop using the word "bug"!

It implies randomness and an external source (an insect) that made the error. Your team made the error. Your team can track errors and reduce them over time.

### You have room to improve

Present data showing how much of our time goes into finding, fixing mistakes

- Roughly 50% of time on a project is spent debugging, refactoring, reworking existing code. *This time can be cut down by writing better code the first time.* (Mills 1983, Boehm 1987, Cooper and Mullen 1993, Fishman 1996, Haley 1996)
- In the literature, there are 10x differences between programmers in size of a completed program, speed to complete a program, error rate, and error detection rate. This implies that everyone has room for improvement on a number of dimensions. (Evidence to support this: Sackman, Erickson, Grant (1986) found 10x differences in productivity between programmers at System Development Corporation, Curtis (1981) found 10x differences in times to detect errors in sample programs, Demarco and Lister (1985) found that programmers who completed a sample program fastest also made half as many errors as those who took three times as long. I have more)

### Where do errors come from?

- 18-36% of errors are clerical (spelling, syntax, etc)
- 3 most expensive errors of all time were changes of a single character
mistakes in a correct program
- Most errors are localized - 85% can be fixed in a single class or routine
  (Endres 1975)
- Most errors are your fault - don't blame hardware, the compiler, AWS, etc
- Changing requirements, communication breakdowns (Fred Brooks), thin
  domain knowledge all correlate with errors
- Unused variables, high numbers of comments, complex control flow correlate with errors
- 80% of errors come from 20% of code. 50% of errors from 5% of the code
- In general there are 1-25 errors per 1000 lines of code written

### How effective are different bug-finding tools?

- Data summarized from Jones 2000 and Shull 2002
- [See this chart for a breakdown][chart].
- Most effective is a high-volume beta test, eg if you don't find the bugs,
  your customers will almost certainly find them.
- Unit testing / regression testing is not very effective at catching
  errors
- Design inspections and formal inspections are more effective

- Tools don't overlap; different techniques find different types of errors.
  Use more than 1 technique to increase effectiveness

### What is an inspection, and why are they better than testing?

- Michael Fagan, IBM, 1976. In an inspection you focus on finding errors
  (no solutions), everyone has reviewed the code already, a moderator
  walks through code and author defends the work. Management is not
  present.

- In multiple studies, inspections are more effective than testing at
  finding errors, and also find more errors than tests (Basili & Selby
  1987, Russell 1991, Kaplan 1995, Moore 1992)

- **Why doesn't testing catch everything?** It's hard to catch unclear
  error messages, duplicate code, hard coded variable values, magic
  numbers, inadequate comments, changing requirements

### If you do test:

- Automate your test procedure, 50% of tests run manually were run
  incorrectly in one study
- Double check test code for errors, test code is more error prone than
  regular code
- Make errors extremely hard to miss (e.g. with sys.exit)

### Review small changes. They are extremely dangerous

- In one company 55% of one line changes were incorrect, before code review
  was mandated
- Most likely to make errors with 5 line changes (reviewers don't pay as
  close attention, perhaps, or these changes aren't reviewed)

### How expensive is it to fix errors?

After deployment, 10-100x as expensive to fix architecture changes as before
you deployed (Fagan 1976, Dunn 1984, Shull 2002). Fixing things as soon as
possible keeps software projects on time.

### Summary

- Use multiple techniques for finding bugs

- Assume you and your team can get better at preventing errors

- Code review everything (especially 1-5 line changes)

- Consider formal code/design inspections

Review, and then time for questions.

[chart]: https://kev.inburke.com/slides/errors/#different-types-of-review


### Additional Notes

I gave this talk internally at Twilio, then at the Go SF meetup, and was approached by multiple people after the meetup who wanted me to give it privately to the engineers at their company. I think the content of this talk resonates with a lot of people who want to know how to best spend their time and what they can do to prevent errors.

I also submitted this talk to the SF Python Meetup Group, hopefully it'll get picked for an upcoming meetup so I can practice some more.

[The slides from that talk are online here][slides]. I am worried about being able to present the data at "ballroom distance," so if it gets accepted, I'm going to clean up *all* of the charts present (and probably add more) using Flot or similar, so they're appropriate and readable at Pycon conference room distances.

[slides]: https://kev.inburke.com/slides/errors/

I'm also going to look at more recent studies; a lot of the data comes from Code Complete, which was written in 2004, so there's a lot of new material to summarize.

### Background

Until very recently I was an API engineer at Twilio, responsible for scaling the API and reviewing all code related to it.

I have previously given talks at Railsconf 2013, Twiliocon, and the API Strategy Conference, as well as many times internally at Twilio. [Here is a video of me giving a talk on client libraries at Twiliocon][twiliocon].

[twiliocon]: https://www.youtube.com/watch?v=C_UJHqR_2Mo

I would love to speak at Pycon - everyone struggles with building error-free software and the best way to spend engineering resources and there's a ton of value in here for people who are curious about it.

I spent most of this week in the library reading papers, requesting more resources, working on this talk, and would love to share with everyone at Pycon what I've learned about how to build better software.

# Becoming a Go Contributor

### Abstract

Contributing to the Go project and associated tools can look really
intimidating. I'll share how I went from being a part time Go hacker to a
contributor, and discuss how you can get started contributing to Go!

### Description

For each of these points I'll try to tell a story about a change I made, or
failed to make.

- You *are* smart enough! As Woody Allen said, 80% of life is showing up. Most
of the people who are big contributors to Go got their start in very small ways.
But they show up over and over again: reviewing patches, suggesting new features
on the mailing list, responding to bug reports. You see the big, brilliant
patch, but you don't see the decades spent reading the code, responding to
bug reports, submitting small changes. Doing those things will give you more
experience. The main thing separating you from the people you see are they show
up over and over and they have for a long time.

- It is okay (recommended) to start really, really small. There is a significant
hump you have to get over when you start: figuring out how to submit changes,
and building trust/your reputation among reviewers. Of the first 15 changes that
I submitted, 13 were fixing typos and adding documentation/examples. One of the
other two was applying a patch submitted by someone else.

- Everyone thinks "contributing" is submitting changes to the standard library,
but submitting changes to the standard library is very difficult. It's already
pretty good at the things it is trying to do. Consider starting by contributing
to golang.org/x repos. One way I contribute is by fixing tests when they break.
Changes to the standard library usually require 10-100x more communication and
organization than actual coding.

- Corollary: Helping respond to issues, and figure out what direction the
project should move in, is really valuable. Doing things like responding to
error reports, code reviewing what you can, and adding more documentation, are
really valuable!

- You are going to have false starts. It's okay! Not every change works out, and
sometimes you'll want to do something and won't know how to do it. Ask on the
mailing list, and when you do learn something, try to document how to fix it.

- Notice when you are confused, when something is wrong, or when something is
harder than it should have been. A lot of my best contributions have come from
this: here's a cool thing, why isn't there an example for it? Why does copying
the source code from godoc also copy the line numbers? Being willing to dig in a
little bit can go a long way.

- "Success", as a contributor, is coming off the bench and playing 8 minutes
and maybe scoring four points and playing solid defense. You are not Brad
Fitzpatrick, not least because he has been coding for decades, he is paid to
contribute to Go, he/Russ Cox etc get more time to work on it, and you don't.
That is just to say: It's tremendously difficult to contribute code and no one
is expecting you to submit brilliant changes. There are lots of ways to make a
positive impact on the project and even though a lot of them appear small, all
of them deserve to be celebrated.

### Notes

I'm a Go contributor (not working at Google); I've made over 100 commits to
the Go project in the last year. I'm not paid to contribute to Go and I didn't
really know anyone on the team before getting started, so I can talk about
getting more involved with contributing and eventually helping build out
features.

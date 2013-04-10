# how to write documentation for users that don't read

why do you have documentation? why bother?

1. help people figure out how to install your tool
2. help people figure out how to use it
3. see what options are available
4. summarize how it works

can abstract these into a few goals for people

1. I want to install the tool
2. I want to use the tool
3. I want to do more with the tool
4. The tool is not working in a way I expected and I need to fix it.

OKay so now that we have these, what are the consequences of bad documentation

- people can't install your product, and use something else
- people can't use it - they use something else, or give up on computers
- people don't know what they can do with your product
- it breaks and people don't know why

for documentation to solve your users problems, need to assume a bunch of
things:

1. people will know about it, or be able to find it
2. people will read it
3. people will find what they are looking for in your documentation
4. you have documentation for the problem the user is trying to solve
5. your documentation actually solves the users problem (Windows users)

## do people find your documentation?

what percentage of your users actually make it to your documentation? i know
for myself, i'd much rather try to just get something done and go figure out
why it broke later. (determine the statistic here).

twilio example: github homepages

## how do they find it?

for us, 50% come from google. other competing sources: ehow, stack overflow,
etc. 

Law of the web: people spend most of their time on other sites.

## will people read it? 

well, how do users read on the web? answer: **no**

Jakob Nielsen, "How Users Read on the Web"

- people scan for the information they want

- if they can't find it, they leave, or try google again

- people are terrible searchers

## will people be able to find what they are looking for?

disconnect between the way documentation is written and the way it's consumed.

- writing docs: highly focused on one thing, assume linear progression down the
page

- reading: highly distracted, watching reruns of 30 rock on a different screen,
scanning for the answer to their specific question.

### results from eyetracking studies

Jakob Nielsen, "F-Shaped Pattern for Reading Web Content"

what do they look at?

- first two paragraphs - must contain important information

- headings

- bold words

- first three words of a paragraph

- bulleted lists

#### what don't they look at?

- things that look like ads. example from twilio: "Next Page" link in
  quickstarts, banner blindness for Twilio Client

- images

- fat paragraphs - nebraska example from How Users Read

- us census bureau example - find the population of the united states. 86%
  missed the giant red text.

http://media.nngroup.com/media/editor/alertbox/census-homepage-small.gif

## practical implications

1. Users are coming from Google. SEO matters.

- know basic SEO - url's, titles, one h1, meta description, rich structure etc.
- angular JS - whole page is a client side app, you can't find anything
- you might get outranked by demand media

2. Users are busy.

Change your mental model of your user. Your user is not trying to get something
done with **your** product. They are trying to get something done with **their
own** product. Twilio example:

- I'm trying to verify people are actually people by sending them an SMS
- I'm trying to send texts to my sister to remind her to take her meds on
  a schedule

**Your** product is ancillary.

3. Consider using your product as documentation

- Optimize the hell out of your error messages, so people know what went wrong
  and how to fix it. Think about error messages as your users journey through
  your product.

    - Problem: Some error messages you can't control. Bash's error messages are shit.

- Don't let users make mistakes. Example - Shipping the SSL cert with the
  library, using a custom install script



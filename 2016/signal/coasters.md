## Title

Hacking Roller Coaster Tycoon with Genetic Algorithms and Go

## Abstract

Sure, you're good at designing coasters in Roller Coaster Tycoon. But you could make even cooler coasters if you let Go build them for you.

We'll look a little at RCT code, which is written in x86, and how to reverse engineer it. I'll share how I found various pieces of important information in the binary, and discuss strategies for finding information in a large binary you don't know much about.

We'll learn how to implement algorithms that genetically evolve cooler and cooler coasters. We'll discuss the types of problems genetic algorithms are good at solving, walk through a simple genetic algorithm, and discuss ways to evaluate the fitness of a given mutation.

Finally we'll discuss why Go is a perfect fit for interfacing with RCT and writing a coaster generator.

## Bio

A Bay Area native, Kevin Burke gets mad at computers so you won't have to.
He wrote hamms, a misbehaving HTTP test server, and doony, the popular theme
for Jenkins, and contributes to the popular Python http libraries urllib3 and
requests. Kevin unsuccessfully tried to ban Powerpoint at his high school, and
made Techmeme for pointing out that 7 million Virgin Mobile passwords could be
brute forced. Currently he builds great experiences at Shyp; prior to that, he
worked on the API, documentation, and client libraries at Twilio.

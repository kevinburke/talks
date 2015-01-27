# Hacking Roller Coaster Tycoon with Genetic Algorithms and Go

## Abstract

Sure, you're good at designing coasters in Roller Coaster Tycoon. But you could make even cooler coasters if you let Go build them for you. We'll learn how to implement algorithms that genetically evolve cooler and cooler coasters. We'll look a little at RCT code, which is written in x86, and how to reverse engineer it. Finally we'll discuss why Go is a perfect fit for interfacing with RCT and writing a coaster generator.

## Outline

The impetus: I was 12 and wanted to build the perfect coaster in Roller Coaster Tycoon. And now at 26, I have the tools to write the code to do it. So I did.

The talk will cover various parts of the project.

### Reading and writing ride data

Ride data is stored in a custom data format where each byte means something different - one byte controls the ride color, one controls the entrance location, etc, etc. This data is then encoded using a custom ride length encoding. The first step of the project was to write Go encoders and decoders to serialize saved rides into Go structs, and vice versa.

### Getting info about track pieces

There are about 120 different track pieces - sharp turns, corkscrews, straightaways, and more. Instead of encoding this information by hand, I found where it was located in the ~600,000 lines of x86 code making up the game, and read these into Go structs. You'll learn how to read basic x86 and how to parse this data with Go.

### Viewing rides

It's hard to look at a sequence of track pieces and determine whether it's a cool roller coaster or not. I used the Go `image` library to create crude drawings of a given coaster design, without needing to load it in-game.

### Generating a fitness function

To evolve coasters, you need a way to determine whether a given coaster is better than another. At the beginning, you can evaluate coasters based on how close they are to forming a complete loop, and not colliding with themselves. Then you can use the game's excitement, intensity and nausea ratings (which required another dive into the x86 codebase).

### Evolving coasters to find a good one

We'll discuss some strategies for mutating roller coasters, how many coasters to choose, in the context of helping you find the right genetic algorithm to solve your own problem.

### Conclusion

We'll look at some pictures of the finished coasters, and reflect on how Go is perfect for this task:

- reading byte data from an exe
- allowing you to write custom encoding/decoding functions/enforce types on the
  decoded data
- drawing pictures of coasters
- having great performance when asked to generate a large number of mutated
  coasters across multiple rounds, detect track collisions, and more.

# Pitch

Building great roller coasters is many a child's dream, and this talk is going to be a really fun look at how to do that for a game that many people grew up playing. The subject material is pretty accessible, so it will be easier to communicate the basics of genetic algorithm design and implementation.

I'll highlight many different areas of the standard library, shed some light on RCT's internals, teach people a little about reverse engineering, and show off some of the coasters I made! I am qualified because I built the project and I've built some pretty sweet parks in Roller Coaster Tycoon as well.

There's an introductory blog post available by googling " genetic algorithms roller coaster tycoon".

I've made more progress on the project since then, and I will continue to do so leading up to Gophercon.

I've given several public talks in the past, though linking to them would give away my identity. I'm hungry to give a talk to this audience and really knock this talk out of the park. If you pick me, please know I will put an incredible amount of preparation into this, including lots of extra work on the project, several drafts and workshopping rounds in front of friends and other Go hackers.

If you go to https://github.com/my_username/talks/blob/master/videos.md, you can see examples of my previous talks, though these links will give away my identity.

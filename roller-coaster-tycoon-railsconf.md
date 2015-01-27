# Hacking Roller Coaster Tycoon with Genetic Algorithms

## Abstract

Sure, you're good at designing coasters in Roller Coaster Tycoon. But you could make even cooler coasters if you let a computer build them for you. We'll learn how to implement algorithms that genetically evolve cooler and cooler coasters. We'll look a little at RCT code, which is written in x86, and how to reverse engineer it. Finally we'll talk about strategies for tackling large, wide-ranging projects like these and ensuring implementation success.

## Outline

The impetus: I was 12 and wanted to build the perfect coaster in Roller Coaster Tycoon. And now at 26, I have the tools to write the code to do it. So I did.

The talk will cover various parts of the project.

### Reading and writing ride data

Ride data is stored in Roller Coaster Tycoon in a custom data format where each byte means something different - one byte controls the ride color, one controls the entrance location, etc, etc. This data is then encoded using a custom ride length encoding. The first step of the project was to write encoders and decoders to serialize saved rides into structs in memory, and vice versa. Some of this is documented but a lot of it involved deciphering the x86 code to figure out all of the variables associated with a Ride object.

### Getting info about track pieces

There are about 120 different track pieces - sharp turns, corkscrews, straightaways, and more. Instead of encoding this information by hand, I found where it was located in the ~600,000 lines of x86 code making up the game, and read these directly from the binary into memory. You'll learn how to read basic x86 and how to read data directly from the binary.

### Viewing rides

It's hard to look at a sequence of track pieces and determine whether it's a cool roller coaster or not. I used the Go `image` library to create crude drawings of a given coaster design, without needing to load it in-game. You will learn techniques for taking a sequence of 3D points and turning them into an isomorphic picture of a track, as well as meta-information about how to tackle tasks like these where you might not know how to do it right away.

### Generating a fitness function

To evolve coasters, you need a way to determine whether a given coaster is better than another. At the beginning, you can evaluate coasters based on how close they are to forming a complete loop, and not colliding with themselves. Then you can use the game's excitement, intensity and nausea ratings (which required another dive into the x86 codebase).

### Evolving coasters to find a good one

Genetic algorithms can be tricky - unfortunately it's not easy enough to just add random mutations and hope for the best - the simulation may take forever to evolve a likely strategy, or you may reach a local maxima and never find your way out. You need to choose good mutation strategies, and experiment with the number of different mutations to try in each cycle.

### Conclusion

We'll look at some pictures of the finished coasters, and reflect on how you can tackle tasks such as:

- reading byte data from an exe
- write custom encoding/decoding functions and enforce types on the decoded data
- drawing pictures of coasters
- quickly iterating on proposed algorithms/coaster designs.

## Pitch

Building great roller coasters is many a child's dream, and this talk is going to be a really fun look at how to do that for a game that many people grew up playing. The subject material is pretty accessible, so it will be easier to communicate the basics of genetic algorithm design and implementation.

I'll shed some light on RCT's internals, teach people a little about reverse
engineering, and show off some of the coasters I made! I am qualified because
I built the project and I've built some pretty sweet parks in Roller Coaster
Tycoon as well.

There's an introductory blog post available by googling "genetic algorithms roller coaster tycoon", though doing so will reveal my name.

I've made more progress on the project since then, and I will continue to do so leading up to Railsconf.

I've given several public talks in the past, including at a previous Railsconf, though linking to them would give away my identity. I'm hungry to give a talk to this audience and really knock this talk out of the park. If you pick me, please know I will put an incredible amount of preparation into this, including lots of extra work on the project, several drafts and workshopping rounds in front of friends and other Ruby hackers.

If you go to https://github.com/my_username/talks/blob/master/videos.md, you can see examples of my previous talks, though these links will give away my identity.

### A short disclaimer

The ride encoding, image parsing code and algorithms were written in Go.
However, the code is not doing anything incredibly special with types or data
structures, and all of the code/algorithms are straightforward to port to Ruby
for showing on slides. If I am selected, I will do this for the talk.

(I am also happy to discuss why I chose Go for this project, and show examples
of things like type validation/serialization, if the committee thinks it would
be more interesting).

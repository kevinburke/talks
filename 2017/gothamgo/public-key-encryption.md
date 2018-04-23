# <title>

## Abstract (300 character elevator pitch)

For centuries, encrypted messages relied on a shared password between the two
parties. Public key cryptography changed everything - you could receive messages
without ever needing to share your secret with anyone. It is so magical and
powerful the US banned its export throughout the 90's. How does it work? We'll
walk it together in this talk.

## Description

We'll start with a short history of cryptography in the 1900's.

- What is cryptography? Obscuring a message that may be intercepted in
transport. Always assume an attacker can read/steal the message in transport.

- Rot13 - introduce the concept

- Enigma: Reversible cryptography. Relied on sender, receiver knowing the same
  wheel settings.

- One time pads: Unbreakable, but expensive. Required sender, receiver to have
  same pads. Could only be used once.

Then **public key cryptography** happened! You generate a public key and
a private key, and share the public key. People can encrypt messages using your
public key, and the only way to decrypt them is with the private key. This is
**revolutionary** - you don't need to share your private key with anyone, or do
any coordination ahead of time.

History of public key crypto - out of WWII research, expanded on/patented by
RSA. US banned export in the 1990's, as it made messages impossible to intercept

Some metaphors for the process, before the math, and just to prove that it's
possible to send a message without ever sharing a secret key.

- You want to send a message to a friend, but you can't ever meet to deliver it.
  You put the message in a box and put a lock on the box. Your friend arrives,
  later, and puts his lock on the box. You come back and remove your lock.
  Finally, your friend removes his lock and reads the message.



## Notes

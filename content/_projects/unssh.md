---
name: unssh
github: unssh
description: A toy sshd implementation that nobody should use.
layout: project
---

![Demo of unssh](/assets/screenshots/unssh-01.gif)

## About

(_unssh: an [uns]ecure [sh]ell_)

I built part of an SSH server. It's enough to get a shell on the host machine
using an OpenSSH client. This included implementing the binary protocol,
performing key exchange, hooking up encryption and decryption, session
management, PTY allocation, and launching shells or running commands.

### Obvious Disclaimer: Don't use this

It's got no authentication mechanisms, doesn't follow any standards for configuration,
and could have serious security vulnerabilities. This was nothing more than
a weekend project.

## Key Exchange

By far the hardest part was implementing the key exchange. I thought this would be
easier given that there are detailed spec documents and reference implementations,
but I underestimated how finicky it would be to get right and how hard it would be to
debug, since even a single bit wrong somewhere is as broken as not implementing
it at all.

The key exchange is implemented using the Diffie-Hellman Group 14 method, which
I don't actually understand, if I'm honest — again, I just followed the spec.

I actually found the spec wildly ambiguous and confusing in places. Things like:
 * Large integers used in the cryptography are sent over the wire in a specific
   encoding that wraps the underlying bytes, but it's not clear in the spec whether
   the raw bytes are used in the hashing algorithm, or the encoded bytes used in the
   transport protocol. Hint: it's the encoded bytes, for some reason.
 * If you finally get to a point where you can receive `SSH_MSG_CHANNEL_DATA` packets,
   congrats! The spec doesn't say anything about what this data is or how to use
   it. It seems logical to write the bytes to the PTY master file descriptor, and
   that turns out to be correct. But why the spec doesn't specify this is beyond me.
 * Both sides create their own identifiers for channels regardless of who created
   them, and most of the channel-related messages require this identifier to be set.
   It's labeled as "recipient channel" in the spec, so I assumed you should send the ID
   the other party assigned to the channel. I guessed correctly, but again, why the
   spec doesn't clarify this is beyond me.

## Conclusion

In the end, it kinda works. Please don't use it.

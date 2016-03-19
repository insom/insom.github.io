+++
date = "2016-03-10T23:02:30Z"
draft = false
title = "Z80 Microcomputer"
+++

I've wanted a little Z80 microcomputer of my own for ages. I never learned to
get the most out of a Z80 when it was appropriate (like, when I had a Spectrum
+2) and it seemed simple enough to build on my own. There's a smattering of
other Z80 self-builds on the Internet and [a fantastic free book about the
process][book].

I chose the Z80 because:

* It's close to the 8086 instruction set, which I've been exposed to
  on-and-off for years.
* It's still available for purchase.
* It can be clocked very slowly[^1], so I can use blinking lights and the
  electronics group's 10Mhz scope for debugging if I need to.
* IO is not memory mapped. There's effectively a separate bus for memory and
  IO operations.
* There's a tiny pipe dream that I could get CP/M up and running one day.
* I had some experience with the [C language toolchain for it][st].

[book]: https://books.google.co.uk/books/about/Build_Your_Own_Z80_Computer.html?id=mVQnFgWzX0AC&redir_esc=y
[st]: https://github.com/insom/spectrum-toys
[^1]: That said, modern 6502's can apparently be clocked down to DC due to a fully static design. Good for them! My Z80 runs at 125Khz - that's been slow enough so far.

+++
date = "2016-03-25T21:02:30Z"
draft = false
title = "Z80 Microcomputer"
synopsis = "The retro-computing itch comes to us all. Latest update: Not even Hello World."
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

Not Even Hello World
--------------------

I ordered a Z80 (6MHz DIP40 version, though it's never going that fast &mdash;
Z84C0006PEG) and a 1MHz oscillator (not just a crystal!). I had 8K of flash
ROM and 64K of static RAM left over from an earlier project. I got a set of
three breadboards to build everything on, an Arduino Mega clone to program it,
a little breadboard PSU and a PIO (because it was cheap).

The first step was to get a 'free running' circuit working. [I followed what
Stian Soreng did][ss] and pulled the data and address busses down to ground,
and NMI/INT/HALT/WAIT and RESET up to 5V. I put an LED (uh, segment) on A11 so
that I could see the light flash around every 2048 instructions.

![Free Runner](https://farm2.staticflickr.com/1516/25428787264_652b78aa8b_b.jpg)

[ss]: http://jmp.no/blog/free-running-a-z80

Slowly Slowly
-------------

I ended up using a 7493 to divide the clock by 8 so I could slow things down
and monitor things with LEDs. This means my Z80 is running at 125KHz.

![7493](https://farm2.staticflickr.com/1608/26033478585_490e37db7b_b.jpg)

I'm thrilled to say that this worked first time: I'd never used this part and
the part itself was highly suspect, but you can see the original and the
divided signal on the A and B channels of our oscilloscope:

![Oscilloscope Output](https://farm2.staticflickr.com/1711/25760704000_5d75097680_b.jpg)

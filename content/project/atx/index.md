+++
date = "2016-04-25T13:12:30Z"
draft = false
title = "attiny461-atx"
synopsis = "An ATtiny461-based power supply controller, emulating ATX semantics"
+++

As with many of my projects, this one deserves more of a write-up than I have
time to give it right now. I wanted to get it [up on GitHub][gh] so that
anyone else looking for an example of using the timer interrupts / ISRs on the
ATtiny461 can find it.

The algorithm is basically:

> * If the power is off, turn it on when the button is pressed.
> * If the power is on, don't turn off on a single press.
> * If the power is on and the button is held for 6 seconds, turn the power off.
> * If the button is held after the power has been turned off, don't turn on
>   until it's released first.

Hopefully [the code is commented enough to be useful][c]: set up the prescaler
and the ports, then enable interrupts and do everything else in the ISR.

The Makefile is from [Markus Conrad's project][p], and was invaluable for
getting things up and running. I used a BusPirate for programming, though I've
gotten a real AVR programmer in the mean time, so I'd like to revisit and
finish this.

[p]: https://github.com/internaut/attiny-instructable/
[gh]: https://github.com/insom/attiny461-atx
[c]: https://github.com/insom/attiny461-atx/blob/master/main.c

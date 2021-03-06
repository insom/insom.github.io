+++
date = "2016-05-12T18:05:09Z"
title = "Butchered USB TTL Serial Adaptor"
type = "project"
synopsis = "Turning a USB to RS232 adaptor into a slightly worse USB to TTL Serial adaptor."
+++

\<insert apology here\>

Isn't it frustrating when you've got a new toy and you want to use it but
you're missing the one piece that you need?

I ordered an [STM32F103 board][eb] on eBay at the start of April. After six
weeks I assumed it went missing so I ordered another one, which arrived a
couple of days later. Then the original one arrived. Typical, eh?

I bought these so I could follow along with the [JeeLabs Dive into
Forth][forth] and because the Cortex ARMs seem ridiculously powerful compared
to the ATmega chips that I've been using so far.

The first one arrived and all of the 5V serial leads we collectively had for
the office (one FTDI cable and a BusPirate) were back at people's homes. I
even looked to see if we had a spare Arduino Uno &mdash; one neat trick is that you
can pry the ATmega out of an Uno and then use digital pins 0 and 1 hooked up
to another device to re-use the Arduino's serial bridge. (Also works for at
least Diecimila and Duemilanove).

Nope. All the Arduino-and-clones we had spare were the kind you program with
an FTDI lead.

The next day [Bas][] brought in the FTDI lead and I got the first board
bootstrapped with Forth, but then my second board arrived and he suggested
getting the two to speak would be a nice 'learning Forth' project. This was
going to mean more cables.

Clearly two cables between four people and a half-dozen semi-finished projects
wasn't tenable, so I bought a couple more leads on eBay from "UK sellers". My
experience in the past has been the cheapest UK sellers are ... not in the UK.
Often, my package ends up arriving a couple of weeks later with a CN22 form
stuck to the outside. This is fine, it's the cost of buying the cheapest
parts.

I didn't want to wait, so I had a look around what we had on hand. We did have
a spare RS232 to USB adaptor. This one speaks the full &plusmn;15V, and I
briefly considered using one of the spare MILSPEC SP232's we got cheap to
bring it down to TTL, but that seemed mad: there was going to be a MAX232 or
similar in the serial adaptor bringing it up to voltage so it could go a
couple of centimetres before getting stepped down again.

Here begins our adventure into very small wires, fine SMT soldering and hot
glue.

We popped open the case, and there were two main ICs, a [Prolific 2303][P]
(the USB to Serial IC) and a [ADM3251E][ADM] (the RS232 line level convertor).
I tried to desolder this with no success, but Bas stepped in, cut the leads
with a craft knife and ran the iron over the chip's leads and it basically
fell off. He also did the very fine soldering to pins 1 and 5 of the Prolific
chip, TX and RX respectively.

![Original Soldering](https://farm8.staticflickr.com/7759/26982444156_637bd86d93_c.jpg)

This was pretty fragile, but a quick test of connecting the two whiskers with
a crocodile clip and typing into a serial terminal confirmed the connections
were good (we got the characters we typed echoed back).

The most convenient connector for our purposes was going to be female DuPont
wires, so I cut down some female-to-male wires with a vague colour code of:

* Red for +5V
* Grey for GND (because I had no black)
* Blue for TX (why not?)
* Green for RX

![Colours](https://farm8.staticflickr.com/7581/26982437376_50127c2922_c.jpg)

Clearly those very fine wires were going to be mechanically weak, so I
borrowed [my wife][]'s glue gun to hold everything in place, and soldered the
much larger DuPont wires to the trimmed magnet wire.

![Outsized Wires](https://farm8.staticflickr.com/7388/26982429786_6cc66b99db_c.jpg)

Time for a quick test with the microcontroller (power is supplied separately
at this point):

![The Microcontroller](https://farm8.staticflickr.com/7328/27015868945_0c1429f81c_c.jpg)

![Woot Test](https://farm8.staticflickr.com/7595/26947661471_b9855b1872_c.jpg)

I drilled a hole in the original case large enough to admit the leads and
wired up the 5V and GND. I pressed everything down and liberally covered it
all in hot glue (not pictured: the photo didn't come out properly).

![Hole](https://farm8.staticflickr.com/7159/26742033910_bcc2a00131_c.jpg)

The original case even fits back on more or less snugly.

![Case](https://farm8.staticflickr.com/7624/26921677542_4436f841dd_c.jpg)

Now I could talk to both microcontrollers at the same time, and get to work
activating their second USARTs to get them speaking to each other.

(Oh, and the cables I ordered actually were in the UK, and they arrived the
next day. Still, there's no harm in spending a lunch time hacking something
you need from what you have to hand, right?)

[eb]: http://www.ebay.co.uk/sch/items/?_nkw=STM32F103&_sacat=&_ex_kw=&_mPrRngCbx=1&_udlo=&_udhi=&_sop=12&_fpos=&_fspt=1&_sadis=&LH_CAds=&clk_rvr_id=1030244027204&rmvSB=true
[forth]: http://jeelabs.org/2016/02/dive-into-forth/
[Bas]: http://balloz.nl/
[ADM]: http://www.analog.com/media/en/technical-documentation/data-sheets/ADM3251E.pdf
[P]: http://www.prolific.com.tw/US/ShowProduct.aspx?pcid=41&showlevel=0017-0037-0041
[my wife]: http://blog.knittage.com/

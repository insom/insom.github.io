+++
date = "2016-11-06T22:15:32Z"
draft = false
title = "Just another Arduino weather monitor"
synopsis = "Just another Arduino weather monitor"
+++

Just a quickie project, mostly consisting of smushing Adafruit modules and eBay
modules together in hardware, and a few choice libraries in software.

I've moved to Canada and I have a garage with mains power. I've never been
through this kind of winter before, so I thought it would be interesting to log
the inside and outside temperature and outside barometric pressure.

While there is power,
there's no connectivity in the garage, so I'm following in the footsteps of
some of my friends back in England, and using a pair of Nordic nRF24 modules to
establish a reliable radio link.

The source code to the whole thing is [on GitHub](https://github.com/insom/weather), and it depends on the [RF24][] and [Adafruit Sensor][af] libraries.

[RF24]: https://github.com/tmrh20/RF24/
[af]: https://github.com/adafruit/Adafruit_BMP085_Unified

I tested my set up by starting with low power amplifier settings on the radios
and using [an example sketch provided with the library][sk]. It might seem like
setting power to the maximum is the best, but that also puts the most load on
your 3.3V voltage regulator, and some Arduinos and clones don't deal well with
that.

Luckily, I was able to verify that mine does, and use that. I also only
transmit at 250Kbps, which results in acceptable performance at higher ranges.

[sk]: http://tmrh20.github.io/RF24/GettingStarted_8ino-example.html

#### Sensor Node

<table border="1" cellpadding="5">
<tr><th>Arduino Pin</th><th>BMP180 Pin</th><th>nRF2401+ Pin</th></tr>
<tr><td>GND</td><td>GND</td><td>GND</td></tr>
<tr><td>VCC</td><td>VCC</td><td>-</td></tr>
<tr><td>3.3</td><td>-</td><td>VCC</td></tr>
<tr><td>7</td><td>-</td><td>CE</td></tr>
<tr><td>8</td><td>-</td><td>CSN</td></tr>
<tr><td>11</td><td>-</td><td>MOSI</td></tr>
<tr><td>12</td><td>-</td><td>MISO</td></tr>
<tr><td>13</td><td>-</td><td>SCK</td></tr>
<tr><td>A4</td><td>SDA</td><td>-</td></tr>
<tr><td>A5</td><td>SCL</td><td>-</td></tr>
</table>

#### Receiver Node

<table border="1" cellpadding="5">
<tr><th>Arduino Pin</th><th>nRF2401+ Pin</th></tr>
<tr><td>GND</td><td>GND</td></tr>
<tr><td>3.3</td><td>VCC</td></tr>
<tr><td>7</td><td>CE</td></tr>
<tr><td>8</td><td>CSN</td></tr>
<tr><td>11</td><td>MOSI</td></tr>
<tr><td>12</td><td>MISO</td></tr>
<tr><td>13</td><td>SCK</td></tr>
</table>

#### Reading the Output

As long as the Receiver node can pick up the signal from the Sensor node, you'll get regular data output over the USB serial:

    picocom --baud 9600 /dev/ttyUSB0
    ...
    OHAI
    P1022.41T4.30

That's 1022.41 hPa and 4.30C outside.

Because I have a local Graphite install, I can just send metrics to
`127.0.0.1:2003` with a [tiny Ruby script][trs], which parses the serial output
and spits Carbon formatted data over TCP to my Graphite install.

![It's not cold ... yet](https://insm.cf/_/temperature.png)

[trs]: https://github.com/insom/weather/blob/master/agent.rb

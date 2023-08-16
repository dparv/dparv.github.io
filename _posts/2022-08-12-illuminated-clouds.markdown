---
title: Illuminated RGBW clouds
subtitle: ""
layout: post
tags: ["led", "shelly"]
---

I wanted to make some nice clouds with backlightning for my kids room, however everything I found online was not up to my expectations, so naturally I went for the DIY approach :)

First... where to find anything that looks like clouds. I found someone online selling wooden shelves with the backside shaped like clouds, so I reached out and asked only for the 'cloud' part and I got 3 very nice different clouds!

![Raw1](/assets/images/ledclouds/raw1.jpg)

![Raw2](/assets/images/ledclouds/raw2.jpg)

I got a 12V [simple RGB LED strip][vivaluxled]{:target="_blank"} (14.4W/m), because I've cut it to be super short I had hoped that it wouldn't consume that much power, but it still took .5 amps (~6W)! Hungry hungry hippo..

{:refdef: style="text-align: center;"}
![Power1](/assets/images/ledclouds/powermeter.jpg)

![Power2](/assets/images/ledclouds/power2.jpg)
{: refdef}

So the initial idea was to use 18650 batteries (3.7V) and use a simple 12V LED strip and I bought a charger and 6 batteries, they did work great initially but.. for a week the [Shelly RGBW2][rgbw]{:target="_blank"} device I had to controll the LED strip drained the battery (althou it only drained about ~2 mA..) and I didn't realize that.. which just destroyed the chemistry of 2 of those batteries, so I decided to abandon that path. Another downside was the battery pack was pretty thik, so the clouds stays weird on the wall. The solution was simpler - [12V DC power source][vivaluxpower]{:target="_blank"} and some cables that I masked with cable canal on the wall and the same paint I painted the wall :-D

This is how it rawly looked without the battery holders:

![RGBW1](/assets/images/ledclouds/rgbw1.jpg)

![RGBW1](/assets/images/ledclouds/rgbw2.jpg)

I got to mounting on the wall and this is the result:

![Room0](/assets/images/ledclouds/room0.jpg)

![Room1](/assets/images/ledclouds/room1.jpg)

![Room2](/assets/images/ledclouds/room2.jpg)

Added some decorations as well:

![Room3](/assets/images/ledclouds/room3.jpg)

[rgbw]: https://www.shelly.com/en-bg/products/shop/shelly-rgbw2-1
[vivaluxled]: https://vivalux.bg/en/product/viv003591
[vivaluxpower]: https://vivalux.bg/en/news/new-series-of-led-power-supplies-vivalux
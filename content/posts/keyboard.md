---
title: "Keyboard"
date: 2021-03-27T16:05:51-05:00
draft: false
description: "My keyboard"
tags: ['workflow', 'keyboards']
externalLink: ""
series: []
math: false
---

## My Keyboard

I'm really into custom mechanical keyboards. I find they help me be more productive
since I actually enjoy typing on them. For the last year or so, I've
been rocking a [corne keyboard](https://github.com/foostan/crkbd)

![Old Corne](/images/crkbd_old.jpg)

I like the corne for a number of reasons. I like its split ergonomic design and
also I like how it only has 3 rows. My hands are not especially big and I find
it very convenient to [reach every key](https://github.com/DevonMorris/crkbd-devo) without having to move my hands.
Furthermore, using this keyboard for the past year has increased my
touch-typing ability which is important to me as a developer.

This build is specifically made of following components
* [Corne keyboard PCBs](https://keyhive.xyz/shop/corne-v3)
* [Gateron Brown Switches](https://mechanicalkeyboards.com/shop/index.php?l=product_detail&p=1640)
* [MT3 Susuwatari keycaps](https://drop.com/buy/drop-matt3o-mt3-susuwatari-custom-keycap-set)

Keyboard enthusiasts will know that I went pretty cheap on the switches, but
overall this is a great keyboard. I like it so much, that I'm going to make
a new one! So stay tuned!

## Anotha One

I like the corne keyboard so much, that I'm building another
one (my wife made me swear this would be my last build). This will allow me to
have the same keyboard at work and home, which will be super nice.

Furthermore, it's really nice to work with my hands and build something
physical. I enjoy programming, but in most cases, code is far less tangible
than building a physical object.

So I'm taking my experience building 2 other keyboards, and making
one last keyboard.

*note from future Devon*: this was not and probably won't be my last keyboard

Once again, I am making a corne keyboard. I've got some tricks up my sleeve for
this build and going to try some new things. Here is a basic parts list.

* [Corne PCBs](https://keyhive.xyz/shop/hotswap-corne-helidox)
* [Corne Acrylic Plates](https://keyhive.xyz/shop/corne-acrylic-plates)
* [Glorious Panda Switches](https://www.pcgamingrace.com/products/glorious-panda-mechanical-switches)
* [Lubrication Station](https://www.amazon.com/gp/product/B08JLJZ95Z/ref=ppx_yo_dt_b_asin_title_o01_s00?ie=UTF8&psc=1)
* [Krytox 205g0](https://www.amazon.com/Krytox-Grease-Pure-PFPE-PTFE/dp/B00MWLDALQ/ref=sr_1_1_sspa?dchild=1&keywords=krytox+205g0&pd_rd_r=54e91049-0511-469c-8e59-ded0f2f5aa08&pd_rd_w=8rcUH&pd_rd_wg=1U5sj&pf_rd_p=4fa0e97a-13a4-491b-a127-133a554b4da3&pf_rd_r=7SXDCFKH1KBSY314833A&qid=1616876097&sr=8-1-spons&psc=1&spLa=ZW5jcnlwdGVkUXVhbGlmaWVyPUFLUFpFVlJXSVY0TFAmZW5jcnlwdGVkSWQ9QTA1MzYwNDRCM0g5V0tPRFZMRFAmZW5jcnlwdGVkQWRJZD1BMDYzNDgyMU9PUEdJMEdDTjUwRCZ3aWRnZXROYW1lPXNwX2F0ZiZhY3Rpb249Y2xpY2tSZWRpcmVjdCZkb05vdExvZ0NsaWNrPXRydWU=)
* [Elite C V4](https://keyhive.xyz/shop/elite-c)
* [MT3 White on Black](https://drop.com/buy/drop-mt3-white-on-black-keycap-set)

I already have a soldering iron, solder, flux and digital multimeter, but if you
want to replicate this build, you will want those as well.

## Soldering

I  have soldered only a total of 10 or 15 times, so I'm
definitely not an expert. Here are some tips I've picked up especially for
soldering surface mount components.

Basically, I soldered the surface mount diodes in 3 steps

## Prepare the pads
I start by putting a small amount of solder on each pad.

![wide](/images/prepare_pads.jpg)

### Do the slide
Then I hold the component with some tweezers, heat up the solder I put on the
pad and "slide" the component into the solder.

![wide](/images/slide.jpg)

### Solder the other side
Lastly, I come back and solder the other side of the component.

![wide](/images/fully_soldered.jpg)

### The rest
The rest of the soldering on this board is pretty straight-forward. You put the
component through the hole and then heat both the hole and the component. Flow
solder into the tip of the soldering iron until you have good coverage on both
the component and the hole. It takes some practice but there is this moment
when you see the solder "snap" into place and create a minimal surface. At that
moment you want to remove the iron.

![wide](/images/elite_c.jpg)

The switches are very similar, they just have very large holes to solder into,
so you have to flow quite a bit of solder before you get a solid connection.

Before you solder the switches, it might be worthwhile to do a bit of
debugging to ensure all the connections are good.

## Debugging The Corne Keyboard

The debugging process for the corne keyboard is pretty easy, especially if you
install the oled modules. Basically you want to grab the
[qmk firmware](https://docs.qmk.fm/#/) and flash it on each board.

### Checking the diodes
Immediately after soldering a diode, you can check the connection with a
digital multimeter. The corne pcbs have a convenient traces to test each diode.
Basically just follow [this guide](https://www.fluke.com/en-us/learn/blog/digital-multimeters/how-to-test-diodes).


### Flashing the Firmware
In linux, this consists of putting your boards into dfu mode by holding the
reset button while connecting the keyboard. Then with the board in dfu mode,
you run one of the commands below (corresponding to which side).
```bash
qmk flash -kb crkbd -km devo -bl dfu-split-left
qmk flash -kb crkbd -km devo -bl dfu-split-right
```

### Checking The Firmware and the Board
Once the firmware has been flashed successfully and before the switches have
been soldered, you can test the diodes and the firmware by taking some tweezers
and shorting the connection the switches make.

![wide](/images/test_firmware.jpg)

If you have the OLED modules you'll see the character register on the screen.
If not, you can open a document and see if the letters appear when you make the
short. This is a super easy way to check the progress of the build, so I never
skip it.

## Glorious Lubing of Glorious Pandas

I finally got
all the parts in the mail last week. As mentioned in my previous post, I
splurged and got the [Glorious Pandas](https://www.pcgamingrace.com/products/glorious-panda-mechanical-switches).
Everything I read points to these being some of the best tactile switches if
you lube them properly. So this is my experience with lubing the the glorious
pandas.

I should first mention that [this video](https://www.youtube.com/watch?v=44Wv4OGdmu4)
was super helpful in understanding how to lube switches. The main takeaway is
that "less is more" when it comes to lubing switches.

I tried a few different techniques but this is the process I found to me most
effective and streamlined.

![wide](/images/lube_station.jpg)

Here is my basic setup. I've got my switch opener, a little plate for lube,
a thin brush and the switches. I put a small pea sized amount of lube on the
plate to remind me to no use too much.

![wide](/images/switch.jpg)

For each switch, I perform the following steps
1. Open up the switch.
2. With a pin size amount amount of lube, lube one end of the spring
3. Put the spring on the switch and lube the other end of the spring
4. Grab the stem of the switch with the 4 pronged grabber
5. Distribute a half-pin sized amount of lube between the two slotted sides of the stem
6. Brush the lube on all 4 sides of the stem avoiding the legs to maintain tactility
7. With the remaining lube on the brush, brush the inside of the steam where the spring connects
8. Reassemble and close the switch
9. Repeat!

These switches already felt smoother than the gateron browns before I did the
lubing process, but after lubing they incredible. I'm really pleased with how
they feel and am excited to put them on the PCB soon.

## The Proof is in the Switches?

This is just a place to share a bunch of pics of the finished build. Overall,
I'm incredibly pleased with how the build turned out and how it sounds.

![wide](/images/holy_pandas.jpg)

![wide](/images/black_mt3.jpg)

![wide](/images/angry_pandas.jpg)

---
title: Verilog Snake
permalink: /
---

# hey! it's writeup time

this is the story of the pain i endured to make a project for hack club's [asic](https://asic.hackclub.com) ysws, strap in tight because it's about to get pretty writeuppy in here. 
written in lowercase except where i decide otherwise, because lowercase feels calmer.

## ok, so what even is a hack club asic ysws

[hack club](https://hackclub.com/) is this really awesome non-profit all about getting teens into coding and making projects. they do events called You Ship, We Ship(s) (YSWSs)
where the premise is that you 🫵 build _something_, normally related to the theme or topic of the ysws, and hack club will give you _something_ in return. 
it could be that you design a cookie cutter in 3d design software and receive your physical cookie cutter design and cookies ([bakebuild](https://bakebuild.hackclub.com)), or in this case
you build a project in verilog (keep reading and i'll explain what this is later), and hack club ships you an FPGA to test it on (pictured below), and once you've tested it, they'll fabricate your design, 
along with a bunch of other people's, onto a real manufactured ASIC chip!

![icepi zero](assets/icepi-tilted-transparent.png)

you may notice it looks kind of familiar, that's because it is! it's designed to be the same size and shape as a raspberry pi zero, so it's compact and fits in where the pi zero does too.

## okkkkk, what's an fpga??

fpga stands for Field Programmable Gate Array, which means its got loads (1000s) of logic gates on it (like AND, NOT, OR, etc...) and you can decide how they are wired together. normally,
to do this yourself you'd need so many physical chips and run wires between them, so an FPGA means you can easily do so much more, and you can change the wiring instantly at your will.

real uses for this include digital audio mixers (like [the one my school has](https://www.allen-heath.com/hardware/qu/qu-5-qu-5d/)!) 
here's a [cool article](https://support.allen-heath.com/hc/en-gb/articles/37281423483665-XCVI-a-signal-processing-revolution-from-DSP-to-FPGA) about FPGAs in digital mixers

---

## verilog

verilog is a Hardware Description Language (HDL). this means it's a special type of language where instead of telling a CPU what instructions to run,
you're describing a logical circuit which can then physically become a circuit of logic gates, either in an FPGA or fabricated as an ASIC chip.

just like how you don't have to program computers in assembly language, you also don't have to write verilog using just logic gates. verilog offers **procedures**, which
can express a circuit at a higher level. it all gets compiled down to logic gates at the end, but it's much easier to understand. they can either be

- constant logic gates (`always @(*)`) (this is known as combinational)
  
  or
  
- clocked (`always @(posedge clk)`) which means the circuit is only triggered at the rising edge of every clock cycle.

i started to learn verilog by using [the HDLBits problem set](https://hdlbits.01xz.net/wiki/Step_one)

by the time i got to [More Verilog Features/Reduction Operators](https://hdlbits.01xz.net/wiki/Reduction), i decided it was time to use my limited knowledge of the language in practice, and moved to [VGA playground](https://vga-playground.com/), which is a website where you can write verilog that makes a VGA output, and it gives you a visualisation of it.

## vga playground

![](assets/vga-playground1.png)

at first, i was just messing around with the inputs, trying to figure out how it works and how it decides to draw what where. (sidenote: if you're making something in vga playground, please please make sure to frequently press the download button, because the site clears anything you've written any time the page reloads. i made this mistake numerous times, and while i guess it was a good learning opportunity to have to try and recall and redo what i had done previously, it was not fun)

the default demo project, stripes, has three main outputs you'll want to use, R, G, and B, which as you may have guessed, correspond to red green and blue. "but these aren't pixels, how do i output a picture??" you ask

that's done by timing when rgb are turned on or off to match where the "scan lines" currently are. this is done by comparing the values of pix_x and pix_y to where you want to draw, and setting the colours at the moment you want it to be drawn. say you wanted a red dot at (0, 0), you would check if pix_x == 0 and pix_y == 0, if true set R to 11 (the most red), G 00, B00, or otherwise set R 00, G 00, B 00. 

else branches are important in verilog, otherwise the value either won't change or could be undefined (x) which is bad and painful, i'll get on to this in more depth at a later point.

## snake

to actually make my game, I created a big array of registers that can store the different cells of the snake.

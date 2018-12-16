---
layout: post
title: Xbox Open-Source Video Project
---

![XOSVP Side](/public/images/xosvp-side.jpg)

The following is an indepth run through on _why_ the XOSVP exists. If you just want to see results, then [**CLICK HERE!**](#results)

I think the Xbox is one of the most interesting consoles in existence today. Not because it has many great exclusives (it does). Not because it has the best quality ports of the 6th generation (it does). And not because I owned one as a kid (I didn't).

The interest comes from the unique community that built itself around the system, from hardware hackers to homebrew developers and case modders, the Xbox always held a unique place in console history.

What if you just got yourself a nice fresh Xbox, or maybe pulled out your original from the garage? We are now in this future world where progressive scan and digital video sources are standard. The composite cables from your Xbox are giving you less than desirable video quality. We can do better.

The Xbox natively supports component output, can display up to the 1080i through YPbPr Component, there is even has an official cable from Microsoft themselves! Great! We're done then, right? Well, not really...

![Official Microsoft High Definition AV Pack](/public/images/official.png)

Wow that is expensive for just a cable. Turns out not many people had HD capable televisions back in the day so cables like these have gotten a little rare. Surely someone has made a clone of it right? Unfortunately, none of them are up to the quality and features that I wanted from my Xbox, so I decided to build the **Xbox Open-Source Video Project**.

![Xbox Open-Source Video Project Action Shot](/public/images/xosvp-photo.jpg)

The **Xbox Open-Source Video Project**(XOSVP) is a brand new Xbox video cable capable of YPbPr Component video and optical audio out. But it isn't just a video cable, I took great lengths to ensure this cable gives you the _best_ possible video and audio quality from the Xbox possible. I also want to make sure this cable is easy to build as a hobbyist so I picked easy to source parts and made the design completely Open-Source. Reaching this quality took a lot of effort and careful design.

## Impedance Matching
YPbPr Component video standard requires a transmission line with 75Ohm impedance and 75Ohm termination. This is important to maintain integrity in the signal from the Xbox to the TV. An impedance mismatch will cause reflections in the signal which can degrade the image quality. Higher frequencies and longer video lines make this problem more pronounced.

A general rule of thumb is that impedance matching matters with cables of about 1/4 of the wavelength of the signal. HD component video can easily reach 25MHz-30MHz in frequency, meaning a cable with a run length of about 2.5 meters can be affected by this mismatch. Noise within the line from the Xbox and outside sources may also reach higher frequencies than that, making this requirement even tighter.

To ensure impedance matching throughout the full system, I decided to repurpose a VGA cable. Good quality VGA cables are well shielded which was important. VGA also has 3 RGB video lines that are double shielded and have an impedance of 75Ohm, perfect for YPbPr component video. VGA also has many accessory pins for various signals which is useful for providing voltage and the digital audio to the breadboard. It is also very easy and economical to find VGA cables and plugs as a hobbyist.

![Example XOSVP Cable](/public/images/xbox-to-vga.jpg)

Not only is the cable well shielded and impedance matched, but I also insured all video data traces on the PCB are impedance matched to 75Ohm. So when used with 75Ohm Component Cables, the entire system will be impedance matched to the TV for the best possible quality.

## Shielding
An important piece missing from other cable solutions for the Xbox is shielding. 3rd party cables are severely lacking in shielding which harms the quality of the signal. Especially in today's world where everyone owns a WiFi router and multiple phones, high-frequency noise is everywhere. Using a VGA cable means each video signal is individually shielded, and all 3 are protected once more by another shield with a ferrite bead.

![shielding](/public/images/shielding.jpg)

Combined with high-quality component cables this configuration ensures minimum exposure to outside noise on the video signal. Unfortunately, there is still internal noise sources inside the Xbox, so we need one more step of protection.

## Filtering
The Xbox is not that old of a system, but unfortunately, it is a system that was manufactured during the [capacitor plauge](https://en.wikipedia.org/wiki/Capacitor_plague). Even then the Xbox seems particularly prone to this problem, two of the machines I've inspected have had many malfunctioning capacitors, even ignoring the regular problem with the clock capacitor. Because of this, power filtering is poor and the power supply itself provides rather noisy rails. All of these together mean the Xbox delivers a relatively large amount of noise out of its own audio/video DACs.

This noise is bad for video quality, as it affects the analog signal coming out of the Xbox. This is most noticeable as jitter, looking closely you may see rows of the video signal jump a pixel left or right periodically. Also noticeable is a reduction in accurate color reproduction and "snow" on screens with solid color.

These problems are then amplified by a poor video cable. Without shielding and impedance matching, all of this noise and reflections may amplify or attenuate the true video signal.

To cut down the effects of this problem, I have added a video filter + amplifier combination, the THS7316DR. I spent a lot of time and effort researching multiple video filters to use with the Xbox. Texas Instruments, Analog Devices, Fairchild/On Semiconductors were just some of the manufacturers I tried. The Texas Instruments filter had the best performance, removing most of the noise from the signal while simultaneously not harming the sharpness or clarity of the picture.

Some Prototypes:
![prototypes](/public/images/prototypes.jpg)

The filter takes the incoming video signal and runs it through a low-pass filter to greatly reduce the amount of noise in the video signal. Most good quality TV's with proper input have a low-pass filter on the receiving end. By cutting the noise out of the signal close to the Xbox less of the signal will bounce around within the cable which hurts the quality even more. It also accommodates for lower quality component inputs on TV's, and directly compatible with the OSSC as you can disable its internal low-pass filter.

The filter also re-amplifies the video output from the Xbox, increasing transmission strength and ensuring the video signal meets the required output voltage for maximum contrast.

## Digital Audio
The Xbox and its impressive library of games have amazing audio. Unfortunately, the stock stereo output of the system leaves a lot to be desired. Luckily the Xbox supports high quality digital audio output. By taking this output and converting it into optical audio we can connect the Xbox to modern audio systems for full quality audio reproduction. Thankfully digital audio is relatively low frequency compared to the video source and digital, so recreating perfect audio from the Xbox is very easy.

To get digital audio working I had to test a few optical transmitters. TOSLINK being the default choice, but the only transmitters I could find are about $15 each and still require a custom circuit to convert the digital signal into optical. Luckly I found a cheaper device that can convert the optical signal and transmit the right wavelengths. Unfortunately this cheaper device had an incorrect datasheet so I burned a few of them.

![optical prototype](/public/images/optical-prototype.jpg)
A photo of a couple optical transmitters who have had their magic smoke escape during prototyping.

# <a id="results"></a> Results
All of this is well and good, but you want to see results right?

### Popular Amazon 3rd Party Component Cable
![Popular Amazon 3rd Party Component Cable](/public/images/cheap.gif)

### XOSVP
![Xbox Open-Source Video Project](/public/images/xosvp.gif)

The difference is staggering, it is very clear how much more clean the XOSVP signal is. I was surprised myself, I figured the difference would be tricky to spot but even on the TV I am able to tell the difference between the two cables. Both oscilloscope measurements are on the same video source (the UnleashX dashboard screensaver).

Noise is not the only thing reduced, the following captures show 10s of signal persisted to get a feel for the signal spread + jitter:

### 3rd Party
![3rd Party Jitter](/public/images/cheap-jit.png)

### XOSVP
![XOSVP](/public/images/xosvp-jit.png)

# Open-Source
I want to make this project open-source. My full-time job is working as a software developer, and over the past year I've been teaching myself hardware + electronics and using this as my first real project for fun. Working in software has made me appreciate the power of open-source and I feel hardware design needs a stronger push for the same privileges and this is one of my small contributions.

I will be uploading all the PCB schematics + designs (naturally in KiCad), 3D Print files for the enclosure, BOM for the parts, and build instructions for the adventurous DIY'er. While most of the passives are surface mount, they are not too difficult to solder and the filter is a SOIC footprint. Unfortunately, the surface mount passives are necessary to reduce potential noise and keep the impedance matching as tight as possible. While I wouldn't recommend this as your first soldering project, it isn't too difficult to do yourself.

To help cover the costs of researching and designing this hardware, I will also be selling the unit as DIY kits and completed units. I have also found a part supplier that has provided me with a large supply of brand new Xbox video connectors with a large enough surround to fit the VGA cable from monoprice. These will be sold separately for people not wishing to cut open existing Xbox video cables to make their own.

# Alternatives
Now, why would you use the XOSVP instead of any other alternative solutions? Well, that is a great question, and you may in fact not need this cable.

### 3rd Party Component Cable
![3rd Party Cable](/public/images/3rd-party.png)
This is dirt cheap, easy to find, and does work. You are limited to stereo audio and the video signal will be filled with noise from an impedance mismatch, no filtering, and a lack of shielding. If you are ok with this, then I won't be able to convince you not to use this cable. It is a solid deal, XOSVP is for the people looking for a bit more from their Xbox.

### Pound HDMI Cable, (Hyperkin?)
![Pound Cable](/public/images/pound.jpg)
This cable takes the component video source from the Xbox and converts it to HDMI. If you have a TV without component in, this is probably your most cost-effective option. If you want the video to _just work_ then definitely go for this. There are a few downsides though for those out there who want the ultimate Xbox audio-visual experience.

Firstly, video quality will suffer a bit. The pound cable converts an analog signal to HDMI, but that analog signal was created through a conversion from the Xbox's video DAC. Two layers of conversion is bound to create a drop in quality, also the quality conversion may not be optimal depending on the quality of the HDMI converter within the pound cable.

Secondly, the Pound cable does not send the digital audio through the HDMI, it re-encodes the analog out into digital for HDMI. This again means a drop in quality from the 2 stages of encoding/de-encoding, but also prevents you from experiencing surround sound from any of the available Xbox titles.

### Official Microsoft High Definition AV Pack
![Official](/public/images/official-marketing.png)
If you can get one for a reasonable price, I don't think the XOSVP will meaningfully beat it. These are limited though as they are no longer produced, and as they get rarer and in more demand, the price may rise. Not having an alternative did not sit right to me, which was partially the reason why I started to develop the XOSVP.

# XOSVP FTW
If you have an analog CRT that accepts component, you are all set to use the XOSVP! You will get the most perfect video source from the Xbox using this cable, congratulations! I know not everyone has a way to receive digital audio, especially if all you have is an analog TV. I recommend picking up a half decent optical to stereo converter which should do the conversion just fine, with the benefit of being externally powered and isolated from the noise leaving the Xbox.

If you have a TV that only takes in HDMI or a digital TV with less than stellar component input, I recommend you pick up a RetroTINK-2X or OSSC. Both of these devices do near-perfect analog to digital conversion without introducing any latency so they will get you the best quality picture on your digital television. This combined with the optical out capabilities of the XOSVP should give you near perfect video and audio quality on a modern TV setup.

Hopefully, the XOSVP is cool and useful to you, I had a lot of fun making it. I will be uploading the source to my GitHub soon along with a new blog post describing how to build it, and make parts available.

![final shot](/public/images/with-case-side-1.jpg)
![final shot](/public/images/with-case-side-2.jpg)

---
title: Linux bluetooth fix for QC35 II
date: 2023-07-25
tags:
- linux
- bluetooth
- hardware
summary: How I got full bluetooth support for Bose QC35s on Linux
---

I have been having trouble getting my Bose QC35 headphones working on Linux. The symptoms were:
* A2DP worked fine (high sound quality, no mic - to be expected)
* HFP/HSP did not work at all: no mic or audio output

Now everything seems to work just fine.

"What changed", I hear you ask...

I bought a new bluetooth dongle. Not a very exciting change I agree, and also does not really narrow down what _actually_ fixed the problem, but here are some working theories:
* The new dongle is bluetooth 5 rather than 4 which means double the bandwidth amongst other changes. The headphones [support bluetooth 4.1 and 5](https://www.soundguys.com/how-to-fix-problems-bose-quietcomfort-35-ii-36217/) so it's possible the dongle I bought was 4.0, but unlikely.
* The old dongle was actually defective or broken in some way. This is also unlikely since it worked fine in A2DP mode.
* The new dongle is plugged into a front-mounted USB port, which I know is USB3. The back port may have been USB2 (to be checked).

This post is a living document in the sense of: I cannot check which of the above is the right answer right now, but will do in the future. Watch this space.

**Update 2023-07-29**: the previous dongle was definitely Bluetooth 4 so it's likely that was the cause of the issue. Perhaps it was Bluetooth 4.0 which is not supported. It certainly has less bandwidth than Bluetooth 5.

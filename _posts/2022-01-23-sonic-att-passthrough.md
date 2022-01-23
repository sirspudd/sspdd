---
layout: post
title: Painpoint in successful BGW320-505 passthrough on AT&T/Sonic
subtitle: My digital stubbed toe 
date:   2022-01-22
published: true
tags: [passthrough,at&t,sonic]
---

Having finally moved to a bit of San Francisco where Sonic is an option, I was stoked at the prospect of a symmetric connection. The slight catch is that AT&T appear to have a monopoly on the infrastructure in my area, so there is an additional ($30/month) hop, and I have to use AT&T's hardware. Cool beans, it is just a dumb modem, so this is a non-event, right?

So starts a multi day melodrama in an attempt to get this passthrough working.

I contacted Sonic support who have consistently been solid, a minute or 2 on the line and you get a real human being; I can't stress how cool this company is in comparrison to the corporate American trope of pissing their customers time against a wall of robo voices or people on the other side of the world who appear to have a 10 kb/s connection judging by voice quality and latency. The Sonic peeps once faced with my request told me passthrough was unsupported and they could do nothing to help me other than wish me luck and point out there are plenty of guides which deal with exactly my asking point.

Ok, cool. The problem is the first guide I followed told you to allocate a fixed ip for your secondary router before passing it through. This single step taken in following guides, cost me several hours of life. Having passed through my router, it was non-pingable, and nothing was exposed. In debugging this, I went through every permutation of options available to me. The problem, which will probably be evident to anyone with a networking clue, is that the allocated ip I had set in following the guide, was not the same as the public ip assigned to the secondary router as part of the passthrough. This mismatch in BGW320-505 allocated ip vs the passthrough ip was responsible for all my woes. Removing the explicit allocation at the BGW level and rebooting the secondary router got it the correct wan ip, and saw all traffic flowing correctly.

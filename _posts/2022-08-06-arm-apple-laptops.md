---
layout: post
title: Apple M1 laptops from a Linux fiends perspective
subtitle: The prophesized one
date:   2022-08-05
published: true
tags: [linux, arch, fedora, Apple, laptop, M1, M1 Max, Asahi]
---

# Overview

I have been waiting for the arrival of ARM laptops with baited breath; I jumped on the samsung series 3 chromebook in 2012; it was a $250 device with a TN panel, slow storage; it cut every corner and you could feel it. I am sick of the Intel monopoly on desktop machines, and would like to a diverse range of designs/instruction sets. The band aid needs to come off, and the only really cost is 30 years of dos/Windows binaries tied to x86/AMD64 and Rosetta Stone 2 has shown that this does not even need to be a complete break.

I am super stoked Apple went all in on ARM; they are well positioned to make this kind of momentous leap. They tooling and their ecosystem make it entirely possible to pop out binaries targeting a novel architecture. Cross compiling with gcc is nontrivial and stepping people through cross compilation in the Qt landscape normally involved several visits to the school of hard knocks.

My daily job involves C++ development and the ongoing design of a custom yocto OS which I have to iteratively improve on and build. This therefor constitutes my primary load testing.

I also work with embedded ARM devices, have done so for over a decade and am used to what is normally passed off as a BSP by ARM hardware vendors. Anyone hoping for something as functional as the M1 emerging outside of Apple better get their prayer game on because the average ARM vendor takes a shit on the chest of their customers, and by proxy their customers customers. You don't have to believe in trickle down economics to believe in trickle down technical debt. Now that Google is making their own chips, you might in the future be able to sacrifice your privacy for a reasonable ARM consumer device. It would be hard for me to overstate Qualcomm's role in screwing consumers and napalming the ARM ecosystem. The whole reason I moved away from Android is because on principle I will never buy another Qualcomm based handset while I can afford to vote with my wallet. Qualcomm is the face of patent Trolldom for me.

# Platforms

* M1 Macbook air, base configuration (awesome value)
* M1 Max Macbook Pro kitted out (my attempt at an Apple ARM compilation beast)

# Successes

* Absurdly good battery life; perf per watt is nuts
* laptop feels like any other high end polished laptop
* entry level m1 air is fantastic value

## Asahi Linux native

Asahi Linux was a treat to download and install; as soon as they had an alpha I grabbed it and managed to install it with relatively little drama. Their instructions were awesome, there were relatively few impediments and within X minutes I was inside of a nimble feeling KDE install. Everything basically worked out the can, and the only really evident limitations are easy to understand; lack of GPU acceleration and lack of sound support on the M1 Max.

I was not blown away by the compilation performance I saw under Asahi; I don't know what the primary bottleneck is/was. I did not get the impression the cores were clocking up to full load the device, and I was relying on the scheduler to know the difference between performance and efficiency cores. I do intend to provide concrere metrics; please accept my handwaving in the meantime

As mentioned, I work with ARM platforms; I have been downwind of the TI OMAP, broadcom, st-micro, nxp (the stb-220, imxX), Nvidia (the Tegra2 onwards). If you are going to learn about ARM considerations on any reference platform and if you can afford it, I would recommend the M1. So nice to see a civilized well integrated Arch OS set up by people who have a clue. Most hardware vendors appear to be Windows users, and their attempts at Linux integration really communicate this. Learning from first principles and google hurts; learning from bright clued up people is comparatively economical.

## Fedora aarch64 under Parallels

I am normally an Arch fiend, but I have a lot of love for Fedora and their aarch64 port seems gloriously nonbroken. I remember some lumps in the Fedora installer process which required minor experimentation to overcome; that said, once installed Fedora on aarch64 behaves exactly like Fedora on any other platform.

Building my yocto OS under Fedora allowed me to actually hit a memory bottleneck for the first time. My cores were humming and I managed to run out of the 27.7G I had allotted; I am actually stoked/impressed why my cores do enough work to choke out that much memory. I normally hit a CPU throughput ceiling well before I hit a memory ceiling. 

Parallels also does not make it very easy to pass through the performance cores and hold on to the efficiency cores. I basically want Mac OS to have perpetual access to the efficiency cores, and I want the build VM to be loading the performance cores. Parallels just had a knob which let me pass through 1-10 cores.

## Building a Yocto OS on an aarch64 host

On a positive note, it was comparatively trivial to get Yocto spitting out SDKs for an aarch64 host. The only real adjustment I had to make was within our own software realm which assumed an intel host and an ARM target. It speaks volumes about the thoroughness and competence across the community that everything else largely worked out the box. (We also thankfully don't use custom allocators like jemalloc and other pieces of code which were/are known to blow up with 16kb page sizes)

# Disappointments

* shitty window management; why does cmd+tabbing between apps take me back to the top level non fullscreen desktop, instead of to the app I just selected. 2 cmd+tab selections to move from fullscreen application A to fullscreen application B

## Running iOS applications

When I purchased my first m1 device, I had bought into the idea that I could run iOS applications on my laptop. This appealed as I was expecting to be able to cut out all the fat web runtime crapware people ship out of convenience to themselves and no-one else. There are plenty of apps I would like to run the mobile version of:

* spotify
* slack
* lastpass
* tidal

there is no real point enumerating these things; this list is not complete. Let me be more direct, I have never searched for an App I wanted in the App Store and found it under iphone/ipad apps. Not a single solitary time. If you google this crus, people step you through grabbing application bundles off your phone and transferring them to your laptop. Hello, what, why the hell are the publishers allowed to dictate this crud. This is a clear case of publisher vs end user, and it appears the publishers have been empowered to be complete asshats on this front. I like Apple as they normally give off the impression of being pro-end user; not so much on this front.

## Metal and only metal

This hardware is lovely; I consider it an affront that Metal is the only game in town. Ongoing Vulkan and opengl support would not bankrupt Apple and would feel less like vendor lock in.`:w

## Emulating AMD64 OSes

This one is probably largely on me, but I just want to make it crystal clear that if you do attempt to run a virtualized AMD64 OS, it will be as slow as mollases. If there is a fast path there, I have not seen it.

## Apples Virtualisation Framework

Ideally I can get Fedora running on Apple's Virtualizatin Framework without any Parallels participation and with less overhead. This is in the drawback column at present as information is thin on the ground; I tried to compile a reference Application from Apple which is meant to facilitate this, and I stumbled at the signing point.

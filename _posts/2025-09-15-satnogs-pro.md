---
layout: post
title: Building a Non-Rotary SatNOGs Ground Station in Egypt  
date: 2025-08-25 1:00:00
description: A summary of my internship project with Outlyer.space to develop a non-rotary ground station in Egypt.
tags: satnogs, ground-station, omni-antenna
categories: outlyer.space
featured: false
images:
  compare: true
  slider: true
---

# Building a Non-Rotary SatNOGs Ground Station in Egypt

## Project Overview  
**Project**: Development of a SatNOGs non-rotary omnidirectional ground station with two Power Distribution Units (PDUs) to support a future rotator system.  
**Organization**: [Outlyer.space](https://outlyer.space/)  
**Timeline**: February – September 2025 (~30 weeks)

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/satnogs/8.jpg" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Introduction  

This post documents the challenges and lessons learned from building what began as a 5–6 week project but evolved into a **30-week** effort.  

If you’re new to [SatNOGs](https://satnogs.org/), it’s an open-source, global network of satellite ground stations. I encourage you to check it out if you haven’t already.  

I’m especially grateful to Ralph, founder of [Outlyer.space](https://outlyer.space/), for offering me this unique opportunity. Not only did I gain practical experience with satellites, antennas, and ground stations, I also discovered the realities of shipping electronics to Egypt.

## First Build  

My first attempt followed the [SatNOGs omnidirectional station tutorial](https://wiki.satnogs.org/Omnidirectional_Station_How_To). The setup consisted of:  

- An omnidirectional antenna  
- A general-purpose LNA  
- A NooElec SDR  
- A Raspberry Pi 5  

All components were mounted using:  

- [DeskPi RackMate](https://deskpi.com/products/deskpi-rackmate-t0-black-version-rackmount-10-inch-4u-server-cabinet-for-network-servers-audio-and-video-equipment)  
- [DeskPi KL-P24 Shelf](https://deskpi.com/products/deskpi-kl-p24-raspberry-pi-adapter-board?_pos=5&_sid=2d380a52d&_ss=r)  
- [DeskPi DC PDU Lite](https://deskpi.com/products/deskpi-dc-pdu-lite-7-ch-0-5u-for-deskpi-rackmate-t1?_pos=1&_sid=f10dbf8b5&_ss=r)

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/satnogs/5.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

### Challenges

#### 1. Shipping Delays  
Egyptian customs are thorough (which is good!), but shipping a single component could take up to a month. This delayed the build significantly.

#### 2. Power Delivery Issues  
The DeskPi PDU had an unadvertised ~0.4 V drop per channel due to safety diodes. This caused the Raspberry Pi to fail to boot.  

At first, I suspected insufficient current and purchased a 10 A / 5 V adapter—without success. DeskPi support later confirmed the PDU was not designed for 5 V loads and recommended the [52Pi 4-Channel 5 V Power Supply Module](https://52pi.com/collections/new-arrivals/products/52pi-4-usb-channel-5v-power-supply-module-for-raspberry-pi-pico-0-91-inch-oled-screen-compatible-with-1u-rack-mounting).

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/satnogs/10.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Revised Build: Dual PDU Setup  

In the new configuration, the 52Pi PDU is connected to the original DeskPi PDU so they can power each other. This allows the 5 V components to be isolated while preserving the existing hardware for future upgrades.  

The Raspberry Pi and the new PDU are securely mounted on a [DeskPi DP-0039 Bracket](google.com).  

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/satnogs/11.png" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/satnogs/9.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

This iteration is now ready to make its first observations.

### Performance on NOAA Satellite Reception  

As expected for an omnidirectional antenna, especially from the 5th floor, reception is limited. Nevertheless, faint signals were successfully detected and are visible in the waterfall plot below:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/satnogs/12.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Useful Links  

- [Outlyer.space](https://outlyer.space/)  
- [All Weekly Blogs](https://studhamza.github.io/hamza-folio/blog/tag/gnuradio/)  

---

Thanks for reading! Feedback and suggestions from the SatNOGs community are very welcome — especially regarding improving reception from an omnidirectional setup in an urban environment.

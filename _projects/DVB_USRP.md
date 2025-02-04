---
layout: page
title: USRP DVB Project
description: another without an image
img:
importance: 3
category: communication
---

In this project, I did hardware-in-loop testing for DVB V2 signal. I used a USRP N2900 to transmit and receive a audio signal.

First part of this project, evaluates the performance of DVB-S2 using MATLAB simulations
under different channel conditions, focusing on the BER performance with
8PSK modulation in AWGN, Rayleigh, and Rician channels. A comparison
between DVB-S, DVB-S2, and DVB-S2X standards is also presented, along with
their key parameters extracted from the ETSI standard.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/USRP/AWGN.png" title="Channels" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/USRP/rayleigh.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/USRP/rician.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
AWGN Channel
Rayleigh Channel: To simulate multipath fading with no line-of-sight
(LOS).
Rician Channel: To evaluate performance under fading with a
dominant LOS component.
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/USRP/channel.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Channels
</div>

Second Part of this project is the Hardware-In-Loop testing of a DVB S2 audio singal using a USRP N2900 and Labview software.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/USRP/labview1.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/USRP/labview2.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Transmission and Reception Block View
</div>

More about this project, [GitHub Repository - DVB-S2 Standard Performance Evaluation](https://github.com/Adham-Saad163/DVB-S2-Standard-Performance-Evaluation)


---
layout: page
title: SDR MATLAB Simulation
description: Design and implement an effective FIR filter for the purpose of isolating the desired NB-IoT signal from a composite IF signal. 
img: assets/img/DSP/cover.png
importance: 3
category: communication
---

- Developed a customized FIR filter for the NB Physical Uplink Shared Channel (NPUSCH) to support uplink data and control information for six NB-IoT devices.
- Designed the filter to attenuate interference from adjacent channels, noise, and unwanted signals, ensuring the waveform primarily contains NB-IoT carriers.
- Configured the FIR filter to handle concurrent data transmission from six NB-IoT devices, ensuring efficient processing of unique data streams.
- Optimized the filter design for seamless integration with the down-sampling stage, ensuring compliance with 180kHz narrowband carrier specifications.
- Ensured compliance with NB-IoT standards, particularly in Standalone mode, for operation outside the LTE spectrum, including GSM or satellite communication bands.
- Achieved a computationally efficient FIR filter, meeting real-time processing demands for the NB-IoT SDR base station receiver, and providing a clean signal for subsequent processing stages.

<div class="row">
    <div class="col-sm mt-6 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/DSP/KaiserFIR.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-6 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/DSP/powerSpectrum.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
Impulse Response of 104 tap Kaiser FIR. Power Spectrum of 6 different transmitted NB-IoT signal on different IFs. 
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/DSP/fdomain.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Frequency Domain of transmitted signal
</div>

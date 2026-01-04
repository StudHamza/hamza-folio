---
layout: page
title: SatNOGs Ground Station
description: Non-rotary omnidirectional ground station with dual PDU architecture for satellite telemetry reception
img: assets/img/satnogs/8.jpg
importance: 2
category: communication
mathjax: true
related_publications: false
---

This document covers the technical aspects of a complete SatNOGs ground station implementation featuring dual Power Distribution Unit architecture and omnidirectional VHF/UHF reception capability.

<div class="row">
    <div class="col-sm mt-2 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/satnogs/8.jpg" title="Complete Ground Station Assembly" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-5 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/satnogs/11.jpg" title="Dual PDU Configuration" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-5 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/satnogs/9.png" title="Rack Integration" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Final ground station assembly with DeskPi RackMate 4U cabinet and dual PDU power delivery system
</div>

## Small Introduction
When I first thought about satellites, the first think that comes to mind is how far they are. Receiving a satellite signal was sci-fi or big man thing until I stared reading about SatNOGs. Then is was clear that what anyone really needs to receive a satellite signal was a piece of wire and a radio. Technically we all are receiving radio waves, through our body and every metal object around us; however we can't "capture" them. The reason is explained by the [Nyquist–Shannon sampling theorem](https://en.wikipedia.org/wiki/Nyquist%E2%80%93Shannon_sampling_theorem):


<strong>Theorem:</strong> Let $$x(t)$$ be a band-limited signal such that $$X(f) = 0$$ for all $$|f| > B$$. 
  Then $$x(t)$$ is uniquely determined by its samples $$x(nT)$$ if:
  $$T < \frac{1}{2B}$$


Intuitively, think of how your eyes sees a helicopter propeller strobe, you might notice that the blades are moving really slow or even stationary! Wrong, thats called the [Wagon-Wheel Effect](https://en.wikipedia.org/wiki/Wagon-wheel_effect); what is really happening (super simplified) is that your eyes takes a "picture" every 100ms, but every 100ms the blade will have rotated a full cycle plus a centimeter, so you only see it move a centimeter. Your perception is limiting you.

<div class="row justify-content-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        <video controls class="img-fluid rounded z-depth-1">
            <source src="https://upload.wikimedia.org/wikipedia/commons/transcoded/7/77/Propeller_strobe.ogv/Propeller_strobe.ogv.480p.vp9.webm" type="video/webm">
        </video>
    </div>
</div>

But just like how your eyes samples light at a specific rate, a computer sound card (DAC) samples at $$48 kHz$$. This is fine for audio (up to $$20 kHz$$); however the lowest satellite transmission frequency (VHF band) is 30 - 300 Mhz, which would require a DAC with sampling rate of **60 million up to 600 million**. Lucky the SDR solves this problem by digital down conversion, which is just a fancy way of saying shifting a slice of spectrum to lower frequencies before the ADC. Then whats really left, is a signal processing chain hosted on a hardware component.

## System Architecture

The ground station implements a standard SatNOGs omnidirectional reception chain. The RF path consists of an omnidirectional dipole antenna feeding a wideband low-noise amplifier, connected via coaxial cable to a NooElec RTL-SDR receiver interfaced with a Raspberry Pi 5 USB port, running SatNOGs client software. All components are integrated into a DeskPi RackMate 10-inch 4U server cabinet with custom power distribution.

The architecture supports future expansion to rotator-based directional tracking through the dual PDU configuration.

## Technical Implementation

**RF Reception Chain**
The omnidirectional antenna provides hemispherical coverage with typical 3dBi gain across the VHF/UHF amateur satellite bands. The general-purpose LNA maintains noise figure below 1dB with sufficient gain to overcome SDR receiver noise.
<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/satnogs/4.jpg" title="RF Frontend" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

**Power Distribution System**
Initial deployment revealed voltage regulation issues with the [DeskPi DC PDU Lite](https://deskpi.com/products/deskpi-dc-pdu-lite-7-ch-0-5u-for-deskpi-rackmate-t1), which exhibits approximately 0.4V drop across output channels due to series protection diodes in each channel. This drop proved incompatible with Raspberry Pi 5 power requirements, causing boot failures despite enough current capacity. Voltage measurements across PDU outputs confirmed the regulation issue extended beyond simple current limitation.
<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/satnogs/11.jpg" title="Power Distribution" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/satnogs/20.png" title="PC and PDU Mount" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    52Pi 4-channel USB power module providing isolated 5V rails for compute and SDR components
</div>

The dual PDU architecture resolves this constraint by cascading a 52Pi 4-channel USB power module with the existing DeskPi PDU. The 52Pi module delivers regulated 5V to the Raspberry Pi and SDR hardware through independent USB channels, while the DeskPi PDU maintains 12V distribution for future motorized components.

**Mechanical Integration**
Component mounting utilizes DeskPi KL-P24 adapter boards and DP-0039 brackets for secure mechanical attachment within the rack rails. The Raspberry Pi mounts directly to the KL-P24 shelf with proper standoff spacing, while the 52Pi PDU attaches via custom bracket fabrication. Cable management maintains minimum bend radius for coaxial lines and separates power distribution from RF signal paths.

**Software Application**
Initially downloaded a desktopless raspberry pi image, however for more friendly solution switched to a desktop environment. Configured SatNOGs software via package manager, and initialized ground station as stated in the [SatNOGs Wiki](https://wiki.satnogs.org/Get_Started).

<div class="row justify-content-sm-center">
    <div class="col-sm-6 col-md-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/satnogs/16.png" title="Raspberry Pi Image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 col-md-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/satnogs/1.jpg" title="Raspberry Pi Image Config" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="row justify-content-sm-center">
    <div class="col-sm-6 col-md-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/satnogs/2.jpg" title="SatNOGs Software Running" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 col-md-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/satnogs/6.jpg" title="SatNOGs Software Config" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Deployment Challenges

**Logistics Constraints**
Component procurement through Egyptian customs imposed significant timeline impact, with individual shipments requiring 3-4 weeks for customs clearance and inspection. This extended a projected 5-6 week build to 30 weeks due to iterative troubleshooting requiring additional component sourcing. The customs review process, while thorough, created feedback loop delays when hardware revisions became necessary.

**Power Regulation Discovery**
The voltage drop issue emerged only after complete assembly and initial boot attempts. Systematic debugging using precision multimeter measurements across PDU output channels revealed the 0.4V regulation loss, initially misattributed to insufficient current capacity. Attempted resolution through higher amperage 5V supplies (10A rating) failed to address the fundamental voltage regulation constraint, requiring vendor consultation to identify the root cause and appropriate mitigation strategy.

## Reception Performance

Initial observations targeting NOAA weather satellites demonstrate functional RF reception despite omnidirectional antenna limitations and urban interference environment. Waterfall spectrograms show detectable satellite carrier presence during overhead passes, though signal-to-noise ratio remains marginal for reliable demodulation without directional tracking. The system successfully validates the SatNOGs client integration and RF chain functionality as a baseline for future directional antenna upgrades.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/satnogs/12.png" title="NOAA Satellite Reception" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    NOAA satellite pass waterfall showing carrier detection with omnidirectional antenna from 5th floor installation
</div>

Expected omnidirectional performance manifests as anticipated - elevation-dependent signal strength with notable atmospheric attenuation at low angles. The 5th floor mounting position provides clear sky visibility but urban RF noise floor degrades SNR compared to rural installations. 

## System Evolution

Currently working on replacing the antenna with a UFH QFH antenna. In addition, the current implementation establishes foundation infrastructure for rotary system enhancement. The dual PDU architecture specifically accommodates future rotator motor controllers requiring 12V power rails, avoiding redesign of the power distribution subsystem. Mechanical integration within the 4U rack cabinet maintains modularity for component substitution and expansion.

## Resources

**Project Information**
- [Outlyer.space](https://outlyer.space/)
- [SatNOGs Omnidirectional Build Guide](https://wiki.satnogs.org/Omnidirectional_Station_How_To)
- [Progress Documentation](https://studhamza.github.io/hamza-folio/blog/tag/gnuradio/)

**Hardware Components**
- DeskPi RackMate T0 4U Cabinet
- DeskPi DC PDU Lite 7-Channel 0.5U
- 52Pi 4-Channel USB Power Module
- Raspberry Pi 5
- NooElec RTL-SDR
- Omnidirectional VHF/UHF Antenna
- Wideband LNA

---

*Developed during internship with Outlyer.space from February to September 2025. The system demonstrates practical satellite ground station implementation while establishing infrastructure for future rotator-based tracking capability.*
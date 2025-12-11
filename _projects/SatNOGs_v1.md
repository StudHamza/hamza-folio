---
layout: page
title: SatNOGs Ground Station
description: Non-rotary omnidirectional ground station with dual PDU architecture for satellite telemetry reception
img: assets/img/satnogs/8.jpg
importance: 2
category: communication
related_publications: false
---

A complete SatNOGs ground station implementation featuring dual Power Distribution Unit architecture and omnidirectional VHF/UHF reception capability. Built during an internship with Outlyer.space, this system demonstrates practical satellite telemetry acquisition while addressing power delivery constraints in rack-mounted SDR configurations.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/satnogs/8.jpg" title="Complete Ground Station Assembly" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/satnogs/11.png" title="Dual PDU Configuration" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/satnogs/9.png" title="Rack Integration" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Final ground station assembly with DeskPi RackMate 4U cabinet and dual PDU power delivery system
</div>

## System Architecture

The ground station implements a standard SatNOGs omnidirectional reception chain optimized for VHF/UHF satellite telemetry. The RF path consists of an omnidirectional dipole antenna feeding a wideband low-noise amplifier, connected via coaxial cable to a NooElec RTL-SDR receiver interfaced with a Raspberry Pi 5 running SatNOGs client software. All components are integrated into a DeskPi RackMate 10-inch 4U server cabinet with custom power distribution.

The architecture supports future expansion to rotator-based directional tracking through the dual PDU configuration, which separates 5V and 12V power domains while maintaining centralized control. This modular approach enables incremental system upgrades without redesigning the power delivery subsystem.

## Technical Implementation

**RF Reception Chain**
The omnidirectional antenna provides hemispherical coverage with typical 3dBi gain across the VHF/UHF amateur satellite bands. The general-purpose LNA maintains noise figure below 1dB with sufficient gain to overcome SDR receiver noise, critical for low-power satellite downlinks at elevation angles near the horizon. Coaxial losses are minimized through short cable runs within the rack enclosure.

**Power Distribution System**
Initial deployment revealed voltage regulation issues with the DeskPi DC PDU Lite, which exhibits approximately 0.4V drop across output channels due to series protection diodes in each channel. This drop proved incompatible with Raspberry Pi 5 power requirements, causing boot failures despite adequate current capacity. Voltage measurements across PDU outputs confirmed the regulation issue extended beyond simple current limitation.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/satnogs/10.png" title="52Pi USB PDU Module" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    52Pi 4-channel USB power module providing isolated 5V rails for compute and SDR components
</div>

The dual PDU architecture resolves this constraint by cascading a 52Pi 4-channel USB power module with the existing DeskPi PDU. The 52Pi module delivers regulated 5V to the Raspberry Pi and SDR hardware through independent USB channels, while the DeskPi PDU maintains 12V distribution for future motorized components. This topology isolates critical 5V loads from voltage drop effects while preserving the investment in rack infrastructure.

**Mechanical Integration**
Component mounting utilizes DeskPi KL-P24 adapter boards and DP-0039 brackets for secure mechanical attachment within the rack rails. The Raspberry Pi mounts directly to the KL-P24 shelf with proper standoff spacing, while the 52Pi PDU attaches via custom bracket fabrication. Cable management maintains minimum bend radius for coaxial lines and separates power distribution from RF signal paths.

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

Expected omnidirectional performance manifests as anticipated - elevation-dependent signal strength with notable atmospheric attenuation at low angles. The 5th floor mounting position provides clear sky visibility but urban RF noise floor degrades SNR compared to rural installations. Future rotator integration with directional Yagi arrays will address these limitations through increased antenna gain and nulling of terrestrial interference sources.

## System Evolution

The current implementation establishes foundation infrastructure for phased capability enhancement. The dual PDU architecture specifically accommodates future rotator motor controllers requiring 12V power rails, avoiding redesign of the power distribution subsystem. Mechanical integration within the 4U rack cabinet maintains modularity for component substitution and expansion.

Planned enhancements include azimuth/elevation rotator integration with directional UHF/VHF Yagi antennas, automated satellite tracking through SatNOGs scheduling integration, and potential S-band reception capability for higher data rate downlinks. The existing Raspberry Pi 5 compute platform provides sufficient processing headroom for multi-band simultaneous reception and local demodulation tasks.

## Technical Insights

This project demonstrated that seemingly straightforward system integration often reveals subtle hardware constraints during operational deployment. The PDU voltage regulation issue illustrates the importance of validating complete power delivery chains rather than relying solely on component specifications. Series protection elements, while enhancing safety, introduce voltage drops that accumulate across distribution paths.

Rack-mounted SDR configurations require careful attention to power topology design, particularly when mixing voltage domains and protection schemes. The dual PDU approach provides a general solution for isolating critical 5V loads while maintaining flexibility for future expansion requiring alternative voltage rails.

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
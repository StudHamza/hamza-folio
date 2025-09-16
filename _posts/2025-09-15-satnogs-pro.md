---  
layout: post  
title: Building a SatNOGs Ground Station in Egypt  
date: 2025-08-25 1:00:00  
description: A summary of my internship project with Outlyer.space to develop a non-rotary ground station in Egypt.  
tags: satnogs, ground-station, omni-antenna  
categories: outlyer.space  
featured: false  
images:  
compare: true  
slider: true  
---

## Bill of Materials  

| Component | Model / Link | Purpose | Price (USD) |
|-----------|--------------|---------|-------------|
| Antenna | [Bingfu Omni, VHF (136-174 MHz) & UHF (400-470 MHz)](https://www.amazon.com/Bingfu-136-174MHz-400-470MHz-Handheld-Magnetic/dp/B07X2LK2NL) | Receiving satellite signals; suitable for non-rotary builds | 10.00 |
| Low Noise Amplifier (LNA) | [NooElec Inline LNA v2](https://www.nooelec.com/store/lana.html) | Signal amplification | 35.00 |
| SDR Receiver | [NooElec NESDR SMArt v5](https://www.nooelec.com/store/nesdr-smart-sdr.html) | RF signal reception | 37.95 |
| Controller | Raspberry Pi 5 (8 GB) | Running SatNOGs client software | 93.80 |
| Power Distribution Unit (PDU) 1 | [DeskPi DC PDU Lite](https://deskpi.com/products/deskpi-dc-pdu-lite-7-ch-0-5u-for-deskpi-rackmate-t1) | Original PDU for 12 V / mixed loads | 50.00 |
| Power Distribution Unit (PDU) 2 | [52Pi 4-Channel 5 V PDU](https://52pi.com/collections/new-arrivals/products/52pi-4-usb-channel-5v-power-supply-module-for-raspberry-pi-pico-0-91-inch-oled-screen-compatible-with-1u-rack-mounting) | Stable 5 V power for Raspberry Pi and peripherals | 17.50 |
| Rack Mount | [DeskPi RackMate](https://deskpi.com/products/deskpi-rackmate-t0-black-version-rackmount-10-inch-4u-server-cabinet-for-network-servers-audio-and-video-equipment) | Enclosure for station hardware | 80.00 |
| Bracket | [DeskPi DP-0039 Bracket](https://deskpi.com/products/deskpi-rackmate-10-inch-1u-rack-mount-with-2-pcie-nvme-boards-for-raspberry-pi-5-supports-m-2-nvme-ssds/) | Mounts Raspberry Pi + new PDU | 40.00 |
| Display | [GeeekPi 10.1-inch LCD Screen](https://www.amazon.com/dp/B0CPLYD5GD?ref_=cm_sw_r_cp_ud_dp_2EGBYHWQDMQYEVC9GPZV) | Display screen for desktop environment | 69.99 |


## Project Overview  

**Project**: Development of a SatNOGs fixed ground station with two Power Distribution Units (PDUs) to support a future rotator system.  
**Organization**: [Outlyer.space](https://outlyer.space/)  
**Timeline**: February – September 2025 (~30 weeks)  

<div class="row">  
<div class="col-sm mt-3 mt-md-0">  
{% include figure.liquid loading="eager" path="assets/img/satnogs/8.jpg" class="img-fluid rounded z-depth-1" alt='Complete Ground Station' %}  
</div>  
</div>  

---

## Introduction  

This post documents the **final build** of a SatNOGs omnidirectional ground station in Egypt, created during my internship with [Outlyer.space](https://outlyer.space/).  
Originally planned as a 5–6 week project, it evolved into a **30-week effort** due to hardware challenges, shipping delays, and power distribution issues.  

If you’re new to [SatNOGs](https://satnogs.org/), it’s an open-source, global network of satellite ground stations. This project gave me hands-on experience with antennas, SDRs, and ground station operation — as well as navigating Egyptian customs for importing electronics.  

---

## Software Setup  

The software stack follows the SatNOGs deployment model:  

1. **Prepare the Raspberry Pi**  
   - Download the SatNOGs 64-bit stable image from the [SatNOGs Raspberry Pi Image Repository](https://gitlab.com/librespacefoundation/satnogs/satnogs-pi-gen).  
   - Verify the image integrity using SHA256.  
   - Flash the image to the SD card using [Raspberry Pi Imager](https://www.raspberrypi.com/software/https://www.raspberrypi.com/software/):  
     - Select your Raspberry Pi model.  
     - Provide the image path.  
     - Configure Wi-Fi, locale, SSH, and create a user account.  

2. **Initial Boot**  
   - Insert the SD card into the Raspberry Pi and power it on.  
   - Login with the credentials set earlier.  
   - Update the system:  
     - `sudo apt update && sudo apt upgrade -y`  

3. **Install & Configure SatNOGs Client**  
   - Run `sudo satnogs-setup` to start the configuration menu.  
   - Register on [SatNOGs Network](https://network.satnogs.org) and obtain your API token.  
   - In `satnogs-setup`, enter:  
     - **SATNOGS_API_TOKEN**  
     - **SATNOGS_STATION_ID**, LAT, LON, ELEV  
     - **SATNOGS_SOAPY_RX_DEVICE** = `driver=rtlsdr`  
     - **SATNOGS_RX_SAMP_RATE** = `2.048e6`  
     - **SATNOGS_RX_GAIN** (adjust per satellite)  

4. **Gain Calibration**  
   - Connect the SDR and use a tool like CubicSDR to experiment with gain and noise levels:
            

<div class="row">  
<div class="col-sm mt-3 mt-md-0">  
{% include figure.liquid loading="eager" path="assets/img/satnogs/17.png" class="img-fluid rounded z-depth-1" alt="CubicSDR application" %}  
</div>  
</div>  

(*Extra*) **Desktop Enivroment**
    - The SatNOGs images are server optimized with no GUI enviroment. To install a desktop enviroment run:
        - `sudo apt update sudo apt upgrade –y` to update the system
        - `sudo apt install raspberrypi-ui-mods –y` install Raspberry Pi PIXEL Desktop
        - Enable GUI on boot:
            - `sudo raspi-config`
            - Go to System Options > Boot / Auto Login
            - Choose Desktop Autologin or just Desktop

---

## Hardware  

The **final hardware build** integrates dual PDUs to solve earlier power issues:  

- **Antenna**: Omnidirectional, mounted on the building roof.  
- **Signal Chain**: Antenna → LNA → NooElec SDR → Raspberry Pi 5.  
- **Power**:  
  - The original DeskPi PDU remains for 12 V and mixed loads.  
  - The new 52Pi 5 V PDU exclusively powers the Raspberry Pi, LNA, and LCD screen.  
  - Both the Pi and PDU are mounted using the DeskPi DP-0039 bracket for stability and modularity.  

### Assembly  

First, assemble the [DeskPi Rackmount](https://deskpi.com/products/deskpi-rackmate-t0-black-version-rackmount-10-inch-4u-server-cabinet-for-network-servers-audio-and-video-equipment) following the included 14-page manual (6 steps). A YouTube [guide](https://www.youtube.com/watch?v=_yTCz_-Y8ks) is also available, though not necessary due to the straightforward assembly.  

Next, install the [DeskPi Rackmate 10-inch 1U Rack](https://deskpi.com/products/deskpi-rackmate-10-inch-1u-rack-mount-with-2-pcie-nvme-boards-for-raspberry-pi-5-supports-m-2-nvme-ssds?_pos=4&_sid=6a17b54a4&_ss=r), designed for the Raspberry Pi 5. It provides a robust and efficient housing solution for both the [EP-0249 PDU](https://52pi.com/blogs/blog/how-to-use-52pi-4-usb-channel-5v-power-supply-module-in-your-project)(52Pi 4-USB Channel PDU) and Raspberry Pi. Connect the 52Pi 4-USB Channel PDU to power the 5 V components, LCD screen, LNA and Pi.  

For more information about the [EP-0249 PDU](https://wiki.52pi.com/index.php?title=EP-0249), consult the wiki — it includes detailed information on the built-in Raspberry Pi Pico I²C LCD display of the PDU.  


<div class="row">  
    <div class="row">  
        <div class="col-sm mt-3 mt-md-0">  
        {% include figure.liquid loading="eager" path="assets/img/satnogs/18.jpg" class="img-fluid rounded z-depth-1" alt="Pi RackMount" %}  
        </div>  
        <div class="col-sm mt-3 mt-md-0">  
        {% include figure.liquid loading="eager" path="assets/img/satnogs/19.png" class="img-fluid rounded z-depth-1" alt="Pi Rackmount 2" %}  
        </div>  
    </div>  
        <div class="row">  
        <div class="col-sm mt-3 mt-md-0">  
        {% include figure.liquid loading="eager" path="assets/img/satnogs/20.jpg" class="img-fluid rounded z-depth-1" alt="Pi RackMount" %}  
        </div> 
    </div>  
</div>  

Finally assemble the LCD Display and place it on top of the mount, wire everything together and your station is should be up and running. If not checkout the trouble shooting page of the SatNOGS Wiki, [https://wiki.satnogs.org/Troubleshooting](https://wiki.satnogs.org/Troubleshooting).

<div class="row">  
        <div class="col-sm mt-3 mt-md-0">  
        {% include figure.liquid loading="eager" path="assets/img/satnogs/11.jpg" class="img-fluid rounded z-depth-1" alt="Power supply connections" %}  
    </div>  
        <div class="col-sm mt-3 mt-md-0">  
        {% include figure.liquid loading="eager" path="assets/img/satnogs/9.png" class="img-fluid rounded z-depth-1" alt="Final ground station and location" %}  
        </div>  
</div>  

<div class="row">  
        <div class="col-sm mt-3 mt-md-0">  
        {% include figure.liquid loading="eager" path="assets/img/satnogs/13.jpg" class="img-fluid rounded z-depth-1" alt="Wiring" %}  
</div>  
</div>  

This configuration isolates sensitive 5 V components, ensures stable operation, and leaves room for a future **rotator upgrade** without redesigning the power system.  

---

## Challenges & Lessons Learned  

While the **final build** works reliably, the path to get here involved several challenges:  

1. **Shipping Delays**  
   - Importing electronics to Egypt often took weeks per component. This extended the timeline far beyond the initial 6-week plan.  

2. **Power Delivery Issues in the First Build**  
   - The initial setup used only the DeskPi PDU Lite.  
   - Its ~0.4 V drop per channel prevented the Raspberry Pi from booting even with a 10 A adapter.  
   - DeskPi support confirmed it was not intended for 5 V loads, prompting the switch to the 52Pi PDU.  

These lessons underscore the importance of **local sourcing** and **voltage verification** when designing ground stations.  

---

## Performance: NOAA Satellite Reception  

From a 5th-floor urban location, an omnidirectional antenna has limited reception compared to directional or rotator-based systems. However, the station successfully captured faint NOAA signals visible in the waterfall plot below:  

<div class="row">  
<div class="col-sm mt-3 mt-md-0">  
{% include figure.liquid loading="eager" path="assets/img/satnogs/12.png" class="img-fluid rounded z-depth-1" alt="Received signal waterfall" %}  
</div>  
</div>  

---

## Useful Links  

- [Outlyer.space](https://outlyer.space/)  
- [All Weekly Blogs](https://studhamza.github.io/hamza-folio/blog/tag/gnuradio/)  

---

Thanks for reading! Feedback and suggestions from the SatNOGs community are very welcome — especially regarding improving reception from an omnidirectional setup in an urban environment.  

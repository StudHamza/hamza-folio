---
layout: page
title: LoRa Python Receiver & TinyGS Ground Station
description: Pure-Python LoRa PHY decoder (softlora) and an MQTT-connected TinyGS SDR ground station (tinygs)
img: assets/img/lora/lora_packet.png
importance: 1
category: communication
mathjax: true
related_publications: false
---

A complete software LoRa ground station built during Google Summer of Code 2026 with [LibreCube](https://librecube.org). It combines two pure-Python libraries into a single reception chain: [softlora](https://gitlab.com/librecube/lib/python-softlora), a LoRa physical-layer decoder, and [tinygs](https://gitlab.com/librecube/lib/python-tinygs), an MQTT-connected ground station that reports decoded packets to the [TinyGS](https://tinygs.com) network.

## SoftLoRa — the LoRa Python Receiver

[SoftLoRa](https://gitlab.com/librecube/lib/python-softlora) is a pure-Python implementation of the LoRa physical layer, including. It depends only on `numpy` and `scipy`.

```python
from softlora import LoRaDecoder

decoder = LoRaDecoder(sf=10, bw=125_000, fs=125_000, fc=437e6)

for packet in decoder.decode_file("recording.wav"):
    print(packet.payload_text, packet.crc_valid)
```

The decoder was built for satellite downlinks, where signals are weak and Doppler-shifted. Synchronization follows the low-complexity 3-stage algorithm of Xhonneux et al. (2021), which estimates the Carrier Frequency Offset (CFO) and Symbol Timing Offset (STO) by exploiting the known chirp structure of the LoRa preamble.

<figure id="fig-packet">
{% include figure.liquid path="assets/img/lora/lora_packet.png" class="img-fluid rounded z-depth-1" zoomable=true caption="A real LoRa packet received from the Polytech Universe-3 (PU-3) satellite (SF8, 62.5 kHz bandwidth). The preamble, sync word, SFD, header, payload, and CRC regions are all visible. Recording courtesy of IE6ISP." %}
</figure>

Key capabilities:

- **Recordings and live streams** — decodes `.wav`, `.cfile`, `.dat`, and `.bin` files, as well as streaming IQ chunks as they arrive from an SDR.
- **Spreading factors 7–12** — configurable bandwidth, sample rate, and center frequency.
- **Packet metadata** — each `Packet` exposes the payload bytes, UTF-8 text, CRC validity, and an SNR estimate, so you can filter for `crc_valid` packets only.
- **Chase decoding** — rescues weak packets by retrying marginal symbol decisions.

### Performance — AWGN SNR Sweep

The receiver is validated with an end-to-end AWGN Monte Carlo testbench: a clean LoRa packet is generated with the `gr-lora_sdr` GNU Radio TX chain, then the *same* noisy draws are decoded, so only the sync + decode path is measured. Packet-error-rate curves sweep spreading factors 7 through 12.

<figure id="fig-sweep">
{% include figure.liquid path="assets/img/lora/awgn_sweep_sf7-12.png" class="img-fluid rounded z-depth-1" zoomable=true caption="PER vs SNR waterfall for SF7–12 (BW 125 kHz, CR 4/5, CRC on). Each spreading-factor step adds roughly 4 dB of processing gain." %}
</figure>

### Generating Packets with GNU Radio

Packets used for round-trip testing are produced by a [`gr-lora_sdr`](https://github.com/tapparelj/gr-lora_sdr) **TX** flowgraph, [`lora_TX.grc`](https://gitlab.com/librecube/lib/python-softlora/-/blob/main/gnuradio/lora_TX.grc). A message strobe pushes a payload through the LoRa transmit chain and writes the resulting complex IQ to a `.bin` file.

<figure id="fig-tx">
{% include figure.liquid path="assets/img/lora/lora_TX.png" class="img-fluid rounded z-depth-1" zoomable=true caption="The gr-lora_sdr TX flowgraph opened in GNU Radio Companion — SF10, BW 125 kHz, 250 kHz sample rate." %}
</figure>

## TinyGS — the Ground Station

[TinyGS](https://gitlab.com/librecube/lib/python-tinygs) is a Python ground station for the TinyGS satellite network, an unofficial reimplementation of the ESP32 TinyGS firmware. It handles the MQTT-TLS connection to `mqtt.tinygs.com`, receives satellite assignments, and publishes decoded packets to the server.

```bash
pip install tinygs            # the station library + softlora decoder
pip install 'tinygs[sdr]'     # the RTL-SDR Python backend
```

Running a station is three commands:

```bash
tinygs-create    # answer some questions once -> station.json
tinygs-run       # go live: connect, receive, report
tinygs-stop      # go offline
```

The station is radio-agnostic: `RtlSdrRadio` is just an IQ source plus a decoder, so supporting a HackRF, a ZMQ stream, or a recorded file only means swapping the source. A simulator is included, so the whole pipeline can be exercised without hardware.

## How They Connect

`softlora` ships as a dependency of `tinygs`. The station library supplies the IQ samples from the SDR; softlora turns those samples into packets; and the station library publishes the valid payloads back to the TinyGS network. Together they form a single, testable software ground station that runs on any Linux PC attached to an SDR and a band-tuned antenna.

## Resources

**Code & Documentation**
- [python-softlora](https://gitlab.com/librecube/lib/python-softlora) — the LoRa PHY decoder ([PyPI](https://pypi.org/project/softlora/))
- [python-tinygs](https://gitlab.com/librecube/lib/python-tinygs) — the ground station ([PyPI](https://pypi.org/project/tinygs/))

**Related Writing**
- [TinyGS SDR Ground Station setup guide]({% post_url 2026-08-25-tinygs %})
- [LoRa Synchronization Analysis]({% post_url 2026-07-16-lora %})

**Technologies Used**
- Python 3.9+
- NumPy / SciPy
- SoapySDR + RTL-SDR
- MQTT-TLS

---

*Developed as a Google Summer of Code 2026 project with LibreCube, an open source initiative for space and earth exploration systems.*

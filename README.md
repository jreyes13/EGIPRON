# EGIPRON - European GNSS RFI Situational Awareness & Sensor Deployment Platform (v6.0)

[![EUSPA Supported](https://img.shields.io/badge/Supported_by-EUSPA-yellow.svg)](https://egipron.eu/)
[![QASCOM](https://img.shields.io/badge/Hardware-QASCOM_QB--100_/_QB--010-blue.svg)](https://qascom.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Tactical planning, geolocation, and situational awareness platform for Radio Frequency Interference (RFI) monitoring across GNSS signals (Galileo / GPS / EGNOS). Developed in alignment with the European **[EGIPRON](https://egipron.eu/)** initiative supported by **EUSPA** (*European Union Agency for the Space Programme*).

---

## 🛰️ Architecture & Mission Overview

The platform evolves from a simple geometric sensor planner into a comprehensive **Multi-Domain Operations (MDO) Intelligence & Situational Awareness Hub**, fusing dedicated tactical sensors with high-density sensors of opportunity:

```
                          ┌─────────────────────────────────────┐
                          │    EGIPRON v6.0 MISSION CONTROL     │
                          └──────────────────┬──────────────────┘
                                             │
      ┌──────────────────────┬───────────────┴───────────────┬──────────────────────┐
      │                      │                               │                      │
┌─────▼──────────────┐ ┌─────▼──────────────┐          ┌─────▼──────────────┐ ┌─────▼──────────────┐
│   QASCOM SENSORS   │ │    AVIATION ADS-B  │          │    MARITIME AIS    │ │   IGS / EPN CORS   │
│  QB-100 & QB-010   │ │  NACp / NIC Drops  │          │  Spoofing & Alerts │ │   SNR / C/N0 Watch │
└────────────────────┘ └────────────────────┘          └────────────────────┘ └────────────────────┘
      │                      │                               │                      │
      └──────────────────────┴───────────────┬───────────────┴──────────────────────┘
                                             ▼
                             ┌───────────────────────────────┐
                             │    FUSION & GEOLOCATION       │
                             │  Multilateral TDoA & GDOP     │
                             │  Automated EUSPA Reports      │
                             └───────────────────────────────┘
```

---

## 🛠️ The Four Multi-Domain Layers

### 1. Dedicated QASCOM Hardware Sensors
* **QB-100 Professional:**
  * Fixed high-sensitivity monitoring station (-125 dBm).
  * Multi-frequency reception: Galileo E1, E5a, E5b, E6 (High Accuracy Service - HAS), and GPS L1/L2.
  * Sub-nanosecond time synchronization disciplined via GNSS / atomic standards.
  * Multilateral **TDoA (Time Difference of Arrival)** engine with **GDOP (Geometric Dilution of Precision)** contour computation when $\ge 3$ units overlap.
* **QB-010 Tactical Probe:**
  * Ruggedized compact probe (-95 dBm) for rapid deployment on patrol vehicles, masts, or tactical tripods.
  * Power signature analysis (RSS) and rapid on-the-ground emitter localization.
* **Crowdsourced Mobile Node:**
  * Opportunistic mobile sensing leveraging Android Raw GNSS Measurements (-100 dBm).

### 2. Aviation as a Sensor (ADS-B GNSS Integrity Feed)
* Real-time monitoring of commercial aircraft navigation telemetry:
  * **NACp** (*Navigation Accuracy Category for Position*)
  * **NIC** (*Navigation Integrity Category*)
  * **SIL** (*Surveillance Integrity Level*)
* Automatic detection of active ground Jamming / Spoofing clusters when aircraft experience sudden integrity drops (NACp $\le 3$).
* *Reactive Deployment*: One-click action to dispatch a ground tactical QB-010 probe directly underneath affected flight paths.

### 3. Maritime as a Sensor (AIS Maritime Watch)
* Continuous surveillance of vessel traffic in ports, straits, and critical maritime corridors:
  * Transponded GNSS position accuracy flag (**PA**, $\le 10\text{ m}$ vs $> 10\text{ m}$).
  * Automatic detection of **maritime GNSS Spoofing** ("crop circles", circular drift, teleportation to inland coordinates).
  * Kinematic mismatch detection between non-GNSS gyroscopic heading (**HDG**) and GNSS course over ground (**COG**).
* *Port Shield Mode*: Calculates optimal QB-100 station placement on lighthouse and VTS radar towers to shield harbor approaches.

### 4. Permanent Geodetic Networks (IGS / EUREF EPN)
* Integrated European geodetic reference network (Ondrejov GOPE, Wettzell WTZR, Brussels BRUX, Onsala ONSA, Metsahovi METS, Tallinn TALL, Yebes YEBE, San Fernando SFER, etc.).
* Continuous Carrier-to-Noise Ratio (**$C/N_0$ in dB-Hz**) surveillance across multi-frequency bands.
* **Gap Analysis Engine**: Identifies coverage blind spots exceeding 120 km from active geodetic stations to prioritize QB-100 deployments.

---

## 🧮 Radio Frequency Link Budget

Effective sensor coverage is modeled by the physical intersection of tropospheric line-of-sight and free space path loss:

1. **Tropospheric Radio Horizon:**
   $$d_{horizon} = \sqrt{2 \cdot R'_e \cdot h_{rx}} + \sqrt{2 \cdot R'_e \cdot h_{tx}}$$
   Where $R'_e = \frac{4}{3} R_e \approx 8.497 \times 10^6 \text{ m}$ (standard 4/3 effective Earth radius for atmospheric refraction).

2. **Free Space Path Loss (FSPL):**
   $$FSPL(dB) = P_{tx}(dBm) - \text{Sensitivity}_{rx}(dBm)$$
   $$d_{FSPL} = 10^{\frac{FSPL - 20\log_{10}(f) - 20\log_{10}\left(\frac{4\pi}{c}\right)}{20}}$$

3. **Effective Detection Range:**
   $$d_{effective} = \min(d_{horizon}, d_{FSPL})$$

---

## 🚀 Getting Started

The platform is a zero-dependency static web application:

1. Open `index.html` (or `sensor-coverage.html`) directly in any modern browser:
   ```bash
   open index.html
   ```
2. Or start a lightweight local web server:
   ```bash
   python3 -m http.server 8080
   ```
   Then navigate to `http://localhost:8080`.

---

## 📁 Plan File Compatibility

* **JSON Export / Import**: Fully backward compatible with legacy EGIPRON v5.2 deployment plans while supporting the new multi-domain schema of version 6.0.
* **Executive Situation Reports**: One-click generation of formatted operational briefs detailing active threats, network readiness, and recommendations ready to print or save as PDF for EUSPA and national stakeholders.

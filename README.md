# EGIPRON - European GNSS RFI Situational Awareness & Sensor Deployment Platform (v6.0)

[![EUSPA Supported](https://img.shields.io/badge/Supported_by-EUSPA-yellow.svg)](https://egipron.eu/)
[![QASCOM](https://img.shields.io/badge/Hardware-QASCOM_QB--100_/_QB--010-blue.svg)](https://qascom.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Plataforma táctica de planificación, geolocalización y conciencia situacional de interferencias de radiofrecuencia (RFI) en señales GNSS (Galileo / GPS / EGNOS). Desarrollada en el marco de la iniciativa europea **[EGIPRON](https://egipron.eu/)** apoyada por **EUSPA** (*European Union Agency for the Space Programme*).

---

## 🛰️ Visión General y Capacidades

La plataforma evoluciona desde un planificador geométrico hacia un **centro de operaciones de inteligencia multidominio (MDO)**, combinando sensores propietarios con sensores de oportunidad masivos:

```
                          ┌─────────────────────────────────────┐
                          │    EGIPRON v6.0 MISSION CONTROL     │
                          └──────────────────┬──────────────────┘
                                             │
      ┌──────────────────────┬───────────────┴───────────────┬──────────────────────┐
      │                      │                               │                      │
┌─────▼──────────────┐ ┌─────▼──────────────┐          ┌─────▼──────────────┐ ┌─────▼──────────────┐
│  QASCOM SENSORES   │ │   AVIACIÓN ADS-B   │          │    MARÍTIMO AIS    │ │   REDES IGS / EPN  │
│  QB-100 & QB-010   │ │  NACp / NIC Drops  │          │  Spoofing & Alerts │ │   CORS SNR / C/N0  │
└────────────────────┘ └────────────────────┘          └────────────────────┘ └────────────────────┘
      │                      │                               │                      │
      └──────────────────────┴───────────────┬───────────────┴──────────────────────┘
                                             ▼
                             ┌───────────────────────────────┐
                             │  FUSIÓN Y GEOLOCALIZACIÓN     │
                             │  TDoA & GDOP Multilateral     │
                             │  Generación de Informes EUSPA │
                             └───────────────────────────────┘
```

---

## 🛠️ Cuatro Capas de Detección

### 1. Sensores Propietarios QASCOM
* **QB-100 Professional:**
  * Estación fija de alta sensibilidad (-125 dBm).
  * Soporte multifrecuencia: Galileo E1, E5a, E5b, E6 (HAS) y GPS L1/L2.
  * Sincronización temporal sub-nanosegundo disciplinada por GNSS/reloj atómico.
  * Motor multilateral de **TDoA (Time Difference of Arrival)** y cálculo de contorno **GDOP** cuando se despliegan $\ge 3$ unidades.
* **QB-010 Tactical Probe:**
  * Sonda compacta y rugerizada (-95 dBm) para despliegue rápido en vehículos, patrullas o mástiles móviles.
  * Análisis de densidad de potencia (RSS) e interceptación sobre el terreno.
* **Crowdsourced Mobile Node:**
  * Nodos oportunistas basados en smartphone / Android Raw GNSS Measurements (-100 dBm).

### 2. Aviación como Sensor (ADS-B GNSS Integrity Feed)
* Monitorización en tiempo real de los parámetros telemétricos de navegación de aeronaves comerciales:
  * **NACp** (*Navigation Accuracy Category for Position*)
  * **NIC** (*Navigation Integrity Category*)
  * **SIL** (*Surveillance Integrity Level*)
* Detección automática de brotes de Jamming/Spoofing cuando las aeronaves pierden integridad (NACp $\le 3$).
* Herramienta de *despliegue reactivo*: Alerta con botón de un clic para posicionar un QB-010 en tierra bajo el corredor aéreo afectado.

### 3. Marítimo como Sensor (AIS Maritime Watch)
* Análisis de datos AIS en puertos, estrechos y canales de navegación:
  * Bit de precisión de posición (**Position Accuracy flag**, PA=0 o 1).
  * Detección de patrones anómalos de **Spoofing marítimo** ("crop circles", saltos repentinos de coordenadas hacia aeropuertos o tierra firme).
  * Incoherencia cinemática entre rumbo giroscópico no-GNSS (**HDG**) y rumbo de fondo GNSS (**COG**).
* Modo *Port Shield*: cálculo de cobertura para blindar aproximaciones portuarias con estaciones QB-100.

### 4. Redes Geodésicas Permanentes (IGS / EUREF EPN)
* Red de estaciones geodésicas de referencia (Ondrejov GOPE, Wettzell WTZR, Bruselas BRUX, Onsala ONSA, Metsahovi METS, Tallin TALL, etc.).
* Supervisión del ratio portadora-ruido (**C/N0**) en dB-Hz.
* Algoritmo de **Análisis de Huecos (Gap Analysis)** para identificar puntos ciegos entre estaciones geodésicas donde es prioritario ubicar un sensor QASCOM.

---

## 🧮 Modelado Radioeléctrico (RF Budget)

La cobertura efectiva de cada sensor se determina mediante la intersección de dos restricciones físicas:

1. **Horizonte Radioeléctrico Troposférico:**
   $$d_{horizon} = \sqrt{2 \cdot R'_e \cdot h_{rx}} + \sqrt{2 \cdot R'_e \cdot h_{tx}}$$
   Donde $R'_e = \frac{4}{3} R_e \approx 8.497 \times 10^6 \text{ m}$ (modelo estándar de refracción atmosférica troposférica).

2. **Atenuación en Espacio Libre (Free Space Path Loss - FSPL):**
   $$FSPL(dB) = P_{tx}(dBm) - \text{Sensibilidad}_{rx}(dBm)$$
   $$d_{FSPL} = 10^{\frac{FSPL - 20\log_{10}(f) - 20\log_{10}\left(\frac{4\pi}{c}\right)}{20}}$$

3. **Alcance Efectivo:**
   $$d_{efectivo} = \min(d_{horizon}, d_{FSPL})$$

---

## 🚀 Puesta en Marcha

La aplicación es un cliente web estático autosuficiente sin dependencias pesadas de servidor:

1. Abre directamente `index.html` (o `sensor-coverage.html`) en cualquier navegador web moderno:
   ```bash
   open index.html
   ```
2. O levanta un servidor local:
   ```bash
   python3 -m http.server 8080
   ```
   Y accede a `http://localhost:8080`.

---

## 📁 Compatibilidad de Archivos

* **Exportar / Importar JSON:** Plena compatibilidad tanto con los planes de despliegue históricos (versión 5.2) como con los nuevos esquemas multidominio de la versión 6.0.
* **Informes EUSPA:** Generación instantánea de resumen ejecutivo con estadísticas de red, alertas aéreas y marítimas listo para imprimir o guardar en PDF.

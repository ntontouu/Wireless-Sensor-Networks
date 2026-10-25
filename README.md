# 🔋 ESP32-C6 Wireless Sensor Node (ESP-NOW)

[![University](https://img.shields.io/badge/University-University%20of%20Ioannina-blue.svg)](https://www.uoi.gr)
[![Hardware](https://img.shields.io/badge/Hardware-ESP32--C6%20%7C%20DHT11-blueviolet.svg)]()
[![Protocol](https://img.shields.io/badge/Protocol-ESP--NOW-yellow.svg)]()

## 📚 Table of Contents
1. [Overview](#-overview)
2. [Theoretical Background](#-theoretical-background)
3. [Hardware & Wiring](#️-hardware--wiring)
4. [Software Architecture & Optimizations](#-software-architecture--optimizations)
5. [Visualization](#-visualization)
6. [Author & Credits](#-author--credits)

---

## 🔍 Overview
This project implements a low-power, edge-computing wireless sensor node for a Wireless Sensor Network (WSN). Utilizing the modern **RISC-V based ESP32-C6** microcontroller and a **DHT11** environmental sensor, the node records temperature and humidity, transmitting the telemetry via the connectionless **ESP-NOW** protocol to a central Coordinator (Raspberry Pi / Node-RED).

---

## 📐 Theoretical Background
### Why ESP-NOW over Zigbee or Wi-Fi?
While Zigbee excels in large-scale Mesh topologies with multi-hop capabilities, and traditional Wi-Fi provides heavy data throughput, both impose significant overhead (handshakes, routing tables, association requests). 
**ESP-NOW** operates directly at the MAC layer. It is a connectionless protocol that utilizes IEEE 802.11 Vendor Specific Action Frames. This allows the ESP32-C6 to wake up, broadcast data instantly using peer MAC addresses (`FF:FF:FF:FF:FF:FF`), and return to sleep in milliseconds, drastically extending battery life.

---

## 🛠️ Hardware & Wiring
The circuit is cleanly constructed on a breadboard, utilizing the shared power rails to avoid direct cross-wiring:
- **VCC:** Connected to ESP32 `5V` (supplying adequate voltage for the DHT11).
- **GND:** Connected to ESP32 `GND`.
- **DATA:** Connected to ESP32 `GPIO 4` for 1-Wire digital communication.

---

## 💻 Software Architecture & Optimizations
- **Custom Payload Struct:** Data is packaged into a strict `struct_message` containing the Node ID (15), packet counters, float sensor values, RSSI metrics, and a local checksum (`Temp + Hum + Counter`) for data integrity verification by the receiver.
- **Deep Sleep & RTC Memory:** To achieve absolute energy efficiency, the node enters deep sleep after transmission. The packet counter is stored in the `RTC_DATA_ATTR` memory segment, ensuring it survives power-down cycles.
- **Dynamic Sleep Calibration:** The execution time of the code is actively measured (`millis() - start_time`) and subtracted from the 60-second target interval, compensating for clock drift and ensuring a perfectly synchronized telemetry cycle.

---

## 📊 Visualization
Data transmitted by this node is received by an ESP-NOW Coordinator and forwarded to a **Node-RED Dashboard** for real-time visualization of temperature gauges, humidity charts, and packet metrics.

---

## 📝 Author & Academic Use
- **Student:** Maria Alexandra Ntontou (ΜΑΡΙΑ ΑΛΕΞΑΝΔΡΑ ΝΤΟΝΤΟΥ)
- **Student ID:** 2799
- **Institution:** University of Ioannina — Department of Informatics and Telecommunications
- **Course:** Wireless Sensor Networks (Δίκτυα Αισθητήρων)

---
title: "Environmental Monitoring System"
excerpt: "A modular ESP32 and Raspberry Pi system for measuring, storing, visualizing, and threat-modeling apartment environmental data."
featured: true
order: 1
category: "EMBEDDED SYSTEMS"
tags: [ESP32, Raspberry Pi, MQTT, InfluxDB, Grafana, Threat Modeling]
---

> This project documents a personal, isolated lab environment. Sensitive configuration details, credentials, and live endpoints are intentionally omitted.

## Project Goal
- To create a modular system that would allow me to monitor and log temperature, pressure, and humidity in my apartment.

## Equipment
- Waveshare ESP32-C3-Zero (x2)
- BME-280 Temperature, Pressure, and Humidity Sensor (x1)
- BME-680 Temperature, Pressure, Humidity, and Gas Sensor (x1)
- Raspberry Pi 4 B+ —> running ubuntu headless
- Generic 1.3 inch I2C OLED Screen, 128x64 pixels, SH1106 driver
- Generic 0.91 inch I2C OLED Screen, 128x32 pixels, SSD1306 driver

## Data Flow
- First, a note about the network architecture: I set up a separate VLAN for the Environmental Monitoring System. The sensors and the Pi live on this Lab-VLAN. The Lab-VLAN is isolated from other VLANs at the network level and has no internet access under normal operation, with the laptop having access to both the Lab-VLAN and the main VLAN. The Pi gets internet access during short, infrequent maintenance windows. 

1. ESP32 samples sensor every 2 seconds, and averages the data over 10 min
2. ESP32 sends the 10-minute average to the Pi over MQTT
3. Mosquitto MQTT Broker on the Pi receives the data
4. Telegraf takes the data from MQTT and writes it to InfluxDB
5. Grafana pulls data from the database and creates a visualization dashboard
6. Laptop accesses the Grafana dashboard to view the data

## Process
This project began with a question: Can I build a system that would allow me to monitor my environment at home, collect and record data, and be able to visualize the data for a historical analysis. After some preliminary research, the answer was a resounding yes, so I began designing the system. I decided to build two sensor modules to collect data ("Livingroom" and "Bedroom"), and send the data over MQTT to a Raspberry Pi 4 to record and process. 

First, some constraints. The system must be modular and allow for expansion in the future. The sensor module should be no more than 8.0x6.0x6.0 cm. The sensors should have wireless connectivity. The MCU should be compatible with either Arduino IDE, or the Raspberry Pi Pico development environments. 

I compared the various MCU boards offered by Arduino, Espressif, and Raspberry Pi, and chose an Espressif ESP32-C3-Zero board manufactured by Waveshare.  At the time of research, I was unable to find any Arduino board that had as small of a footprint as the ESP32-C3-Zero. Waveshare did offer boards with the RP2040 and the RP2350 MCUs with a similar footprint. When compared to the RP2040 and the RP2350 boards, the ESP32-C3-Zero board had a faster processor at 160 MHz, compared to 133 MHz for the RP2040 and 150 MHz for the RP2350. While the offerings by Raspberry Pi had more cores, that was not a deal-breaker. What was an immediate deal-breaker was that neither of the two Raspberry Pi MCUs had wireless connectivity natively. The ESP32-C3-Zero, on the other hand, supports 2.4 GHz Wi-Fi (802.11 b/g/n) and Bluetooth 5 LE. This board is also compatible with the ESP-IDF development environment and the Arduino IDE. Finally, the ESP32-C3-Zero board is also extremely small, measuring at 18.00 mm by 23.50 mm. 

I then turned my attention to the sensors. Some of the most common sensors (temperature/humidity) in the hobbyist space are: SHT40, BMP/BME280, AHT20/DHT20, HTU21D-F, and BME-680. Here is a quick comparison: 

| **Feature**            | **BME680**                        | **BME280**               | **SHT40**             | **AHT20**             | HTU21D-F              |
| ---------------------- | --------------------------------- | ------------------------ | --------------------- | --------------------- | --------------------- |
| **Sensing Parameters** | Temp, Humidity, Pressure, VOC Gas | Temp, Humidity, Pressure | Temp, Humidity        | Temp, Humidity        | Temp, Humidity        |
| **Humidity Accuracy**  | ±3.0% RH                          | ±3.0% RH                 | **±1.8% RH**          | ±2.0% RH              | ±2.0% RH              |
| **Temp Accuracy**      | ±0.5°C to ±1.0°C                  | ±0.5°C to ±1.0°C         | **±0.2°C**            | ±0.3°C                | ±1.0°C                |
| **Power Consumption**  | Higher (Heater uses ~10-15 mA)    | Ultra-Low (Microamps)    | Ultra-Low (Microamps) | Ultra-Low (Microamps) | Ultra-Low (Microamps) |
| **Interface**          | I2C / SPI                         | I2C / SPI                | I2C                   | I2C                   | I2C                   |

After comparing several offerings, I settled on two sensors: a Bosch BME-280 and a Bosch BME-680 sensor. These sensors have the advantage of measuring barometric pressure in addition to temperature and humidity, thus providing a better, all-in-one solution. The BME-680 sensor has an additional advantage of being able to sense VOC Gas. This provides an option for expanding the capabilities of the system in the future. The BME-280 sensor is used on the "Livingroom" module, and the BME-680 sensor is used on the "Bedroom" module. 

I decided to give both modules a screen. Both displays were Generic OLEDs. For the "Livingroom" module I used a 0.91 inch OLED display with a 128X32 pixel resolution. This display used a SSD1306 I2C driver. For the "Bedroom" module I used a slightly larger 1.3 inch OLED display with a 128X64 pixel resolution. This display used a SH1106 I2C driver. Both displays are very small, and, frankly, I had a few lying around, so I chose to use them instead of buying something new. On these displays, the sensor module prints the current time, and the temperature and humidity readings which are sampled every 2 seconds. 

The last piece of the setup was the central brain, the data recording and processing. I chose a Raspberry Pi 4 B+. To be transparent, I had one lying around, and I'm very comfortable with the ecosystem. On the Pi, I'm using MQTT to collect the data sent by the sensor modules every 10 minutes, Telegraf to record the collected data into InfluxDB, and Grafana to read the recorded data, parse it, and visualize the data. 

## Threat Model
1. **Assets**
	- Sensor data - this is what I'm collecting and most interested in, so the integrity and confidentiality of the data is important for later analysis to be accurate
	- Pi - stores, processes, and visualizes the data; tampering with the Pi could lead to malicious destruction of data, and malicious code execution
	- Stored credentials and configuration data - compromise of the credentials and data can lead to unauthorized network access, enumeration, and further movement
	- Laptop - has access to the internet through the gateway, stores secure keys, has access to both VLANs, if compromised, could lead to the compromise of the entire system
2. **Adversary**
	- Following the idea that any device can be compromised at a point in time, some potential adversaries are: 
		- An attacker with physical access - the standard door lock is the only physical control in place, this is accepted as adequate for the threat model's scope
		- A compromised laptop on the VLAN or bridging the two VLANs
		- Any device (phone, tablet, other SBCs, Virtual Machines, other laptops) on my main VLAN that could be compromised and serve as ground-zero, leading to the compromise of my laptop, and subsequently, the Lab-VLAN
	- Out of scope: direct, internet-facing attacks against the sensors and the Pi, except during infrequent maintenance windows when the Pi is granted temporary internet access for patching
3. **Trust Boundaries**
	- Sensor - broker —> an attacker can publish data, provided they intercept a packet containing auth data (e.g. MITM attack), they can also spoof the broker IP and impersonate the broker
	- Broker (mosquitto), telegraf, influxdb, grafana —> all run on the same device (Pi), a compromise of one of the services can allow for lateral movement on the Pi 
	- Pi - LAN  and sensor - LAN  —> these devices are on their own, isolated VLAN with no internet access, the isolation is on the network level 
	- Laptop - Lab-VLAN and Laptop-Main-VLAN —> the laptop bridges both VLANs, so while from the point of view of the router the VLANs are isolated, from the point of view of the laptop they are not, the isolation guarantee hinges on the laptop's OS-level configuration and the assumption that the laptop is never compromised

## STRIDE Analysis
1. **S**:  ***Spoofing***
	- The broker IP could be spoofed. The ESP32 does not verify the identity of the broker. 
	- Sensor identity could be spoofed. The broker does not verify the identity of the sensor
	- Attacker compromises laptop and inherits trust on the Lab-VLAN, and can now abuse the trust for malicious purposes
	- Password-based authentication on SSH
2. **T**: ***Tampering*** 
	- Default TCP port (MQTT: 1883 and Grafana: 3000) , which is unencrypted. The authentication only gates the initial connection, it does not ensure the integrity of the data. 
	- Attacker with physical access can reflash malicious firmware to the sensors
	- Attacker with physical access can replace microSD card in the Pi with one running malicious software
3. **R**: ***Repudiation***
	- Even with logging enabled, because the sensor identity is not cryptographically verified, there is no way to prove that a given log entry came from a real device or something spoofing it
4. **I**: ***Information Disclosure***
	- Default mqtt tcp port 1883, so the data is unencrypted 
	- Default grafana port 3000 is unencrypted
	- An attacker could also extract firmware if they have physical access to the device and expose hardcoded IPs, credentials, etc. 
5. **D**: ***Denial of Service***
	- Pi is not internet facing, but DoS can happen on the VLAN if an attacker gets access to it
	- An attacker can physically power off the sensors or the Pi if they get access to my apartment
6. **E**: ***Elevation of Privilege***
	- Mosquitto, telegraf, influxdb, grafana are all run by their own service accounts, which do not have sudo permissions; however this does not guarantee that there is no possible lateral movement or privilege escalation path

## Threat Mitigation
1. **Spoofing:** 
	- Use mTLS and  X.509 client certificates to verify the identity of the MQTT broker and the sensors
	- Harden security on the laptop and make sure it's up to date
	- Limit the laptop access on the Lab-VLAN to grafana (port 3000) for viewing data
	- Use key-based instead of password-based authentication on the Pi
2. **Tampering:** 
	- Use the MQTTS port (8883) - protect the data cryptographically
	- Firmware reflashing is an accepted risk at this point, because enabling secure boot on the ESP32-C3-Zero boards requires physical modification to the boards. 
	- Secure boot on the Raspberry Pi 4 is also irreversible, and therefore outside the scope of this project
3. **Repudiation:** 
	- Cryptographically verify the identity of the sensors
	- Use asymmetric keys instead of password-based authentication
4. **Information Disclosure:**  
	- Use the MQTTS port (8883)
	- Use TLS/HTTPS for Grafana 
	- Physical (firmware extraction) risk accepted at this stage
5. **Denial of Service:**  
	- DoS on the VLAN requires an attacker to have already compromised the laptop or gained physical Lab-VLAN access
	- Physical power off is an accepted risk under the same acceptance as the rest of the document
6. **Elevation of Privilege:**  
	- Containerise the system - use docker to separate the services from the system

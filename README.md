# 🌡️ Autonomous Temperature-Controlled Fan Speed Regulator

An industrial-grade, zero-latency temperature-controlled fan speed regulator utilizing a **100% discrete analog closed-loop** (LM35, LM358, NE555) augmented with a **passive Arduino-based I2C OLED telemetry layer**.

---

## 📌 Project Overview
Conventional cooling systems in power electronics and computing enclosures rely on either constant full-speed operation or abrupt electromechanical relay switching. These traditional approaches cause excessive power consumption, high acoustic noise, accelerated mechanical bearing wear, and severe thermal shock. 

Furthermore, relying entirely on digital microcontrollers (MCUs) for mission-critical thermal regulation introduces software-dependent vulnerabilities such as boot-time latency, firmware crashes, and electromagnetic interference (EMI) lockups. 

**This project solves these issues by implementing a hybrid architecture:**
1. **Analog Core (Active Cooling):** A discrete analog loop generates a proportional PWM signal that directly drives a power MOSFET to regulate fan speed with absolutely zero software latency.
2. **Digital Telemetry (Passive Monitoring):** An Arduino Uno passively reads the analog sensor voltage via a precision divider to display real-time status on an OLED, completely decoupled from the main hardware control loop.

---

## ✨ Key Features
* **Zero-Latency Response:** Pure analog signal processing ensures immediate fan acceleration as temperatures rise.
* **Proportional PWM Control:** Eliminates sudden inrush currents and thermal shock by smoothly scaling fan RPM (0 to 16,000 RPM) rather than abruptly toggling power.
* **Fail-Safe Robustness:** The core cooling loop operates completely independently of the Arduino. If the microcontroller crashes or loses power, the fan continues to regulate temperature automatically.
* **Passive Digital Telemetry:** Real-time environmental monitoring (`ROOM: COOL`, `ROOM: HOT`, `FAN: OFF`) displayed on a 0.96" OLED UI.
* **Hardware Safety Override:** An integrated tactile push-button allows operators to instantly drop the MOSFET gate voltage to 0V, halting the motor for emergency maintenance.

---

## 🛠️ Hardware & Software Components
### Core Analog Control Loop (Dot PCB)
* **Temperature Sensor:** LM35DZ Precision Analog IC
* **Signal Conditioning:** LM358N Dual Operational Amplifier
* **PWM Generator:** NE555P Precision Timer
* **Power Switching:** IRFZ44N N-Channel Power MOSFET
* **Protection:** 1N4007 Flyback Clamp Diode
* **Voltage Regulation:** LM2596 DC-DC Buck Converter (12V to 5V)

### Passive Telemetry Layer
* **Microcontroller:** Arduino Uno R3 (ATmega328P)
* **Display UI:** 0.96-inch I2C Monochrome OLED (SSD1306)
* **Software IDE:** Arduino IDE (C++) using `Adafruit_GFX` and `Adafruit_SSD1306` libraries.

---

## ⚙️ System Methodology
1. **Thermal Acquisition:** The LM35 sensor continuously monitors ambient heat, outputting 10mV/°C.
2. **Carrier Generation:** The NE555 Timer operates in astable mode, generating a constant 1kHz triangle carrier wave.
3. **Signal Comparison:** The LM358 Op-Amp compares the LM35 sensor voltage against the reference threshold (set at 30.0°C).
4. **PWM Modulation:** As temperature rises, the Op-Amp modulates the carrier wave into a variable Pulse-Width Modulated (PWM) square wave.
5. **Motor Actuation:** The PWM signal drives the gate of the IRFZ44N MOSFET, dynamically scaling the 12V cooling fan's RPM.
6. **Telemetry Sync:** The Arduino Uno reads the analog voltage and updates the OLED display without interrupting the primary analog loop.

---

## 📂 Repository Structure
* `/code` - Contains the `fan_controller.ino` Arduino firmware for the OLED telemetry.
* `/hardware` - Contains the circuit diagrams, block diagrams, and hardware prototype photos.
* `/docs` - Contains the IEEE-formatted technical project report and presentation slide deck.

---

## 👨‍💻 Developer
Developed by **Giridharun S** 
*Electronics and Communication Engineering*

*Feel free to star ⭐ this repository if you found it useful!*

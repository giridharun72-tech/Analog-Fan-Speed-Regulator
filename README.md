🌡️ Autonomous Temperature-Controlled Fan Speed Regulator

An autonomous, zero-latency temperature-controlled fan speed regulator utilizing a 100% discrete analog closed-loop with a passive Arduino-based I²C OLED telemetry layer.

This project bridges the gap between fail-safe industrial analog hardware and modern digital monitoring, ensuring that critical thermal management is never compromised by software latency, boot times, or MCU crashes.

🎯 Problem Statement

Conventional cooling systems in industrial equipment and computing enclosures rely on constant full-speed operation or abrupt relay switching. This causes:

Excessive Power Waste: Fans running at 100% when not needed.

Mechanical Degradation: Accelerated bearing wear and high acoustic noise.

Thermal Shock: Sudden temperature gradients caused by ON/OFF relays.

Software Vulnerability: Using microcontrollers for primary cooling introduces single points of failure (firmware crashes, EMI interference, and boot-up delays).

💡 Proposed Solution

This system implements a Hybrid Control Architecture:

Analog Core (Active Cooling): A purely discrete analog control loop (LM35 + LM358 + NE555) continuously generates a proportional Pulse-Width Modulated (PWM) signal. It directly drives a power MOSFET to smoothly scale fan speed based on real-time heat, operating with zero software latency.

Digital Telemetry (Passive Monitoring): An Arduino Uno safely reads the analog sensor voltage via a precision divider network. It displays real-time data on an OLED screen without ever interrupting or governing the primary analog hardware.

✨ Key Features

Zero Firmware Latency: Instantaneous analog reaction to temperature spikes.

Proportional PWM Scaling: Smooth fan acceleration prevents abrupt inrush currents.

Fail-Safe Robustness: The cooling loop operates completely independently of the Arduino.

Zero-Power Standby: Fan draws 0A when ambient temperature is below the 30°C threshold.

Hardware Safety Interrupt: A manual 2-pin push button instantly halts the motor via hardware grounding.

⚙️ Methodology & Flow

Thermal Sensing: The LM35 sensor outputs an analog voltage proportional to ambient heat (10mV/°C).

Amplification & Comparison: The LM358 Op-Amp compares the thermal voltage against a manually tuned threshold and an astable triangle wave.

PWM Generation: The NE555 timer establishes a 1kHz carrier wave, which the Op-Amp modulates into a variable square wave.

Power Drive: The IRFZ44N MOSFET drives the 12V DC fan based on the duty cycle, while a 1N4007 diode clamps inductive flyback spikes.

Passive UI: The Arduino Uno maps the analog voltage to digital telemetry, displaying status (ROOM: COOL, ROOM: HOT, FAN: OFF) on an I²C OLED.

🧰 Hardware & Components

Sensors & Logic: LM35 (Temperature Sensor), LM358 (Dual Op-Amp), NE555 (Precision Timer IC)

Power Switching: IRFZ44N (N-Channel Power MOSFET), 1N4007 (Flyback Diode)

Actuation: 12V DC Brushless Cooling Fan

Digital Telemetry: Arduino Uno R3, 0.96" SSD1306 I²C OLED Display

Power Regulation: 12V 2A DC Supply, LM2596 Buck Converter (12V to 5V)

📂 Repository Structure

/code - Contains the Arduino firmware (.ino) for the passive OLED telemetry monitor.

/hardware - Contains the system block diagram, circuit schematics, and prototype photos.

/docs - Contains the comprehensive IEEE-formatted project report and presentation slides.

🚀 Future Scope

SMD PCB Fabrication: Consolidating the dot PCB circuit into an ultra-compact, double-sided Surface Mount Device (SMD) layout.

IoT Telemetry: Upgrading the Arduino to an ESP32 for Wi-Fi cloud data logging and predictive maintenance alerts.

Closed-Loop RPM Regulation: Implementing optical tachometer feedback to stabilize motor RPM against dust buildup or bearing aging.

# 🐍 VyperBot
### WiFi Laser-Tag Robot Platform for Embedded Systems Workshops

![ESP8266](https://img.shields.io/badge/Controller-ESP8266-blue)
![Arduino](https://img.shields.io/badge/Firmware-Arduino-green)
![UDP](https://img.shields.io/badge/Networking-UDP-orange)

**VyperBot** is a small WiFi-controlled robotics platform designed for teaching **embedded systems, networking, and robotics**.

The robot uses an **ESP8266 NodeMCU**, receives joystick commands via **UDP**, detects **laser hits using an LDR**, and sends scores to a **live leaderboard server**.

This repository is used in a **hands-on robotics workshop** where participants learn how to test hardware, understand firmware, and run the complete robot system.

---

# 📦 Repository Structure

This project is organized using **Git submodules**.

```
VyperBot/
│
├── firmware/        ESP8266 robot firmware (submodule)
├── leaderboard/     Web leaderboard server (submodule)
├── joystick/        Mobile joystick controller app (submodule)
│
└── README.md
```

---

# 🤖 Hardware Overview

Main robot components:

- **ESP8266 NodeMCU**
- **TB6612 Motor Driver**
- **2x DC Gear Motors**
- **LDR Sensor (laser hit detection)**
- **LiPo Battery**

The robot uses **differential drive** for movement.

---


# 🕹️ Robot Control

The robot receives **UDP joystick packets** in the format:

```
X,Y
```

Example:

```
128,200
```

Where:

- **X** controls turning
- **Y** controls forward / reverse speed

These values are converted into **differential motor speeds**.

---

# 🔫 Laser Hit Detection

An **LDR sensor** detects when the robot is hit by a laser.

When triggered:

1. A **score packet** is sent to the leaderboard server
2. The robot enters a **10 second lockout period**

### Lockout Behaviour

| Time | Behaviour |
|-----|-----------|
0 – 5 seconds | Robot frozen |
5 – 10 seconds | Robot moves but cannot score |

This prevents multiple hits being counted instantly.

---

# 🏗️ System Architecture

```
Joystick App
     │
     │ UDP packets
     ▼
ESP8266 Robot Firmware
     │
     │ Score packet
     ▼
Leaderboard Server
     │
     ▼
Live Score Display
```

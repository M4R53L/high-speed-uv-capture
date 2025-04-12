# 📸 High-Speed UV-Triggered Polymerization Capture System

## 🎯 Project Summary

This embedded system captures and visualizes a **polymerization reaction** triggered by a **UV light flash**. The reaction transforms liquid into solid in under **100ms**. This system:

- Detects the UV flash using a sensor  
- Captures a high-speed video burst using a **global shutter camera (OV7251)**  
- Displays the process in **slow motion (2–4 seconds)** on a standard HDMI screen  
- Uses **physical buttons** to control playback (Play / Pause / Reset)  
- Prioritizes **C/C++ implementation** for performance and control  
- Designed to be **low-cost, modular, and embedded**

---

## 🧠 System Architecture

```
[ UV Flash (external) ]
          ↓
   [ UV Sensor (GUVA-S12SD) ]
          ↓
   [ Raspberry Pi Pico ]
     ├ Detects flash via ADC
     ├ Sends GPIO signal to Raspberry Pi 5
     └ (Optional) Pulses LED strobe

          ↓
[ Raspberry Pi 5 + OV7251 ]
  ├ Captures 100ms high-speed burst via libcamera
  ├ Buffers 10–20 grayscale frames to RAM
  ├ Displays playback on HDMI
  └ Handles button inputs (Play, Pause, Reset)
```

---

## 🔩 Hardware Components

| Component                  | Function                                  |
|----------------------------|-------------------------------------------|
| Raspberry Pi 5             | Main processor for image capture/playback |
| OV7251 Global Shutter Camera | High-speed, distortion-free imaging     |
| Raspberry Pi Pico          | Real-time UV detection + trigger signaling |
| GUVA-S12SD UV Sensor       | Analog UV light detection sensor          |
| Physical Buttons (x3)      | User input (Play, Pause, Reset)           |
| HDMI Display               | Playback monitor                          |
| High-power UV LED (optional) | External light source for testing       |
| Breadboard + Wires         | Prototyping connections                   |
| Power Supply (5V 3A+)      | Power for Pi 5 and camera                 |

---

## 📁 Repository Structure

```
📁 high-speed-uv-capture/
├── pico_firmware/               ← C code for Pi Pico (UV detection + GPIO)
│   ├── uv_trigger.c
│   ├── led_strobe.c             (optional)
│   └── Makefile
├── pi_software/                 ← C/C++ code for Raspberry Pi 5
│   ├── capture_controller.c     ← libcamera-based frame capture
│   ├── playback_controller.c    ← slow-motion frame display logic
│   ├── gpio_input.c             ← Button control handler
│   └── Makefile
├── camera_config/               ← OV7251 + libcamera configuration
│   ├── dt_overlay.dts
│   └── libcamera_modes.txt
├── captured_frames/             ← Temporary storage of captured frame data
├── docs/                        ← Diagrams, hardware notes
│   ├── system_architecture.md
│   └── wiring_diagram.pdf
├── images/                      ← Output frames or demo photos
├── LICENSE
└── README.md
```

---

## 🛠️ Build & Run Instructions

### Raspberry Pi Pico

```bash
cd pico_firmware
make flash
```

> Uses Pico SDK, builds firmware to detect UV and send GPIO trigger

---

### Raspberry Pi 5

```bash
cd pi_software
make

sudo ./capture_controller
# Waits for GPIO trigger from Pico and captures burst

sudo ./playback_controller
# Handles frame playback via framebuffer or SDL
```

---

## 🎛️ Button Functions (Pi 5 GPIO Inputs)

| Button   | Function           |
|----------|--------------------|
| Button 1 | Play Playback      |
| Button 2 | Pause Playback     |
| Button 3 | Reset / Arm again  |

---

## ✅ Roadmap

- [x] Build UV sensor trigger using Pico  
- [x] Capture 100ms burst using OV7251 + libcamera  
- [x] Buffer frames in RAM  
- [x] Display playback on HDMI monitor  
- [ ] Implement UI button logic in C  
- [ ] Optional: LED strobe control (frame sync)  
- [ ] Add video export or frame save  
- [ ] Build 3D-printed enclosure for final mount  

---

## 📷 Demo Output (Coming Soon)

> Add sample captured frames or playback video here after testing the full pipeline.

---

## 📜 License

MIT License

# V30 Sensor Stabilizer Core [Embedded Library]

**A deterministic, zero-lag signal recovery library for STM32, ESP32, and ARM Cortex-M.**

![V30 Benchmark](sensor_v30_final.png)
*(Blue: Raw noisy sensor input | Red: V30 Filtered output)*

## 🛑 The Problem
If you are building flight controllers, medical ventilators, or balancing robots, you know the struggle:
*   **Low-Pass Filters** introduce phase lag (your drone drifts).
*   **Kalman Filters** are computationally heavy and hard to tune for non-linear noise.
*   **Moving Averages** destroy signal dynamics.

## ⚡ The Solution: V30 Core
V30 is not an AI/Neural Network. It is a **physics-informed hybrid filter** that uses a statistical detector to switch between "Fast Tracking" and "Noise Suppression" modes in real-time.

It acts like a **Noise Gate for physics**:
*   **Idle state:** Clamps noise to absolute zero.
*   **Active state:** Tracks signal with < 5 samples lag.

### Key Specs (Validated on STM32F103):
| Metric | V30 Core | Standard Kalman |
| :--- | :--- | :--- |
| **Phase Lag** | **< 5 ms** | ~40 ms |
| **Noise Rejection** | **-30 dB** | -15 dB |
| **RAM Usage** | **60 bytes** | > 200 bytes |
| **Flash Size** | **1.9 KB** | Varies |
| **Math** | **Floating Point** | Matrix Math |

## 📦 How to Use

The library is distributed as a pre-compiled static binary (`.a` for GCC) to ensure optimization and IP protection.

### 1. Include Header
```c
#include "v30_core.h"

V30_Filter filter;
```

### 2. Initialize
```c
// Run once at startup
V30_Init(&filter);
```

### 3. Process Loop (100Hz - 1kHz)
```c
float raw_data = read_sensor(); // Accelerometer, Flow, Pressure
float clean_data = V30_Process(&filter, raw_data);
```

---

## 📥 Download

This repository contains the header files and documentation.
The **compiled library (`libv30_cortex.a`)** is available for download here:

👉 **[Download V30 Core Library (Gumroad)]([[https://polynet-lab.gumroad.com/l/v30-core])**

*License: Royalty-free for personal and small-scale commercial use (<1000 units).*

---

## 🔧 Compatibility
*   **STM32** (F0, F1, F3, F4, G4, H7)
*   **ESP32** (S3, C3)
*   **nRF52** Series
*   Any MCU with FPU support (Cortex-M4F recommended for best performance).

## 📊 Methodology
The V30 algorithm was validated against **ISO 80601-2-12** standards for medical respiratory sensors, achieving a MAPE (Mean Absolute Percentage Error) of **< 5%** on signals with 50% noise contamination.

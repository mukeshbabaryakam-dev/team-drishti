# DRISHTI-V1: Edge AI Highway Infrastructure Surveyor

<div align="center">
  <h3>Transforming NHAI Patrol Vehicles into Autonomous Safety Scanners</h3>
</div>

---

## 🎯 The Paradigm Shift
Current National Highway inspections rely on imported handheld retroreflectometers (e.g., Delta LTL) costing upwards of **₹5,00,000 per unit**. These legacy systems require perilous manual lane closures, incur massive labor costs, and yield highly localized, sporadic data. 

**DRISHTI-V1 replaces the act of stopping entirely.** By utilizing a magnetically mounted, ₹4,934 edge-computing IoT node, we transform existing NHAI Highway Patrol vehicles into continuous survey nodes. The entire 1,44,000 km national highway network can be surveyed weekly for ₹19 lakhs in total hardware capital—less than the procurement cost of four legacy devices.

---

## ⚙️ Core Engineering Pillars

### 1. Hardware Viability (HaaS)
* **Microcontroller:** ESP32-CAM (32-bit Tensilica LX6 dual-core).
* **Sensors:** TSL2591 (High Dynamic Range Lux), NEO-6M GPS.
* **Optics:** Dual 3W 6000K highly-collimated LEDs + 850nm IR array.
* **Kinematics:** 40kg magnetic quick-release chassis with integrated IMU auto-leveling.
* **Total BoM:** ₹4,934 per node.

### 2. AI-Assisted Sensor Fusion
Raw hardware sensors are intrinsically context-blind. DRISHTI-V1 pairs physical TSL2591 optics with a **YOLOv8-nano** vision pipeline fine-tuned on authentic Indian National Highway environments.
* The AI dynamically classifies targets at 15 FPS (e.g., `centre_line` vs `highway_sign`).
* It automatically applies distinct mathematical calibration geometries (IRC 35 for flat roads, IRC 67 for vertical signs).
* **Validation mAP@0.5:** 84%.

### 3. Adversarial Robustness
Engineered to survive the realities of Indian highways:
* **Vibration Drift:** IMU continuously tracks chassis pitch. Software mathematically corrects for angular deviations at 80 km/h.
* **Lens Occlusion:** AI sharpness/contrast heuristic prevents false failures from mud, automatically flagging `NODE_MAINTENANCE` instead of `REPAINT_REQUIRED`.
* **Thermal Runaway:** Dynamic inference scaling drops AI loads from 15 FPS to 5 FPS if ambient heat exceeds 42°C, preventing hardware failure.

### 4. Privacy By Design
Continuous video feeds of civilian highways raise severe surveillance concerns. DRISHTI-V1 processes all visual data **exclusively at the edge**. 
* Raw camera frames are purged instantly post-inference.
* Zero civilian faces or license plates are transmitted.
* Only lightweight, anonymized JSON payloads are pushed to the NHAI Data Lake.

---

## 📊 Live Interactive Simulator
To visualize the sensor fusion and JSON payload generation at 80 km/h, run the interactive Edge Simulator:

👉 **[Launch DRISHTI-V1 Simulator](https://[your-username].github.io/team-drishti/)**

*(Note: Replace `[your-username]` with your actual GitHub username in the link above before committing).*

---

## 🗄️ Standard Data Lake Payload (JSON)
```json
{
  "timestamp": "2026-04-18T11:45:00Z",
  "device_id": "DRISHTI-V1-EDGE-001",
  "gps_telemetry": {
    "lat": 28.6139,
    "lon": 77.2090,
    "velocity_kmh": 82.4
  },
  "kinematics": {
    "z_axis_altitude": "1.5m_Fixed",
    "LiDAR_correction": "N/A"
  },
  "yolov8_sensor_fusion": {
    "target_classification": "CENTRE_LINE",
    "differential_lux_net": 132,
    "RL_mcd_lx_m2": 145
  },
  "nhai_compliance_flag": "COMPLIANT",
  "atmospheric_condition": "CLEAR"
}

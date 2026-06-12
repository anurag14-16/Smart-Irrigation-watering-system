# Smart Irrigation / Water Plant System 🌱

An automated smart irrigation system built using Arduino Uno, DHT22 sensor, potentiometer (simulating soil moisture), relay module, pump indicator LED, and I2C LCD — simulated on Wokwi.

---

## Overview

Traditional irrigation is either manual or timer-based — both waste water by ignoring actual soil conditions. This system solves that by continuously monitoring soil moisture and automatically turning the water pump ON only when the soil is dry, and OFF when it is sufficiently wet.

A DHT22 sensor provides real-time temperature and humidity readings displayed alongside pump status on an I2C LCD, giving a complete picture of the plant's environment.

---

## Components Used

| Component | Description |
|---|---|
| Arduino Uno | Main microcontroller |
| Potentiometer | Simulates soil moisture sensor (variable analog output) |
| DHT22 Sensor | Measures ambient temperature and humidity |
| Relay Module | Electrically controlled switch for the water pump |
| LED (Red) | Visual pump ON/OFF indicator |
| Resistor 220Ω | Current limiting resistor for LED |
| I2C LCD (16x2) | Displays moisture status, pump state, temperature, humidity |

---

## Working Principle

1. The potentiometer simulates a soil moisture sensor by producing a variable analog voltage (0–5V), read by Arduino's ADC on pin A0 as a value between 0–1023
2. The DHT22 reads ambient temperature and humidity every 2 seconds
3. Arduino applies hysteresis-based decision logic:
   - Moisture value **above 600** → soil is DRY → relay activates → pump LED turns ON
   - Moisture value **below 400** → soil is WET → relay deactivates → pump LED turns OFF
   - Moisture value **between 400–600** → MOIST → current pump state is maintained (no change)
4. The relay acts as an electrically controlled switch — Arduino's 5V signal controls the coil, which switches the pump circuit independently
5. The I2C LCD updates every cycle showing moisture status, pump state, temperature, and humidity
6. Serial monitor outputs full system status for debugging

---

## Simulation

🔗 [Open in Wokwi](https://wokwi.com/projects/466606278310548481)

**How to test in simulation:**
- Turn potentiometer **towards maximum** → simulates dry soil → pump LED turns ON
- Turn potentiometer **to middle** → simulates moist soil → pump holds current state
- Turn potentiometer **towards minimum** → simulates wet soil → pump LED turns OFF

---

## Code Highlights

- **Hysteresis logic** — two separate thresholds (600 for dry, 400 for wet) prevent pump flickering around the boundary value
- **`getMoistureStatus()`** — converts raw ADC value into a human-readable label (DRY / MOIST / WET)
- **`isnan()` validation** — skips the cycle if DHT22 returns an invalid reading, preventing garbage values from affecting decisions
- **`pumpState` global bool** — persists pump state across loop cycles, enabling hysteresis to work correctly
- **Relay + LED mirrored** — both controlled simultaneously so visual indicator always matches relay state

---

## Features

- Automatic pump ON/OFF based on soil moisture
- Hysteresis logic preventing pump flickering
- DHT22 temperature and humidity monitoring
- I2C LCD real-time status display
- Relay module for safe pump switching
- LED pump indicator for visual feedback
- Serial monitor output for debugging

---

## Future Improvements

- Integrate **ESP32 with OpenWeatherMap API** to fetch rain forecast data — skip watering if rain is expected within a few hours even when soil is dry
- Add a **Blynk IoT dashboard** for remote monitoring of moisture levels, temperature, humidity, and pump status from a smartphone
- Replace potentiometer with an actual **capacitive soil moisture sensor** on real hardware for more accurate and corrosion-resistant readings
- Implement **data logging** — store moisture and pump activity history for trend analysis

---

## Author

Anurag

B.Sc (H) Electronics

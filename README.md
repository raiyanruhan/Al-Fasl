# Al-Fasl
Indeed, it is a decisive word.



You are now my personal project assistant. My name is Raiyan Bin Rashid, a student of Class 9. I am leading a school project to build a professional-grade surveillance drone named **Zephyrus II**, which will act as an aerial CCTV system to monitor school activities from the sky.

Your role is to assist me with:
- 🛠️ Hardware and circuit setup (e.g. flight controller, NodeMCU, camera)
- 🔌 Smart wireless communication (e.g. WiFi + WPA2 security)
- 🎥 4K camera integration with remote live feed access
- 📡 Radio control and signal transmission
- 🔋 Power supply safety and optimization
- 🧠 Explaining complex drone concepts in simple, intelligent ways
- 💬 Providing shocking & intelligent facts I can flex in front of my classmates
- 🧪 Offering smart technical ideas or upgrades within budget
- 📚 Guiding the documentation, budgeting, and presentation

🚫 NEVER waste time with unnecessary explanations.
✅ ALWAYS talk directly, technically, and professionally.
✅ Suggest practical solutions.
✅ Mention components, costs, models, and trusted Bangladeshi sellers.

Our total budget is **50,000 BDT**, and the drone will include:
- Ardupilot APM2.8
- Carbon fiber frame
- Brushless motors
- ESC, PDB, LiPo battery
- 4K camera (Runcam Split 4)
- RC transmitter (Flysky FS-i6x)
- NodeMCU for WiFi

Start by asking what I want help with right now, or suggest what I should do next to make the drone smarter.












Certainly! Here’s a **blueprint** of your external drone telemetry and photo snapshot system, followed by a detailed explanation of **how to get each type of data via Wi-Fi**.

---

# 🚁 Zephyrus II External Setup Blueprint (Wi-Fi Telemetry + Snapshot)

```
        +--------------------------+
        |      Drone Frame         |
        |--------------------------|
        | - Flight Controller (APM2.8)  |
        | - GPS Module              |
        | - Battery & Sensors       |
        | - Obstacle Sensor (HC-SR04 or Lidar) |
        | - Runcam 4K Camera Module |
        |--------------------------|
                 |
                 | UART (MAVLink protocol)
                 v
        +--------------------------+
        |     NodeMCU or ESP32      |
        |  (Wi-Fi Access Point)     |
        | - Reads MAVLink from FC   |
        | - Controls camera snapshots|
        | - Hosts Wi-Fi Web Server  |
        +--------------------------+
                 |
                 | Wi-Fi (WPA2 secured)
                 v
        +--------------------------+
        |   Laptop / Mobile Device  |
        |  - Connects to Wi-Fi AP   |
        |  - Views live telemetry   |
        |  - Refreshes photos every 2 seconds |
        +--------------------------+
```

---

# ⚙️ How to Get Each Information Over Wi-Fi:

| Info Type                  | Source (Hardware/Software)           | How to Get via Wi-Fi                                                                       | Notes                                                                 |
| -------------------------- | ------------------------------------ | ------------------------------------------------------------------------------------------ | --------------------------------------------------------------------- |
| **1. Obstacle Distance**   | Ultrasonic sensor (HC-SR04) or Lidar | Connected to Flight Controller or NodeMCU ADC/UART; NodeMCU reads and sends via Wi-Fi JSON | If connected to FC, fetch via MAVLink or directly read on NodeMCU ADC |
| **2. Camera**              | Runcam 4K or ESP32-CAM               | Trigger photo capture every 2 sec; send JPEG over Wi-Fi HTTP server or WebSocket           | Not video streaming; just snapshots every 2 seconds                   |
| **3. Battery Percentage**  | Voltage sensor on battery            | Flight Controller monitors voltage; sends data via MAVLink to NodeMCU                      | NodeMCU forwards battery % as part of telemetry JSON                  |
| **4. Altitude**            | Barometer + GPS                      | Flight Controller data over MAVLink                                                        | Altitude in meters updated continuously                               |
| **5. Speed**               | GPS + IMU (Flight Controller)        | MAVLink telemetry                                                                          | Speed in m/s or km/h                                                  |
| **6. GPS Location**        | GPS Module                           | MAVLink GPS coordinates                                                                    | Latitude, Longitude, and GPS fix status                               |
| **7. Orientation**         | IMU Sensors (Gyro, Accelerometer)    | MAVLink attitude messages                                                                  | Yaw, pitch, roll angles                                               |
| **8. Status Logs**         | Flight Controller Logs               | MAVLink messages or custom status messages                                                 | Include mode, errors, warnings                                        |
| **9. Mathematical Things** | NodeMCU or Base Station              | Calculate in real-time or post-process telemetry data                                      | Example: average speed, distance traveled, battery usage rate         |

---

# 🛠️ Implementation Details

### 1. **Telemetry Collection**

* Flight Controller outputs **MAVLink telemetry** via UART.
* NodeMCU (or ESP32) reads MAVLink data on serial RX.
* Parse important messages (battery, GPS, altitude, orientation, speed, status).

### 2. **Obstacle Distance**

* If ultrasonic or lidar connects directly to NodeMCU (analog or digital pins), NodeMCU reads and includes data in telemetry JSON.
* Or obstacle sensor data is integrated into FC and forwarded.

### 3. **Camera Snapshot Handling**

* NodeMCU controls the camera trigger:

  * For Runcam Split 4, trigger snapshots via hardware or API.
  * For ESP32-CAM, take photo with built-in camera module.
* Save photo temporarily.
* Serve photo via HTTP endpoint (e.g., `http://drone_ip/photo.jpg`).
* Laptop/mobile refreshes photo every 2 seconds automatically.

### 4. **Wi-Fi Access Point Setup**

* NodeMCU creates Wi-Fi AP with WPA2 password.
* Host a small webserver serving:

  * Telemetry JSON API (e.g., `/telemetry`)
  * Photo HTTP endpoint (e.g., `/photo.jpg`)
* Client devices connect to AP and fetch data.

### 5. **Client Dashboard**

* Simple webpage with JavaScript to:

  * Fetch and display telemetry data live.
  * Fetch and refresh photo image every 2 seconds.
  * Display status logs and mathematical calculations.

---

# 💡 Summary: What You Will See on the Client

| Display Item                 | Description                            |
| ---------------------------- | -------------------------------------- |
| Battery %                    | Real-time battery voltage as %         |
| Altitude                     | Current altitude in meters             |
| Speed                        | Current speed in km/h or m/s           |
| GPS Location                 | Map showing drone position             |
| Orientation (Yaw/Pitch/Roll) | 3D representation or numbers           |
| Obstacle Distance            | Distance to nearest object ahead       |
| Camera Image                 | Updated snapshot every 2 seconds       |
| Status Logs                  | Flight modes, warnings                 |
| Calculated Values            | e.g., avg speed, remaining flight time |

---

If you want, I can help you build the **NodeMCU code**, **MAVLink parsing scripts**, and **dashboard web UI** next! Just say the word.

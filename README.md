# 🚗 Smart Parking System for Smart Cities

An end-to-end, lightweight **IoT-Based Smart Parking Prototype** designed for smart cities. This project simulates real-world physical parking slots equipped with occupancy sensors, communicating with a cloud-processing layer via a REST API, and displaying live updates on an interactive web dashboard.

Built entirely using the **Python Standard Library (dependency-free)** for the backend and **Modern Vanilla HTML/CSS/JS** for the frontend, making it extremely easy to set up, run, and demonstrate.

---

## 🏗️ System Architecture

The application is structured to replicate a real-world IoT deployment:

```text
┌────────────────────────┐          ┌───────────────────────┐          ┌────────────────────────┐
│  Python IoT Simulator  │ ──POST─> │    Local Cloud API    │ ──JSON─> │     Web Dashboard      │
│  (Sensor Simulation)   │          │ (Python server.py/app)│          │ (HTML5 / CSS3 / JS)    │
└────────────────────────┘          └───────────────────────┘          └────────────────────────┘
            │                                   ▲
            ▼ [Optional Production Flow]        │ [Optional Production Flow]
┌────────────────────────┐          ┌───────────────────────┐
│   IBM Watson IoT Hub   │ ───────> │  Node-RED Flow Layer  │
└────────────────────────┘          └───────────────────────┘
```

1. **IoT Sensor Layer (`simulator.py`)**: Simulates ultrasonic/infrared sensors installed at each parking space detecting vehicle arrival/departure.
2. **Cloud API Layer (`server.py` / `app.py`)**: A multi-threaded web server that processes incoming sensor events, manages parking reservations, and tracks occupancy status.
3. **Web Dashboard (`web/`)**: An interactive GUI that displays real-time slot statuses, statistics, reservation forms, and a live activity feed.

---

## 🌟 Key Features

*   **📊 Live Statistics Dashboard**: Real-time counter showing **Available**, **Occupied**, and **Reserved** slots.
*   **🗺️ Interactive Floor Map**: Two-zone interactive UI representation (Zone A & Zone B) showing individual slot statuses.
*   **📅 Reservation System**: Allows drivers to book slots using their **Name** and **Vehicle Number** (simulating pre-booking).
*   **📡 IoT Sensor Simulation**: Automatic continuous simulation of vehicles entering/exiting slots with adjustable time intervals.
*   **📜 Live Activity Feed**: Logs every event (bookings, cancellations, sensor changes) with timestamps in a chronological timeline.
*   **📱 Responsive & Modern UI**: Sleek glassmorphism design with a dark mode aesthetic that works on desktops, tablets, and mobiles.
*   **🧪 Built-in Test Suite**: Unit tests covering status reporting, reservation flows, cancellation limits, and sensor event state transitions.

---

## 📂 Project Structure

```text
├── .vscode/                 # VS Code tasks & configurations
│   ├── settings.json
│   └── tasks.json
├── node-red/
│   └── flows.json           # Importable flow for IBM Cloud Integration
├── tests/
│   └── test_parking.py      # Automated Python Unit Tests
├── web/                     # Web Dashboard Assets
│   ├── app.js               # Frontend Logic & API polling
│   ├── index.html           # Dashboard layout
│   └── styles.css           # Premium Dark Glassmorphism Styling
├── .gitignore               # Ignored files (pycache, local envs)
├── app.py                   # Main Application Entry Point
├── ibm-cloud.env.example    # Configuration example for IBM Cloud deployment
├── README.md                # Project documentation
├── server.py                # Main backend server logic and API routes
└── simulator.py             # Python IoT hardware simulator script
```

---

## 🚀 How to Run the Project

### Method 1: Running in VS Code (Recommended)
1. Open this project folder in **VS Code**.
2. Press `Ctrl + Shift + P` (or `Cmd + Shift + P` on Mac) and select **Tasks: Run Task**.
3. Choose **Run Smart Parking System**.
4. Open your browser and go to: **[http://127.0.0.1:8000](http://127.0.0.1:8000)**.

### Method 2: Running via Terminal
1. Open your terminal in the project directory.
2. Run the web server:
   ```bash
   python app.py
   ```
3. Open your browser and navigate to **[http://127.0.0.1:8000](http://127.0.0.1:8000)**.

### 📡 Starting the IoT Hardware Simulator
To simulate real-time sensor events (vehicles parking and leaving), open a **second terminal** and run:
```bash
python simulator.py
```
This script will send randomized sensor events to the API every 5 seconds. You will see the parking dashboard update automatically in real-time!

---

## 🔌 API Endpoints Reference

The Cloud API exposes the following REST endpoints:

| Method | Endpoint | Description | Payload Example |
| :--- | :--- | :--- | :--- |
| **GET** | `/api/status` | Fetch status of all 12 slots, total statistics, and recent activity logs. | *None* |
| **GET** | `/api/health` | Checks if the server is healthy and online. | *None* |
| **POST** | `/api/slots/{id}/book` | Reserve an available slot (must specify name & vehicle number). | `{"name": "John Doe", "vehicle": "MH-12-AB-1234"}` |
| **POST** | `/api/slots/{id}/cancel` | Cancel an active reservation and release the slot. | *None* |
| **POST** | `/api/sensor-event` | Publish raw hardware sensor data (occupied: true/false). | `{"slot": 5, "occupied": true}` |
| **POST** | `/api/simulate` | Triggers a single randomized sensor event. | *None* |

---

## 🧪 Running Automated Tests

To run the test suite and verify that the booking logic and state changes are functioning correctly:

**Via Terminal:**
```bash
python -m unittest discover -s tests -v
```

**Via VS Code:**
Go to **Terminal → Run Task → Run Tests**.

---

## 🎓 College Demonstration Guide

When presenting this project to evaluators/professors, follow this structured flow:

1. **Explain the IoT Concept**: Explain that in a real-world implementation, each parking slot would have an ultrasonic sensor connected to a microcontroller (like ESP8266/Arduino) publishing state changes to the cloud.
2. **Launch the Dashboard**: Open **[http://127.0.0.1:8000](http://127.0.0.1:8000)** and show the 12-slot layout. Explain the color codes (Green = Available, Red = Occupied by vehicle, Yellow = Reserved/Pre-booked).
3. **Demonstrate Pre-Booking**: Click on an available (Green) slot, enter a driver's name and vehicle number, and click **Book**. The slot color immediately changes to Yellow.
4. **Trigger Sensor Activity**:
    *   *Manual*: Click the **Simulate sensor** button on the UI to trigger a single event.
    *   *Automated*: Start `python simulator.py` in the terminal to demonstrate continuous live vehicle entries/exits. Notice how the dashboard and activity logs update automatically.
5. **Explain the Cloud Integration Option**: Explain that the project is built to easily switch to cloud brokers like **IBM Watson IoT** or **AWS IoT Core**. Point to `node-red/flows.json` and explain how Node-RED acts as a middleware to connect hardware devices to the local dashboard.

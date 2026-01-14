# 🚗 Road Safety and Traffic Diagnostics Assisted by AI

[![ROS 2](https://img.shields.io/badge/ROS_2-Humble-22314E?style=flat&logo=ros&logoColor=white)](https://docs.ros.org/en/humble/index.html)
[![Simulation](https://img.shields.io/badge/Simulation-SUMO%20%26%20TraCI-blue?style=flat&logo=eclipse-mosquitto&logoColor=white)](https://sumo.dlr.de/docs/)
[![AI Model](https://img.shields.io/badge/AI-LSTM%20Network-orange?style=flat&logo=tensorflow&logoColor=white)](https://keras.io/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

## 📌 Abstract
Road traffic accidents are often the result of the "reactive" nature of traditional safety systems (ADAS). To address this challenge, this project presents **Road Safety and Traffic Diagnostics Assisted by AI**, a real-time system designed to predict potential collisions *before* they occur.

The system utilizes **ROS 2** as middleware to integrate traffic simulation (SUMO) with an **LSTM Deep Learning model**. It predicts the future trajectory of surrounding vehicles (5 seconds ahead) and assesses risk using **Time-to-Collision (TTC)** metrics. Experimental results demonstrated a prediction accuracy of **RMSE ≈ 0.0511** and a system latency of **<100ms**, proving the feasibility of proactive safety on Edge Computing architectures.

> **Project Type:** Senior Capstone Project (Phase 1: Proof of Concept).
> **Grade:** A+ (High Distinction).

---

## 🚀 Key Features
- **Proactive Safety:** Predicts hazards 3-5 seconds in advance instead of reacting to them.
- **Deep Learning Core:** Uses **Long Short-Term Memory (LSTM)** networks to analyze historical trajectory data (10 steps) and forecast future paths (5 steps).
- **Real-Time Architecture:** Built on **ROS 2 Humble** with a distributed node system ensuring sub-100ms response time.
- **Risk Assessment Logic:** Dynamic calculation of Euclidean Distance and TTC to classify risks into (Safe, Warning, Critical).
- **Realistic Simulation:** Validated on a digital twin of **Taif City** road network using OpenStreetMap (OSM) and SUMO/TraCI.

---

## 🛠️ System Architecture (ROS 2 Nodes)
The system is modularized into three main ROS 2 packages operating at a 10Hz synchronized frequency:

1.  **Data Publisher Node (`data_publisher`):**
    * Interfaces with **SUMO** via **TraCI**.
    * Extracts raw vehicle states (X, Y, Speed) from the Taif City map simulation.
    * Publishes to topic: `/raw/vehicle_state`.

2.  **AI Prediction Node (`lstm_predictor`):**
    * Subscribes to raw data.
    * Performs **Normalization** (MinMax Scaling).
    * Runs the trained **LSTM Model** to predict the next 5 seconds of trajectory.
    * Publishes to topic: `/prediction/paths`.

3.  **Risk Assessment Node (`risk_assessor`):**
    * Calculates Euclidean Distance ($d$) between the Ego vehicle and obstacles.
    * Applies Decision Logic:
        * 🟢 **Safe:** $d > 60m$
        * 🟡 **Warning:** $30m < d < 60m$
        * 🔴 **Critical:** $d < 30m$ (Triggers Alert).

---

## 📊 Performance & Results
The system was validated through rigorous simulation scenarios:

### 1. Prediction Accuracy
* **Metric:** Root Mean Square Error (RMSE).
* **Result:** **0.0511** (Normalized).
* **Analysis:** The LSTM model successfully captured non-linear vehicle dynamics and temporal dependencies.

### 2. System Latency
* **Requirement:** Real-time processing.
* **Result:** End-to-end latency (Data Ingestion $\to$ Alert) is **< 100 milliseconds**.

### 3. Visualization
* **Tool:** Rviz & RQT Graph.
* **Outcome:** The predicted trajectory (Blue Line) aligned perfectly with the actual vehicle position (Red Prism) in the simulation.

---

## 💻 Technology Stack
* **Middleware:** ROS 2 Humble Hawks.
* **OS:** Ubuntu 22.04 LTS (Linux).
* **Language:** Python 3.8+ (Nodes & Logic), C++.
* **Simulation:** SUMO (Simulation of Urban Mobility) & TraCI API.
* **AI Frameworks:** TensorFlow/Keras, Scikit-learn, Pandas, NumPy.
* **Map Data:** OpenStreetMap (OSM) - Taif City Network.

---

## 🔮 Future Work (Phase 2)
As outlined in the project roadmap and recommendations:

- [ ] **High-Fidelity Simulation:** Migration to **CARLA Simulator** to generate photorealistic sensor data (Virtual RGB-D Cameras & LiDAR) instead of utilizing direct coordinates from SUMO.
- [ ] **Hardware-in-the-Loop (HIL):** Deploy ROS 2 nodes on **NVIDIA Jetson** to test real-time processing latency using the data stream from CARLA.
- [ ] **State Estimation:** Implement **Kalman Filters** or similar algorithms to handle noise and uncertainty in the virtual sensor data.


---

## 👥 Team & Contributors
This project was developed as a Senior Capstone Project by:

* **Sadeem AlBoqami** - *Lead Developer & System Architect* (ROS 2 & AI Implementation). [LinkedIn](https://www.linkedin.com/in/SadeemAlBoqami)
* **Lujain AlMalki** - *[Data preprocessing]*
* **Ahdab AlOttaibi** - *[LSTM Model - Trainning]*
* **Hutun AlAzwari** - *[LSTM Model - Testting]*
* **Shahad AlFahmi** - *[Risk Analysis]*
* **Almaha AlAyly** - *[Literature Review]*
* **Joud AlZahrani** - *[Literature Review]*
* **Joury AlSufyani** - *[System Analysis and Design]*

**Supervised by:**
* Dr. Entesar Gemeay

---
*Based on the research report: "Road Safety and Traffic Diagnostics Assisted by AI", Taif University, 2026.*

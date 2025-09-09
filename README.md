# IoT-Enabled Crowd Navigation and Disaster Response System

A cloud-based system using Raspberry Pi, Azure, and LoRa-enabled ESP32 devices to provide real-time navigation instructions during disaster scenarios. Features both a **deterministic core (MVP)** and an **ML-driven advanced research track**.

## 📁 Project Structure & Team Ownership

| Component | Lead | Description |
| :--- | :--- | :--- |
| [`/hardware`](./hardware/esp32-firmware/README.md) | Hardware Team | ESP32 firmware for sensors and LoRa communication. |
| [`/gateway`](./gateway/rpi-scripts/README.md) | Cloud Team | RPi gateway software and Azure IoT client. |
| [`/gateway/docker-simulator`](./gateway/docker-simulator/README.md) | Cloud Team | Dockerized simulator for virtual devices. |
| [`/cloud`](./cloud/azure-functions/README.md) | Cloud Team | Azure Functions, Terraform configs, and cloud logic. |
| [`/ml-model`](./ml-model/README.md) | Data Team | Colab notebooks for the dynamic safe zone ML model. |
| [`/docs`](./docs/contracts.md) | All | Project contracts, plans, and diagrams. |

## 🚀 Getting Started

1.  **Read the Contracts:** Before doing anything, read [`/docs/contracts.md`](./docs/contracts.md). These are the project's rules.
2.  **Setup for your role:** Navigate to your team's directory and follow the detailed README for setup instructions.

## 🔧 Core Technologies
- **Edge:** ESP32 (C++), LoRa (SX1276), GPS (NEO-6M)
- **Gateway:** Raspberry Pi 4, Python, Docker
- **Cloud:** Azure IoT Hub, Azure Functions, Azure Maps, Terraform
- **ML:** Python, Pandas, Scikit-learn (Google Colab)

## 👥 Team
- [] - Cloud & Gateway Architecture
- [] - Hardware Firmware
- [] - Hardware Integration
- [] - Data Science & ML Model

## 📄 License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
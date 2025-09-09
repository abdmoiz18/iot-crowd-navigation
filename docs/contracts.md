This file is the single source of truth for your project and must be agreed upon by all team members before serious development begins.

---

# Project Contracts & Interfaces

**Version:** 1.0
**Last Updated:** YYYY-MM-DD
**Status:** ✅ Active

This document defines the unbreakable contracts between all system components. Any change to these contracts requires a team-wide discussion, a version bump, and coordinated updates across all affected modules.

---

## 1. Data Schema Contract: Device Telemetry

All IoT devices (physical ESP32s and simulated Docker containers) **MUST** transmit data in the following JSON format. This is the common language of the system.

### Schema
```json
{
  "device_id": "esp32_01",
  "timestamp": 1727832865,
  "latitude": 12.345678,
  "longitude": 98.765432,
  "heading": 180.5,
  "battery": 85,
  "rssi": -45
}
```

### Field Definitions
| Field | Type | Description | Example |
| :--- | :--- | :--- | :--- |
| `device_id` | `string` | Unique identifier for the device. | `"esp32_01"`, `"sim_05"` |
| `timestamp` | `integer` | Unix Epoch time (seconds since Jan 1, 1970, UTC). | `1727832865` |
| `latitude` | `float` | WGS84 decimal degrees. Precision to 6 decimal places. | `12.345678` |
| `longitude` | `float` | WGS84 decimal degrees. Precision to 6 decimal places. | `98.765432` |
| `heading` | `float` | Degrees from Magnetic North. Range: 0.0 to 359.9. | `180.5` (South) |
| `battery` | `integer` | Remaining battery percentage. Range: 0 to 100. | `85` |
| `rssi` | `integer` | LoRa signal strength (Received Signal Strength Indicator). | `-45` (Higher is better) |

---

## 2. Data Schema Contract: Cloud-to-Device Command

The Azure Cloud sends commands back to devices to guide them. The system supports two levels of complexity.

### Option A: Core MVP (Simple Bearing)
```json
{
  "command": "navigate",
  "bearing": 45
}
```

### Option B: Upgraded System (Step-by-Step Instruction)
```json
{
  "command": "navigate",
  "instruction": "proceed",
  "direction": "NE",
  "distance": 50,
  "next": "turn_right"
}
```

### Field Definitions
| Field | Type | Description | Allowed Values |
| :--- | :--- | :--- | :--- |
| `command` | `string` | The action to perform. | `"navigate"` |
| `bearing` | `float` | **(Option A)** Degrees to destination. | `0.0` to `359.9` |
| `instruction` | `string` | **(Option B)** The specific action. | `"proceed"`, `"turn"`, `"caution"`, `"stop"` |
| `direction` | `string` | **(Option B)** Cardinal direction. | `"N"`, `"NE"`, `"E"`, `"SE"`, `"S"`, `"SW"`, `"W"`, `"NW"` |
| `distance` | `integer` | **(Option B)** Meters until next action. | Any positive integer |
| `next` | `string` | **(Option B)** The upcoming action. | `"turn_left"`, `"turn_right"`, `"destination"` |

---

## 3. Data Schema Contract: ML Model Output

The Machine Learning model (Advanced Track) publishes its analysis to the cloud to dynamically update safe zones and obstacles.

### Schema
```json
{
  "safe_zone_lat": 12.3456,
  "safe_zone_lon": 98.7654,
  "obstacles": [
    {
      "type": "fire",
      "polygon": [
        [12.344, 98.764],
        [12.346, 98.764],
        [12.346, 98.766],
        [12.344, 98.766]
      ]
    }
  ]
}
```

### Field Definitions
| Field | Type | Description |
| :--- | :--- | :--- |
| `safe_zone_lat` | `float` | Latitude of the dynamic safe zone (WGS84). |
| `safe_zone_lon` | `float` | Longitude of the dynamic safe zone (WGS84). |
| `obstacles` | `array` | List of obstacle objects. |
| `obstacles[].type` | `string` | Type of hazard. | `"fire"`, `"flood"`, `"collapse"`, `"blockage"` |
| `obstacles[].polygon` | `array` | Array of [lat, lon] pairs defining the hazard area. | Minimum of 3 pairs to form a polygon. |

---

## 4. Communication Protocol Contract

This section defines how components talk to each other.

### 4.1. ESP32 to Raspberry Pi Gateway
*   **Protocol:** LoRa (Long Range Radio)
*   **Module:** SX1276 (RFM95W)
*   **Frequency:** `868.1 MHz` (Adjust for your region)
*   **Spreading Factor:** `SF7` (Balance of range and speed)
*   **Data Format:** The JSON string from **Section 1**, transmitted as a plain text packet.

### 4.2. Raspberry Pi Gateway to Azure Cloud
*   **Protocol:** MQTT over TLS (Port 8883)
*   **Client Library:** Azure IoT Python SDK (`azure-iot-device`)
*   **Service:** Azure IoT Hub / Device Provisioning Service (DPS)
*   **Authentication:** X.509 Certificate-based authentication.
*   **Data Format:** The JSON from **Section 1** is placed in the `message.body` of the IoT Hub message.

### 4.3. ML Model to Azure Cloud
*   **Protocol:** HTTP POST
*   **Endpoint:** `https://<azure-function-url>/api/set-safezone`
*   **Headers:** `Content-Type: application/json`
*   **Body:** The JSON from **Section 3**.

---

## 5. Device Identity Contract

*   **Naming Scheme:** `{device_type}_{identifier}`
*   **Physical Devices:** `esp32_01`, `esp32_02`, `esp32_03`
*   **Simulated Devices:** `sim_01`, `sim_02`, `sim_03`
*   **All device identities MUST be pre-registered in Azure IoT Hub's Device Provisioning Service (DPS)** using X.509 certificates.

---

## Version History

| Version | Date | Author | Changes |
| :--- | :--- | :--- | :--- |
| 1.0 | YYYY-MM-DD | [Author Name] | Initial version of all contracts. |
| | | | |

---

## Agreement

By adding code to this repository, all team members agree to adhere to these contracts. Changes must be proposed via a GitHub Issue and agreed upon by the team before implementation.

**Signed,**

*   **Cloud Team:** _________________________
*   **Hardware Team:** _________________________
*   **Data Team:** _________________________

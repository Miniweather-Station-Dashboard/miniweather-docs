# Add Onboarding Device

To onboard a new IoT device into the Miniweather Station Dashboard, follow the steps below.

---

## ✅ As an Admin (How to Add Device via UI)

### Steps:

1. 🔐 **Log in** to the Admin Panel (`/admin/login`)
2. 📦 Navigate to the **Devices page** (`/devices`)
3. ➕ Click `+ Add Device`
4. 📝 Fill in the form:

   * **Device Name** (e.g., `Station Pantai Selatan`)
   * **Serial Number** (must be unique)
   * **Location**: Latitude and Longitude
5. 💾 Click `Save`

---

## ⚙️ Backend Process

Once the device is submitted, the frontend sends a POST request to the backend API.

### Example Request:

```
POST /v1/onboarding-devices
Content-Type: application/json
Authorization: Bearer <admin-token>
```

```json
{
  "name": "Station Pantai Selatan",
  "serial_number": "MW-PS-01",
  "location": {
    "lat": -7.7956,
    "lng": 110.3695
  }
}
```

---

## 🛠 Backend Logic

1. Validate request fields
2. Insert into `onboarding_devices` table
3. Auto-link default sensor types (optional)
4. Subscribe to MQTT topic `miniweather/<serial_number>`
5. Prepare schema mapping for the device
6. Log registration event

---

## 💡 MQTT Topic Convention

Every device is assigned a unique MQTT topic for data streaming:

```
miniweather/<serial_number>
```

Example:

```
miniweather/MW-PS-01
```

---

## 🧠 Redis and Hyperbase Integration

If Hyperbase is **online**:

* Schema is synced immediately

If Hyperbase is **offline**:

* Data sent from the device will be cached in Redis until it can be synced later

---

## 📌 Summary Table

| Step      | Description                             |
| --------- | --------------------------------------- |
| UI Input  | Admin enters device info via form       |
| API Call  | POST `/v1/onboarding-devices`           |
| DB Insert | Record added to `onboarding_devices`    |
| MQTT Sub  | System subscribes to device topic       |
| Redis     | Queues messages if Hyperbase is down    |
| Hyperbase | Syncs schema and storage when available |

---

Once completed, the device is ready to transmit weather data and be visualized on the dashboard.

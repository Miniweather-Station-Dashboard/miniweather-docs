# Data Generator

This section explains how to use the Go-based **Data Generator** to simulate sensor readings and publish them to the MQTT broker. This tool is useful for testing the system when real devices are not available or when you need to generate continuous dummy data.

---

## Overview

The data generator is a standalone Go program that:

* Connects to an MQTT broker (using WebSocket protocol).
* Continuously simulates random sensor values (e.g., seismograph, humidity, pressure).
* Publishes the generated data to a specific device topic.

---

## Prerequisites

Before running the generator, make sure you have:

* **Go** installed (minimum v1.18).
* An **MQTT broker** running (e.g., [EMQX](https://www.emqx.io/)).
* A valid **MQTT topic** assigned to a device (UUID-based).

---

## Installation

1. Save the following code into a file, e.g., `generator.go`:

```go
package main

import (
	"encoding/json"
	"fmt"
	"math/rand"
	"time"

	mqtt "github.com/eclipse/paho.mqtt.golang"
)

type Payload struct {
	Data map[string]interface{} `json:"data"`
}

func main() {
	// MQTT broker over WebSocket
	broker := "ws://127.0.0.1:8083/mqtt" // use 127.0.0.1 instead of 0.0.0.0
	clientID := "GoDummySensor"

	opts := mqtt.NewClientOptions()
	opts.AddBroker(broker)
	opts.SetClientID(clientID)

	client := mqtt.NewClient(opts)

	if token := client.Connect(); token.Wait() && token.Error() != nil {
		fmt.Printf("❌ Failed to connect to broker: %v\n", token.Error())
		return
	}
	fmt.Println("✅ Connected to EMQX broker via WebSocket.")

	// Your device-specific MQTT topic
	topic := "/devices/a8670a62-4a6d-443d-a532-2f1325a8f7db"
	counter := 1

	for {
		// Simulate realistic sensor values
		data := map[string]interface{}{
			"humidity":   50 + rand.Float64()*30, // 50–80%
			"pressure":   1000 + rand.Float64()*50, // hPa
		}

		payload := Payload{Data: data}

		jsonPayload, err := json.Marshal(payload)
		if err != nil {
			fmt.Printf("❌ Failed to marshal JSON: %v\n", err)
			continue
		}

		token := client.Publish(topic, 1, false, jsonPayload)
		token.Wait()

		now := time.Now().UTC()
		fmt.Printf("📤 [%02d] Sent at %s | Topic: %s\n", counter, now.Format(time.RFC3339Nano), topic)

		counter++
		time.Sleep(2 * time.Second)
	}
}
```

2. Install dependencies:

   ```bash
   go mod init generator
   go get github.com/eclipse/paho.mqtt.golang
   ```
3. Build the binary:

   ```bash
   go build -o generator generator.go
   ```

---

## Usage

Run the generator with:

```bash
./generator
```

Expected output:

```
✅ Connected to EMQX broker via WebSocket.
📤 [01] Sent at 2025-09-15T09:00:00Z | Topic: /devices/a8670a62-4a6d-443d-a532-2f1325a8f7db
📤 [02] Sent at 2025-09-15T09:00:02Z | Topic: /devices/a8670a62-4a6d-443d-a532-2f1325a8f7db
...
```

---

## Customization

You can modify the following parts of the code to fit your needs:

* **Broker URL**:

  ```go
  broker := "ws://127.0.0.1:8083/mqtt"
  ```

  Change the host/port according to your EMQX configuration.

* **Topic**:

  ```go
  topic := "/devices/<device-uuid>"
  ```

  Replace with your device’s actual topic.

* **Data Fields**:

  ```go
  data := map[string]interface{}{
      "humidity":   50 + rand.Float64()*30,
      "pressure":   1000 + rand.Float64()*50,
  }
  ```

  Uncomment or add additional fields (e.g., `temperature`, `rainfall`, `wind_speed`).

* **Interval**:

  ```go
  time.Sleep(2 * time.Second)
  ```

  Adjust the interval between messages.

---

## Example Payload

A typical JSON payload looks like this:

```json
{
  "data": {
    "humidity": 65.12,
    "pressure": 1012.56
  }
}
```

---

## Notes

* The generator runs **indefinitely** until you stop it manually (`Ctrl+C`).
* Ensure that the MQTT topic matches your backend subscription.
* You can run multiple instances with different client IDs and topics to simulate multiple devices.

---


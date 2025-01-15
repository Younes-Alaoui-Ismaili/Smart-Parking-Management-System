# Smart-Parking-Management-System
If you're looking for a **second project** that complements the **Smart City Traffic Management System**, here's an idea:

---

## **Project 2: Smart Parking Management System**

### **Project Description**
This project aims to design and implement a **Smart Parking Management System** for cities. The system uses IoT sensors, cameras, and cloud platforms to monitor parking space availability, guide drivers to empty spots, and optimize parking space utilization.

---

## **Key Features**
1. **IoT Sensor Network**: Detects parking space occupancy using ultrasonic or infrared sensors.
2. **Real-Time Data Processing**: Processes parking data using Apache Kafka and Node-RED.
3. **Cloud Integration**: Stores and analyzes data using AWS IoT Core and Google Cloud IoT.
4. **Mobile App Integration**: Provides real-time parking availability to drivers via a mobile app.
5. **Dynamic Pricing**: Adjusts parking fees based on demand and time of day.

---

## **System Architecture**
![System Architecture Diagram](#) *(Add a diagram here if available)*

1. **IoT Layer**:
   - **ESP32 Devices**: Detect parking space occupancy using ultrasonic sensors.
   - **Raspberry Pi**: Acts as a gateway for data aggregation and edge processing.
   - **IP Cameras**: Capture images for license plate recognition and security.

2. **Communication Layer**:
   - **MQTT**: For real-time communication between IoT devices and the cloud.
   - **LoRaWAN**: For long-range, low-power communication in large parking areas.

3. **Data Processing Layer**:
   - **Apache Kafka**: For real-time data streaming.
   - **Node-RED**: For workflow automation and data routing.

4. **Cloud Layer**:
   - **AWS IoT Core**: For device management and data ingestion.
   - **Google Cloud IoT**: For advanced analytics and machine learning.

5. **Application Layer**:
   - **Mobile App**: Displays real-time parking availability and guides drivers to empty spots.
   - **Admin Dashboard**: Provides insights into parking utilization and revenue.

---

## **File Structure**
```
smart-parking/
├── iot/
│   ├── parking_sensor.py          # ESP32 parking sensor code
│   ├── gateway.py                 # Raspberry Pi gateway code
├── data_processing/
│   ├── data_processor.py          # Kafka and Node-RED integration
├── mobile_app/
│   ├── app.py                     # Backend for the mobile app
│   ├── templates/
│   │   ├── index.html             # Frontend for the mobile app
│   ├── static/
│   │   ├── styles.css             # CSS for the mobile app
├── admin_dashboard/
│   ├── app.py                     # Backend for the admin dashboard
│   ├── templates/
│   │   ├── index.html             # Frontend for the admin dashboard
│   ├── static/
│   │   ├── styles.css             # CSS for the admin dashboard
├── config/
│   ├── main_config.py             # Configuration settings
├── requirements.txt               # Python dependencies
├── README.md                      # Project documentation
```

---

## **Implementation**

### 1. IoT Sensor Network

#### `iot/parking_sensor.py`
This script simulates ESP32 devices detecting parking space occupancy.

```python
import paho.mqtt.client as mqtt
import random
import time

# MQTT Configuration
MQTT_BROKER = "mqtt.eclipseprojects.io"
MQTT_TOPIC = "smart-parking/occupancy"

# Simulate parking sensor data
def simulate_sensor_data():
    return {
        "space_id": random.randint(1, 100),
        "occupied": random.choice([True, False]),
        "timestamp": time.time(),
    }

# MQTT Client
client = mqtt.Client()
client.connect(MQTT_BROKER)

# Publish data
while True:
    data = simulate_sensor_data()
    client.publish(MQTT_TOPIC, str(data))
    print(f"Published: {data}")
    time.sleep(5)  # Send data every 5 seconds
```

---

### 2. Data Processing

#### `data_processing/data_processor.py`
This script consumes data from Kafka and processes it.

```python
from kafka import KafkaConsumer
import json

# Kafka Configuration
KAFKA_TOPIC = "parking-data"
KAFKA_BROKER = "localhost:9092"

# Kafka Consumer
consumer = KafkaConsumer(
    KAFKA_TOPIC,
    bootstrap_servers=KAFKA_BROKER,
    value_deserializer=lambda x: json.loads(x.decode("utf-8")),
)

# Process data
for message in consumer:
    data = message.value
    print(f"Processing: {data}")
    # Add your data processing logic here
```

---

### 3. Mobile App

#### `mobile_app/app.py`
This script creates a Flask backend for the mobile app.

```python
from flask import Flask, render_template
import random
import time
import threading

app = Flask(__name__)

# Simulate real-time parking data
parking_data = {"available_spaces": 0}

def update_parking_data():
    global parking_data
    while True:
        parking_data = {
            "available_spaces": random.randint(0, 100),
        }
        time.sleep(5)

# Start background thread
threading.Thread(target=update_parking_data, daemon=True).start()

@app.route("/")
def index():
    return render_template("index.html", data=parking_data)

if __name__ == "__main__":
    app.run(debug=True)
```

#### `mobile_app/templates/index.html`
This is the frontend HTML for the mobile app.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Smart Parking App</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='styles.css') }}">
</head>
<body>
    <h1>Smart Parking App</h1>
    <div id="parking-data">
        <p>Available Spaces: <span id="available-spaces">{{ data.available_spaces }}</span></p>
    </div>
    <script>
        // Update data every 5 seconds
        setInterval(async () => {
            const response = await fetch("/");
            const html = await response.text();
            const parser = new DOMParser();
            const doc = parser.parseFromString(html, "text/html");
            document.getElementById("available-spaces").textContent = doc.getElementById("available-spaces").textContent;
        }, 5000);
    </script>
</body>
</html>
```

---

### 4. Admin Dashboard

#### `admin_dashboard/app.py`
This script creates a Flask backend for the admin dashboard.

```python
from flask import Flask, render_template
import random
import time
import threading

app = Flask(__name__)

# Simulate real-time parking data
parking_data = {"total_spaces": 100, "occupied_spaces": 0}

def update_parking_data():
    global parking_data
    while True:
        parking_data = {
            "total_spaces": 100,
            "occupied_spaces": random.randint(0, 100),
        }
        time.sleep(5)

# Start background thread
threading.Thread(target=update_parking_data, daemon=True).start()

@app.route("/")
def index():
    return render_template("index.html", data=parking_data)

if __name__ == "__main__":
    app.run(debug=True)
```

#### `admin_dashboard/templates/index.html`
This is the frontend HTML for the admin dashboard.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Admin Dashboard</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='styles.css') }}">
</head>
<body>
    <h1>Admin Dashboard</h1>
    <div id="parking-data">
        <p>Total Spaces: <span id="total-spaces">{{ data.total_spaces }}</span></p>
        <p>Occupied Spaces: <span id="occupied-spaces">{{ data.occupied_spaces }}</span></p>
    </div>
    <script>
        // Update data every 5 seconds
        setInterval(async () => {
            const response = await fetch("/");
            const html = await response.text();
            const parser = new DOMParser();
            const doc = parser.parseFromString(html, "text/html");
            document.getElementById("total-spaces").textContent = doc.getElementById("total-spaces").textContent;
            document.getElementById("occupied-spaces").textContent = doc.getElementById("occupied-spaces").textContent;
        }, 5000);
    </script>
</body>
</html>
```

---

### 5. Run the Project

1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

2. Start the IoT sensor network:
   ```bash
   python iot/parking_sensor.py
   ```

3. Start the data processor:
   ```bash
   python data_processing/data_processor.py
   ```

4. Run the mobile app:
   ```bash
   python mobile_app/app.py
   ```

5. Run the admin dashboard:
   ```bash
   python admin_dashboard/app.py
   ```

6. Access the mobile app at `http://localhost:5000` and the admin dashboard at `http://localhost:5001`.

---

This **Smart Parking Management System** complements the **Smart City Traffic Management System** and provides a complete solution for urban mobility. Let me know if you need further assistance! 🚀

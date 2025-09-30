# EcoVision VGEC — SIH Project

EcoVision VGEC is a Smart Waste Segregation System developed as a part of the Smart India Hackathon (SIH). This repository showcases our integrated approach combining hardware design, machine learning, and embedded systems for smart and efficient trash management.

---

## 🚀 Project Overview

EcoVision aims to automate waste segregation using a camera-based ML model, ESP32-powered electronics, and a robust mechanical design.

- **Image Classification:** Detects and classifies waste (plastic, metal, paper, etc.) via a webcam and a custom-trained Roboflow ML model.
- **Automated Sorting:** Based on ML output, actuators sort the waste into respective bins.
- **Custom Electronics:** All-in-one PCB with ESP32, motor drivers, sensor interfacing, and power management.
- **CAD-Designed Bin:** Mechanized dustbin with camera mount, servo sorting mechanism, and sensor integration.

---

## 📷 Photos & Designs

### 1. PCB 3D View

![PCB 3D View](Image1.png)

- ESP32 for WiFi/Bluetooth and microcontroller tasks.
- Motor drivers and sensor interfaces for automation.
- Multiple connectors for easy wiring and expansion.

---

### 2. Schematic Diagram

![PCB Schematic](Image2.png)

- **Power Supply:** Supports stable 3.3V/5V operation.
- **Motors:** DC and servo control for sorting mechanisms.
- **Sensor Integration:** Hall sensors, ADC for feedback.
- **ESP32:** Central controller for all logic.
- **USB to UART:** For programming and debugging.
- **Full-bridge motor driver:** For bidirectional control.

---

### 3. CAD Design — Front View

![Bin CAD Front](Image3.png)

- Sturdy cylindrical bin.
- Overhead camera mount for waste detection.
- Rotating/tilting mechanism for sorting.

---

### 4. CAD Design — Tilted View

![Bin CAD Tilted](Image4.png)

- Side view showing internal mechanisms.
- Designed for easy waste flow and sorting.

---

## 🧠 Machine Learning Script

The Python script below uses your trained Roboflow model to detect waste from a live webcam feed and overlays the result on the video stream.

```python
import roboflow
import cv2
import time

# Initialize Roboflow and your project
rf = roboflow.Roboflow(api_key="eFgxMTrXGckk9vBVPbw8")
project = rf.workspace().project("waste_segregation-fz6et")
model = project.version(1).model

# Start video capture (0 for laptop cam, 1 for external cam)
cap = cv2.VideoCapture(1, cv2.CAP_DSHOW)
if not cap.isOpened():
    raise IOError("Cannot open webcam")

last_pred_time = time.time()

while True:
    ret, frame = cap.read()
    if not ret:
        print("Unable to grab frame !!")
        break

    # Predict every 5 seconds
    if time.time() - last_pred_time > 5:
        cv2.imwrite("temp_frame.jpg", frame)
        prediction = model.predict("temp_frame.jpg").json()
        print(prediction)
        last_pred_time = time.time()

        # Draw the prediction
        if prediction['predictions']:
            top_prediction = prediction['predictions'][0]
            class_name = top_prediction['class']
            confidence = top_prediction['confidence']
            text = f"{class_name} ({confidence:.2f})"
            cv2.putText(frame, text, (10, 30), cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 255, 0), 2)

    # Show video feed with overlay
    cv2.imshow('Live Waste Detection', frame)

    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
```

---

## 🏆 Features

- **End-to-end solution:** Hardware, software, and ML in one repo.
- **Custom electronics & firmware:** ESP32-based for IoT expansion.
- **Easy integration:** Modular code, clear electronics, and design files.
- **SIH-ready:** Professional documentation and engineering workflow.

---

## 📁 Repository Structure

- `/images/` — CAD renderings, PCB schematics, and photos
- `final_test_webcam.py` — Main ML inference script
- `/hardware/` — PCB design and schematics
- `/cad/` — Mechanical design files

---

## 👥 Contributors

- Krish Patel (@krishp1002)
- Team EcoVision VGEC

---

## 📝 License

This project is for SIH 2025 demonstration and academic purposes. Please contact the author for other usage.

---

> Designed & engineered by Team EcoVision_VGEC for Smart India Hackathon 2025.


# Verify-RFID-based-facial-recognition-attendance-system


A Raspberry Pi-based smart attendance system that combines RFID card scanning (MFRC522) with facial recognition (Dlib). The system manages dual-layer authentication for students and lecturers, stores data locally in SQLite, and syncs live session data to Firebase Firestore. It also includes GPIO-driven LED and buzzer feedback.

## Hardware Requirements
* **Raspberry Pi** (Tested on Raspberry Pi 3/4)
* **MFRC522 RFID Reader**
* **USB Webcam or Pi Camera**
* **LEDs:** Yellow (Success) and Red (Error/Duplicate)
* **Buzzer:** Active buzzer for audio feedback
* Jumper wires & Breadboard

### GPIO Pin Configuration (BCM Mode)
* **Yellow LED:** GPIO 17
* **Red LED:** GPIO 27
* **Buzzer:** GPIO 22
* *RFID Pins:* Standard SPI pins (SDA, SCK, MOSI, MISO, GND, 3.3V)

## Software Prerequisites

1. Update your Raspberry Pi system packages:
   ```bash
   sudo apt-get update
   sudo apt-get upgrade
   sudo apt-get install cmake libopenblas-dev liblapack-dev libx11-dev libgtk-3-dev
Install the required Python dependencies:

Bash
pip install -r requirements.txt
Setup & Installation
Clone the Repository

Bash
git clone <your-repository-url>
cd <repository-folder>
Add Dlib Models
Create a data/data_dlib/ directory and download the pre-trained models into it:

shape_predictor_68_face_landmarks.dat

dlib_face_recognition_resnet_model_v1.dat

Add Firebase Credentials
Place your Firebase project's serviceAccountKey.json directly into the root folder. Do not commit this file to version control.

Usage
Run the main script:

Bash
python3 rfid_reader.py
Upon launching, the terminal will display a menu with the following options:

Register New Student: Scans a new RFID card, takes a name, and captures multiple facial angles to generate a biometric profile.

Register Lecturer: Scans a lecturer's RFID card and associates it with a specific course name and code.

Start Continuous Attendance Process:

Students tap their RFID cards to register their initial presence.

The lecturer taps their card to initiate the active class session in Firebase.

The camera opens for a 30-second window, matching the faces of all scanned students to verify attendance fully.

Final status (Present or Partial) is logged to SQLite and synced to Firebase.

Exit: Safely cleans up the GPIO pins and shuts down the system.

Database Structure
Local (SQLite - attendance.db)
students: Stores unique_id, name, rfid_id, and binary face_descriptors.

lecturers: Stores unique_id, name, rfid_id, course_name, and course_code.

attendance_log: Stores daily logs including both RFID tap time and face verification time.

Cloud (Firebase Firestore)
Collection lectures: Documents are created for each session (e.g., COURSECODE_YYYYMMDD_HHMMSS) containing start time and lecturer details.

Sub-collection attendance: Live attendance logs for each verified student in that specific session.

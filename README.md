# Hand Gesture LED Control (MediaPipe + Arduino)

Control LEDs with hand gestures using **MediaPipe hand tracking** on a laptop and an **Arduino (Mega)** for the physical output.  
The model runs on the computer, detects finger states, then sends commands to the Arduino over **Serial (USB)**.

## What it does
- Tracks a hand in real time (21 keypoints)
- Determines which fingers are up / recognizes a gesture (depending on your logic)
- Sends a simple command to Arduino (e.g., `0–5`, `ON/OFF`, or a gesture label)
- Arduino updates LEDs (and optional LCD) based on the command

## How it works (high level)
1. **Camera → MediaPipe**: detects 21 hand landmarks
2. **Gesture logic (Python)**: converts landmarks into “finger up count” or specific gestures
3. **Serial link**: Python sends a short message like `3\n`
4. **Arduino**: reads the message and updates LEDs

## Demo
> Add your demo images/video after you upload them to `images/`

![Demo](images/DEMO.jpg)

## Documentation
- 📄 [Getting Started (install + wiring + run steps)](docs/GETTING_STARTED.md)

## Hardware
- Arduino Mega 2560 (or your board)
- LEDs + resistors (e.g., 220Ω)
- Breadboard + jumper wires
- USB cable
- Laptop with a camera

## Software
- Python
- OpenCV
- MediaPipe
- PySerial
- Arduino IDE

## Notes
- The **AI model runs on the laptop**, not on the Arduino.
- The Arduino is used for **real-world output** (LEDs/LCD/motors).

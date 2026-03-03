# Project 1: Color Memory Game (Arduino Mega) — v2 (LCD Upgrade)

## Build

### Final build
![Final build](../images/FINAL_BUILD_V2.jpg)

### Wiring / breadboard overview
![Wiring overview](../images/WIRING_OVERVIEW_V2.jpg)

### LCD close-up (optional)
![LCD close-up](../images/LCD_CLOSEUP_V2.jpg)

---

## Notes
This is **Version 2** of my Arduino Mega Simon-style color memory game.  
v1 was LEDs + buttons only. In v2, I added a **16x2 LCD** so the game can show **round/score**, prompts like **“Watch”** and **“Your turn”**, and a cleaner **game over** message.

---

## Hardware Overview
**Main board:** Arduino Mega 2560  
**Inputs:** 4 push buttons (player input)  
**Outputs:** 4 LEDs (pattern display) + 16x2 LCD (UI)

---

## Pin Map (v2)

### LED pins
- LED1 (Red)    → D2  
- LED2 (Green)  → D3  
- LED3 (Blue)   → D4  
- LED4 (Yellow) → D5  

### Button pins (recommended: `INPUT_PULLUP`)
- BTN1 → D6  
- BTN2 → D7  
- BTN3 → D8  
- BTN4 → D9  
**Logic:** pressed = `LOW`, released = `HIGH`

### LCD wiring (choose your type)

#### Option A — I2C LCD (recommended if your LCD has an I2C backpack)
- VCC → 5V  
- GND → GND  
- SDA → SDA (Mega SDA pin)  
- SCL → SCL (Mega SCL pin)  

> Mega SDA/SCL are the dedicated SDA/SCL pins near AREF (or SDA = D20, SCL = D21 on many Mega boards).

#### Option B — Parallel 16x2 LCD (if no I2C backpack)
Use the LiquidCrystal library (RS, E, D4–D7).
Example pin plan (change if needed):
- RS → D10  
- E  → D11  
- D4 → D12  
- D5 → D13  
- D6 → D14  
- D7 → D15  
- VSS → GND  
- VDD → 5V  
- VO  → contrast potentiometer middle pin  
- A/K → backlight (if used)

---

## Wiring Guide

### LEDs
For each LED:
1. **Anode (long leg)** → Arduino pin (D2–D5)
2. **Cathode (short leg)** → **220Ω resistor** → GND

✅ Each LED needs its own resistor.

### Buttons (`INPUT_PULLUP`)
For each button:
1. One side → **GND**
2. Other side → Arduino pin (D6–D9)

In code:
- `pinMode(buttonPin, INPUT_PULLUP);`
- pressed = LOW

### LCD
- Wire LCD using **Option A (I2C)** or **Option B (parallel)** from the pin map section.
- Keep all grounds common.

---

## How the Game Works
1. Arduino stores a sequence (example: `2, 0, 3, 1`)
2. Each round adds **one new random** step to the sequence
3. LEDs play the full sequence
4. Player repeats using buttons
5. Correct → next round  
   Wrong → game over

---

## LCD UI Plan (v2)
Screens I’m aiming for:
- **Startup:** `Color Memory Game`
- **Watch:** `Watch the pattern`
- **Input:** `Your turn`
- **Round/Score:** `Round: X` and/or `Score: X`
- **Game Over:** `Game over` + `Score: X`

---

## Code Structure (suggested)
- `setup()`
  - init LED pins
  - init button pins (`INPUT_PULLUP`)
  - init LCD
  - show startup screen

- `loop()` (simple state machine)
  - `SHOW_PATTERN` (LED playback + LCD “Watch”)
  - `READ_INPUT` (LCD “Your turn”)
  - `CHECK_INPUT`
  - `NEXT_ROUND` / `GAME_OVER`

---

## Upload & Test Checklist
- [ ] LEDs all light correctly (one by one test)
- [ ] Buttons register correctly (pressed = LOW)
- [ ] LCD powers on and displays text
- [ ] Pattern playback timing looks right
- [ ] Correct inputs progress to next round
- [ ] Wrong input triggers game over + LCD message
- [ ] LCD updates round/score properly

---

## Troubleshooting

### LCD shows blocks / blank screen (common)
- Adjust contrast (potentiometer) if parallel LCD
- If I2C: check SDA/SCL wiring and confirm I2C address in code

### LCD not showing at all
- Check 5V and GND
- Check you didn’t wire SDA/SCL incorrectly (I2C)
- If parallel: confirm RS/E/D4–D7 match your code pins

### Buttons seem inverted / always pressed
- Make sure you are using `INPUT_PULLUP`
- Button should connect to **GND** when pressed

### LEDs not working
- Check polarity (long leg to pin)
- Confirm resistors are in series
- Confirm correct pin numbers

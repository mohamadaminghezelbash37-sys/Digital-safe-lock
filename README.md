# Digital-safe-lock
Digital safe lock with rotary encoder, 16x2 LCD, and solenoid actuator — built on ATmega32 (CodeVisionAVR)

# 🔐 Digital Safe Lock

A microcontroller-based digital lock system for a safe. The user enters a password using a **rotary encoder** and a **character LCD**, the system stores/validates it, and activates a **solenoid** to unlock when the correct password is entered.

## ✨ Features

- No factory-default password — the user sets their own password on first use
- 6-digit password
- Each digit is selected by rotating the rotary encoder (negative, zero, and positive values)
- Each digit is confirmed by pressing (clicking) the encoder's built-in button
- The currently selected digit is shown live on the LCD while entering the password
- A separate physical button to enter "change password" mode (recommended to be mounted inside the safe body so it's only accessible to the owner)
- Correct password → solenoid activates and the lock opens
- Incorrect password → an error message is shown on the LCD

## 🧩 Hardware Components

| Component | Description |
|---|---|
| Microcontroller | ATmega32 |
| Compiler / IDE | CodeVisionAVR (CVAVR) |
| Crystal | External 16 MHz crystal |
| Fuse bits | `JTAGEN = 1` (disables JTAG to free up Port C pins), `CKSEL0..3 = 1` (selects external crystal oscillator) |
| Rotary encoder | Used to select digits and confirm them via click |
| LCD | 16x2 character LCD |
| Change-password button | Enters password setup/change mode |
| Solenoid lock | Physical locking mechanism — 12V supply |
| 7805 voltage regulator | Steps down the 12V solenoid supply to 5V to power the microcontroller and LCD |
| Relay or suitable driver (transistor/MOSFET + flyback diode) | Drives the 12V solenoid from the 5V microcontroller output |

## ⚙️ How It Works

### 1. Setting / Changing the Password
1. Hold down the "change password" button.
2. Rotate the encoder to select the desired digit (shown on the LCD).
3. Press (click) the encoder to confirm/store that digit.
4. Repeat steps 2–3 until **6 digits** are entered.
5. The new password is saved.

### 2. Unlocking
1. Rotate the encoder to select each digit.
2. Click the encoder to confirm the digit (shown on the LCD).
3. After all 6 digits are entered, the password is checked:
   - ✅ Correct → the solenoid activates and the lock opens.
   - ❌ Incorrect → an "Incorrect password" message is shown on the LCD.

## 🔋 Power

- The 12V input supply powers the solenoid directly (via a relay/driver) and, after passing through a **7805** regulator, is stepped down to 5V to power the microcontroller and LCD.
- It's recommended to add decoupling capacitors on the 7805's input and output (e.g. 100nF and 10µF–100µF) for voltage stability.
- Always drive the solenoid through a transistor/MOSFET driver with a flyback diode across the solenoid to protect the microcontroller from back-EMF when the coil is switched off.



## 📁 Suggested Project Structure

```
Digital-safe-lock/
├── README.md
├── src/
│   └── safe_lock.c        # Main project source (CodeVisionAVR)
├── Images/
    └── wiring.png         # Circuit photo

```

## 🚀 Setup & Usage

1. Wire the circuit according to the wiring diagram.
2. Open the source file (`src/safe_lock.c`) in CodeVisionAVR.
3. Configure the fuse bits as listed above, select the target chip (ATmega32) and programmer, then compile and flash.
4. On first use, set the initial password following the steps above.

## 📌 Possible Future Improvements

- Temporary lockout after several failed attempts
- Store the password in EEPROM so it persists after power loss
- Add a buzzer alert for repeated failed attempts

## 📄 License

This project is released under the [MIT License](LICENSE). *(feel free to choose a different license if you prefer)*

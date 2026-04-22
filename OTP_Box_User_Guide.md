# OTP Box — User Guide

> **Version:** 1.0
> **Manufacturer:** Rigbetel Labs
> **Device:** OTP Box Smart Lock Module

---

## Table of Contents

1. [What is OTP Box?](#1-what-is-otp-box)
2. [How It Works](#2-how-it-works)
3. [Device Overview](#3-device-overview)
4. [Connecting via Wi-Fi (Web Portal)](#4-connecting-via-wi-fi-web-portal)
5. [Using the Physical Keypad](#5-using-the-physical-keypad)
6. [Understanding the Display](#6-understanding-the-display)
7. [Setting an OTP (Operators)](#7-setting-an-otp-operators)
8. [Unlocking the Box (Recipients)](#8-unlocking-the-box-recipients)
9. [Device States Explained](#9-device-states-explained)
10. [Security Features](#10-security-features)
11. [Troubleshooting](#11-troubleshooting)
12. [Technical Specifications](#12-technical-specifications)

---

## 1. What is OTP Box?

The **OTP Box** is a smart lock module designed to secure autonomous delivery robot cargo during transit. It uses a **One-Time Password (OTP)** system to ensure that only the intended recipient can open the delivery box at the destination.

The device supports two independent ways to control and interact with it:

- **Web Portal** — Connect via Wi-Fi from any smartphone or browser
- **ROS2 Integration** — For robot operators and automated systems

The OTP Box is always active and ready to use regardless of whether a robot is connected or not.

---

## 2. How It Works

The delivery workflow is straightforward:

```
1. Operator sets a 6-digit OTP on the device (via web portal or robot system)
2. Box locks automatically and countdown begins
3. Robot transports the cargo to the destination
4. Recipient connects to OTP Box Wi-Fi (or uses keypad)
5. Recipient enters the 6-digit OTP
6. Box unlocks for a set duration, then re-locks automatically
```

The OTP is only valid for a configured time window (default: 90 seconds). If the OTP expires before the recipient enters it, the operator must issue a new one.

---

## 3. Device Overview

### Front Panel

| Element | Description |
|---|---|
| **16×2 LCD Display** | Shows current status, countdown timers, and prompts |
| **4×4 Keypad** | 16 keys (0–9, \*, #, A–D) for entering OTP and controlling the device |

### Key Layout

```
[ 1 ][ 2 ][ 3 ][ A ]
[ 4 ][ 5 ][ 6 ][ B ]
[ 7 ][ 8 ][ 9 ][ C ]
[ * ][ 0 ][ # ][ D ]
```

> **Note:** Keys A, B, C, D and \* are reserved for future use. Only 0–9 and # are used in normal operation.

### Indicator Keys

| Key | Action |
|---|---|
| `0`–`9` | Enter OTP digits |
| `#` | Clear / reset current input |
| Hold `0` for 3 seconds | Display the current ROS2 Domain ID |
| Hold `0` for 6 seconds | Restart the device |

---

## 4. Connecting via Wi-Fi (Web Portal)

The OTP Box broadcasts its own Wi-Fi network at all times. No internet connection is required.

### Connection Steps

1. On your smartphone or laptop, open **Wi-Fi Settings**
2. Connect to the network:
   - **Network Name (SSID):** `OTP_BOX`
   - **Password:** `rbl@otpbox`
3. Once connected, open a browser and go to: **http://192.168.4.1**
   - On most Android and iOS devices, you will be automatically redirected to the portal after connecting

> **Tip:** On Android and iOS, a "Sign in to network" notification will appear automatically after connecting — tap it to open the portal directly.

### Web Portal Features

The web portal provides:

- **Live status dashboard** — Current state, countdown timer, and progress bar
- **Set OTP form** — Configure OTP code, validity duration, and unlock duration
- **Reset button** — Clear the current OTP and return to idle state
- **Auto-refresh** — Status updates every 1.5 seconds automatically

---

## 5. Using the Physical Keypad

### Entering an OTP

1. When the display shows `Enter OTP:`, you can begin typing
2. Press digit keys `0`–`9` to enter your 6-digit code
3. As you type, each digit appears on the bottom row of the display
4. The box **automatically submits** after the 6th digit — no confirmation key needed
5. Press `#` at any time to clear your input and start over

### Display while typing

The bottom row shows your progress as you type:

```
______   → No digits entered yet
1_____   → First digit entered
12____   → Second digit entered
123456   → All 6 digits entered (auto-submits)
```

---

## 6. Understanding the Display

The 16×2 LCD shows the current device status at all times.

### Display Messages Reference

| Top Row | Bottom Row | Meaning |
|---|---|---|
| `OTP Box Ready` | `Awaiting Mission` | Device is idle, no OTP set |
| `Enter OTP: 87s` | `______` | OTP is active, 87 seconds remaining to enter |
| `Enter OTP: 87s` | `123___` | OTP active, user has typed 3 digits so far |
| `Access Granted!` | `Locking in: 10s` | Correct OTP entered, box is unlocked |
| `Wrong OTP!` | `Attempt: 2/3` | Incorrect OTP, 2nd attempt used |
| `ACCESS DENIED` | `Contact Operator` | 3 failed attempts, box is locked out |
| `Connecting ROS2` | `Please wait...` | Device is starting up |
| `  ROS Domain ID` | `    ID: 0` | Domain ID shown on startup |

> **Note:** The countdown timer on the top row updates every second in real time without any screen flicker.

---

## 7. Setting an OTP (Operators)

Operators can set the OTP using either the **web portal** or the **ROS2 robot system**.

### Via Web Portal

1. Connect to `OTP_BOX` Wi-Fi and open `http://192.168.4.1`
2. Fill in the form:
   - **OTP Code** — A 1 to 6 digit number (e.g., `482916`). Single digits are padded to 6 digits automatically.
   - **Valid for (seconds)** — How long the OTP remains active (default: 90s, range: 10–3600s)
   - **Unlock duration (seconds)** — How long the box stays open after correct entry (default: 10s, range: 1–300s)
3. Click **Set OTP**
4. The display immediately updates to `Enter OTP:` and the countdown begins

### Via ROS2 (Robot System)

Publish to the `/otp_box/set_otp` topic with:
- `data[0]` — OTP code as an integer (e.g., `482916`)
- `data[1]` *(optional)* — Validity timeout in seconds (default: `90`)
- `data[2]` *(optional)* — Unlock duration in seconds (default: `10`)

To reset/clear the OTP: publish with `data[0] = 0`

Monitor status on the `/otp_box/get_otp` topic (publishes at 1 Hz).

> **Note:** If both the web portal and ROS2 system set an OTP, the most recent one takes effect.

---

## 8. Unlocking the Box (Recipients)

Once the operator has set an OTP, the recipient follows these steps:

1. **Check the display** — Confirm it shows `Enter OTP:` with time remaining
2. **Enter the 6-digit OTP** using the keypad (or use the web portal if provided)
3. **Wait for confirmation:**
   - **Success:** Display shows `Access Granted!` and the box unlocks
   - **Failure:** Display shows `Wrong OTP!` and the attempt count increases
4. **Retrieve your package** before the unlock duration expires
5. The box **re-locks automatically** when the countdown reaches zero

> **Important:** You have a maximum of **3 attempts** to enter the correct OTP. After 3 failed attempts, the box enters lockout mode and the operator must issue a new OTP.

---

## 9. Device States Explained

The OTP Box operates in four clear states:

### IDLE
- No OTP has been set
- Display: `OTP Box Ready` / `Awaiting Mission`
- The box is mechanically locked but no OTP is required to change this state
- Waiting for an operator to set an OTP

### LOCKED
- An OTP has been set and is active
- Display: `Enter OTP: Xs` with countdown
- Recipient must enter the correct OTP before time runs out
- Wrong entries decrement the attempt counter

### UNLOCKED
- Correct OTP was entered
- Display: `Access Granted!` / `Locking in: Xs`
- Box is physically open for the configured unlock duration
- Automatically transitions back to IDLE when time expires

### ERROR (Lockout)
- 3 incorrect OTP attempts were made
- Display: `ACCESS DENIED` / `Contact Operator`
- Box will not accept further entries
- Only the operator can reset this by issuing a new OTP

### State Transition Diagram

```
           Set OTP
  IDLE ────────────→ LOCKED
   ↑                    │
   │           Correct  │  Wrong OTP
   │           OTP      │  (×3 max)
   │                    ↓
   │ Time expires   UNLOCKED      ERROR
   └────────────── ────┘            │
                                    │  New OTP issued
                                    └──────────────→ LOCKED
```

---

## 10. Security Features

### One-Time Use
The OTP is consumed once the box is unlocked. To lock and re-use the box, a new OTP must be set.

### Time-Limited Access
Every OTP has an expiry window. An OTP cannot be used after it expires, even if it was never used.

### Attempt Limiting
Only 3 attempts are allowed per OTP. Brute-force entry is not possible.

### Auto Re-Lock
The box re-locks automatically after the unlock window closes. The recipient cannot keep the box open indefinitely.

### Isolated Network
The OTP Box Wi-Fi is a local-only access point. No internet connection is made and no data is transmitted outside the device.

---

## 11. Troubleshooting

### The box display shows "Connecting ROS2 / Please wait..."

The device is still starting up and connecting to the robot's ROS2 system. Wait a few seconds. The web portal is still accessible during this time.

### I can't find the `OTP_BOX` Wi-Fi network

- Ensure the device is powered on (display should be lit)
- Move closer to the device — the Wi-Fi range is short (~10 meters)
- Refresh your device's Wi-Fi network list
- The network name is case-sensitive: `OTP_BOX`

### The web portal doesn't open at `http://192.168.4.1`

- Confirm you are connected to `OTP_BOX` and not your regular Wi-Fi
- Try navigating to `http://192.168.4.1` manually in the browser address bar
- Disable mobile data on your phone before connecting (some phones prefer mobile data over captive portals)

### I entered the OTP but the box didn't open

- Ensure you entered all **6 digits** exactly as provided
- OTPs are zero-padded — if your OTP is `123`, you must enter `000123`
- Check if the OTP timer had already expired on the display
- Confirm this is the correct OTP Box unit

### Display shows "ACCESS DENIED / Contact Operator"

You have used all 3 attempts. The device is locked out. Contact the delivery operator to issue a new OTP.

### The OTP expired before I could enter it

Contact the operator. They need to issue a new OTP with a longer validity window.

### The box unlocked but re-locked before I could access it

Contact the operator. They can set a longer unlock duration (up to 300 seconds) when issuing the next OTP.

### The device is unresponsive

Hold the `0` key for 6 seconds to restart the device. The display will show `Restarting...` and the device will reboot.

---

## 12. Technical Specifications

| Parameter | Value |
|---|---|
| **OTP Length** | 6 digits (000000–999999) |
| **Default OTP Validity** | 90 seconds |
| **OTP Validity Range** | 10 – 3600 seconds |
| **Default Unlock Duration** | 10 seconds |
| **Unlock Duration Range** | 1 – 300 seconds |
| **Max Entry Attempts** | 3 |
| **Wi-Fi Network Name** | `OTP_BOX` |
| **Wi-Fi Password** | `rbl@otpbox` |
| **Web Portal Address** | `http://192.168.4.1` |
| **Display** | 16×2 LCD |
| **Keypad** | 4×4 matrix (16 keys) |
| **Status Refresh Rate** | Every 1.5 seconds (web) / Every 1 second (display) |
| **ROS2 Status Topic** | `/otp_box/get_otp` (1 Hz) |
| **ROS2 Control Topic** | `/otp_box/set_otp` |

---

*For support or inquiries, contact Rigbetel Labs.*

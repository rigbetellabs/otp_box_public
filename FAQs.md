# OTP Box — Frequently Asked Questions (FAQs)

> **Rigbetel Labs | OTP Box Smart Lock Module**

---

## Table of Contents

- [General Questions](#general-questions)
- [Wi-Fi & Web Portal](#wi-fi--web-portal)
- [Using the Keypad](#using-the-keypad)
- [Setting & Managing OTPs (Operators)](#setting--managing-otps-operators)
- [Unlocking the Box (Recipients)](#unlocking-the-box-recipients)
- [Errors & Lockouts](#errors--lockouts)
- [Timing & Countdowns](#timing--countdowns)
- [Security & Privacy](#security--privacy)
- [Device Behavior](#device-behavior)
- [ROS2 / Robot System Integration](#ros2--robot-system-integration)

---

## General Questions

**Q: What is OTP Box?**

A: OTP Box is a smart lock module designed for autonomous delivery robots. It secures cargo during transit using a One-Time Password (OTP) system. Only a recipient with the correct 6-digit code can open the box at the destination.

---

**Q: Who makes the OTP Box?**

A: OTP Box is designed and manufactured by **Rigbetel Labs**.

---

**Q: Do I need an internet connection to use it?**

A: No. The OTP Box creates its own local Wi-Fi network. No internet connection is needed for either the web portal or any device functions.

---

**Q: Can the OTP Box work without a robot connected?**

A: Yes. The web portal works completely independently. The device can be controlled entirely through the web portal even when no robot system is present.

---

**Q: What are the two ways to interact with the OTP Box?**

A: There are two independent methods:
1. **Web Portal** — Connect to the `OTP_BOX` Wi-Fi network and access the dashboard at `http://192.168.4.1`
2. **Physical Keypad** — Use the 4×4 keypad on the device to enter OTP codes

Operators can also use the **ROS2 robot system** to set and monitor OTPs programmatically.

---

**Q: Is the OTP Box waterproof?**

A: The OTP Box module itself is not rated for outdoor or wet environments. Check with Rigbetel Labs for enclosure and deployment recommendations specific to your use case.

---

## Wi-Fi & Web Portal

**Q: What is the Wi-Fi network name and password?**

A:
- **Network Name (SSID):** `OTP_BOX`
- **Password:** `rbl@otpbox`

---

**Q: What is the web portal address?**

A: `http://192.168.4.1` — Open this in any browser after connecting to the `OTP_BOX` Wi-Fi network.

---

**Q: Why does my phone automatically open a web page when I connect?**

A: The OTP Box uses a **captive portal** — the same technology used by hotel and airport Wi-Fi networks. After connecting, your phone detects the portal and shows a "Sign in to network" prompt. Tapping it opens the OTP Box dashboard automatically.

---

**Q: The web portal doesn't open automatically. What do I do?**

A: Open your browser manually and type `http://192.168.4.1` in the address bar. Make sure:
- You are connected to `OTP_BOX` (not your regular Wi-Fi)
- Mobile data is turned off on your phone (some phones prefer mobile data over captive portals)

---

**Q: Can multiple people connect to the web portal at the same time?**

A: The portal can serve multiple browsers, but there is only one OTP Box and one lock state. If two operators set an OTP at the same time, the most recently submitted OTP takes effect.

---

**Q: How far away can I be from the device and still connect?**

A: The Wi-Fi range is approximately 10–15 meters in open space. Walls and obstructions reduce this range.

---

**Q: Does the web portal work on all devices?**

A: Yes. The web portal is designed to work on any modern browser including Chrome, Safari, Firefox, and Edge on smartphones, tablets, and laptops.

---

**Q: What does the web portal show?**

A: The portal dashboard shows:
- Current device state (IDLE, LOCKED, UNLOCKED, or ERROR)
- Live countdown timer and progress bar
- A form to set the OTP, validity window, and unlock duration
- A Reset button to clear the OTP

---

**Q: Does the web portal update automatically?**

A: Yes. The status dashboard refreshes every 1.5 seconds without needing to reload the page.

---

## Using the Keypad

**Q: How do I enter an OTP on the keypad?**

A: When the display shows `Enter OTP:`, press the digit keys (`0`–`9`) one at a time. The box automatically submits after the 6th digit — you do not need to press Enter or any confirmation key.

---

**Q: What does the `#` key do?**

A: The `#` key clears your current input. If you make a mistake while entering digits, press `#` to reset and start over.

---

**Q: Do the letter keys (A, B, C, D) do anything?**

A: Not in normal use. Keys A, B, C, D and `*` are reserved for future functionality and have no effect in the current firmware.

---

**Q: What happens if I hold the `0` key?**

A:
- **Hold for 3 seconds:** Displays the current ROS2 Domain ID on the screen
- **Hold for 6 seconds:** Restarts the device (display shows `Restarting...`)

---

**Q: The keypad is not responding. What should I do?**

A: Check that the device display is on and shows a status message. If the display is blank, the device may not be powered. If the display is on but keypad is unresponsive, try holding `0` for 6 seconds to restart the device.

---

**Q: Can I enter the OTP using both the keypad and the web portal?**

A: Recipients can enter the OTP via the keypad only (the web portal is for operators to *set* the OTP). The keypad is the intended entry method for recipients at the delivery point.

---

## Setting & Managing OTPs (Operators)

**Q: How do I set an OTP?**

A: Connect to the `OTP_BOX` Wi-Fi, open the web portal at `http://192.168.4.1`, fill in the OTP form, and click **Set OTP**. You can also set OTPs via the ROS2 robot system.

---

**Q: What format should the OTP be in?**

A: The OTP is a number between 1 and 999999. It is always treated as a **6-digit zero-padded** code on the device. For example:
- You enter `123` → device stores and expects `000123`
- You enter `482916` → device expects `482916`

---

**Q: Can I set any 6-digit number as an OTP?**

A: Yes, any number from `000001` to `999999` is valid. `000000` (zero) is reserved for resetting/clearing the OTP.

---

**Q: How long is an OTP valid?**

A: By default, 90 seconds. You can configure this between 10 seconds and 3600 seconds (1 hour) when setting the OTP.

---

**Q: How do I give the recipient more time to enter the OTP?**

A: When setting the OTP on the web portal, increase the **"Valid for (seconds)"** field. You can set up to 3600 seconds (1 hour).

---

**Q: Can I change the OTP after it has been set?**

A: Yes. Submit a new OTP at any time via the web portal or ROS2. The new OTP immediately replaces the old one and the countdown resets.

---

**Q: How do I cancel or clear the OTP?**

A: Click the **Reset** button on the web portal. This clears the OTP and returns the device to the IDLE state.

---

**Q: Can I set different unlock durations each time?**

A: Yes. The **unlock duration** (how long the box stays open after the correct OTP is entered) can be configured each time you set an OTP, from 1 to 300 seconds.

---

**Q: What if both the web portal and the robot system set an OTP at the same time?**

A: The most recently submitted OTP takes effect. There is no conflict — the last one to arrive wins.

---

**Q: How can I monitor the device status remotely?**

A: Operators using ROS2 can monitor the `/otp_box/get_otp` topic, which publishes a human-readable status string every second. Web users can watch the live dashboard on the portal.

---

## Unlocking the Box (Recipients)

**Q: How do I open the box as a recipient?**

A:
1. Look at the display — it should show `Enter OTP:` with a countdown
2. Enter the 6-digit OTP code on the keypad
3. The box unlocks when the correct code is entered
4. Retrieve your package before the display shows `Locking in: 0s`

---

**Q: Do I need to connect to Wi-Fi to open the box?**

A: No. Recipients use the physical keypad to enter the OTP. Wi-Fi is used by operators to set OTPs or monitor status. You do not need to connect to Wi-Fi just to open the box.

---

**Q: What if the OTP I was given is only 3 or 4 digits?**

A: Pad it with leading zeros to make 6 digits. For example, if you were told the OTP is `123`, enter `000123` on the keypad.

---

**Q: How do I know if I entered the OTP correctly?**

A:
- **Correct:** Display shows `Access Granted!` and the lock opens
- **Incorrect:** Display shows `Wrong OTP!` and shows your remaining attempts

---

**Q: The countdown expired before I could enter the OTP. What happens?**

A: The OTP becomes invalid and the box returns to IDLE state. Contact the operator to issue a new OTP.

---

**Q: The box unlocked but closed before I finished. What do I do?**

A: Contact the operator. They can issue a new OTP with a longer unlock duration (up to 5 minutes / 300 seconds).

---

## Errors & Lockouts

**Q: The display shows "ACCESS DENIED / Contact Operator". What happened?**

A: You (or someone) entered the wrong OTP 3 times. The device is now in lockout mode to prevent unauthorized access. The operator must issue a new OTP to reset the device.

---

**Q: How many attempts do I get to enter the OTP?**

A: You have **3 attempts** per OTP. After 3 wrong entries, the device enters lockout (ERROR state) and must be reset by the operator.

---

**Q: Does the attempt counter reset if I press `#` to clear my input?**

A: No. The `#` key only clears what you have *typed so far* before submitting. The attempt counter only increases when you actually submit an incorrect OTP (i.e., enter the 6th digit).

---

**Q: Can the operator reset a lockout remotely?**

A: Yes. The operator simply sets a new OTP via the web portal or robot system. This immediately clears the lockout and transitions the device back to LOCKED with the new code.

---

**Q: What if the device freezes or becomes unresponsive?**

A: Hold the `0` key on the keypad for 6 seconds. The display will show `Restarting...` and the device will reboot. The operator will need to set a new OTP after restarting.

---

**Q: The display shows nothing / is blank. Is the device off?**

A: The device may not be powered. Check that the power supply is connected. If the device was previously working, try the 6-second hold restart. If the display remains blank, contact Rigbetel Labs support.

---

## Timing & Countdowns

**Q: What is "OTP Validity"?**

A: The time window during which the OTP can be entered. Once this timer runs out, the OTP is no longer valid and the device returns to IDLE. Default is 90 seconds.

---

**Q: What is "Unlock Duration"?**

A: The amount of time the box stays physically open after a correct OTP is entered. After this timer expires, the box re-locks automatically. Default is 10 seconds.

---

**Q: Can the OTP validity be extended after it has been set?**

A: Not directly. However, the operator can submit a new OTP (even the same code) at any time to reset and extend the countdown.

---

**Q: Does the display countdown update in real time?**

A: Yes. The countdown updates every second on the physical display and every 1.5 seconds on the web portal — both without any screen flicker.

---

**Q: Can the box be set to stay open indefinitely?**

A: No. The maximum unlock duration is 300 seconds (5 minutes). The box always re-locks automatically.

---

## Security & Privacy

**Q: Is my OTP transmitted over the internet?**

A: No. The OTP Box operates entirely on a local Wi-Fi network. No data ever leaves the device or reaches the internet.

**Q: Can someone guess the OTP by trying many combinations?**

A: The device allows only **3 attempts** before entering lockout mode. This makes brute-force entry effectively impossible.

---

**Q: Can someone reuse an OTP after the box has been opened?**

A: No. The OTP is consumed when the box unlocks. The device returns to IDLE state and the same OTP cannot be used again unless the operator re-issues it.

---

**Q: Can someone intercept the OTP from the web portal?**

A: The web portal operates on a local Wi-Fi network (not the internet). Only devices connected to the `OTP_BOX` network can access the portal. Operators should share OTP codes through secure out-of-band channels (e.g., SMS, app notification) rather than over public networks.

---

**Q: Can two people unlock the box with the same OTP?**

A: No. Once the correct OTP is entered and the box unlocks, the OTP is cleared. A new OTP must be issued for any subsequent access.

---

**Q: Is the Wi-Fi network encrypted?**

A: Yes. The `OTP_BOX` network uses WPA2 encryption (password: `rbl@otpbox`).

---

## Device Behavior

**Q: What happens when the device first powers on?**

A:
1. The display shows the ROS2 Domain ID for 3 seconds
2. The device attempts to connect to the robot system (shows `Connecting ROS2`)
3. Once connected (or after timeout), the display shows `OTP Box Ready / Awaiting Mission`
4. The Wi-Fi portal is available immediately, even during startup

---

**Q: Does the device remember anything after a restart?**

A: The device remembers the **ROS2 Domain ID** across restarts. All OTP state (current OTP, timers, attempt count) is cleared on restart.

---

**Q: What happens if power is cut while the box is unlocked?**

A: The box will lock on next power-on. The device always starts in a safe locked state.

---

**Q: Can the device be used standalone without a robot?**

A: Yes. The web portal provides full control and the keypad provides full OTP entry — neither requires a robot system to be present.

---

**Q: Can I control the box from both the web portal and the robot simultaneously?**

A: Yes. Both control paths are independent and always active. The most recent OTP set by either method takes effect.

---

**Q: What does "Domain ID" mean?**

A: The ROS2 Domain ID is a number (0–232) that determines which ROS2 communication network the OTP Box joins. Multiple robot systems on the same physical network can use different Domain IDs to operate independently without interfering with each other. This setting is typically configured by robot system administrators, not end users.

---

**Q: Where can I see the current ROS2 Domain ID?**

A: In three places:
- **On boot** — the LCD shows a full-screen splash `ROS Domain ID / ID: X` for 3 seconds every time the device powers on
- **Keypad** — hold the `0` key for 3 seconds; the current ID appears on the bottom row of the display for 3 seconds
- **Web portal** — the status card always shows a **ROS Domain ID** row with the current value

---

## ROS2 / Robot System Integration

**Q: What ROS2 topics does the OTP Box expose?**

A: The OTP Box uses three topics:

| Topic | Direction | Purpose |
|---|---|---|
| `/otp_box/set_otp` | Subscribe | Set OTP code, validity, and unlock duration |
| `/otp_box/get_otp` | Publish | Current status string (1 Hz) |
| `/otp_box/domain_id` | Subscribe | Change and save the ROS2 Domain ID |

---

**Q: What message type does `/otp_box/set_otp` use?**

A: `std_msgs/Int32MultiArray` with:
- `data[0]` = OTP integer value (e.g., `482916`). Set to `0` to clear.
- `data[1]` *(optional)* = OTP validity in seconds (default: `90`)
- `data[2]` *(optional)* = Unlock duration in seconds (default: `10`)

---

**Q: What does the `/otp_box/get_otp` topic publish?**

A: A human-readable string describing current state. Examples:
- `OTP not set. Publish to /otp_box/set_otp to activate.`
- `OTP: 482916 | Valid for 73 sec | Status: LOCKED`
- `OTP: 482916 | Status: UNLOCKED | Re-locking in 8 sec`
- `OTP: 482916 | Status: ERROR | Max attempts reached. Re-publish to reset.`

---

**Q: What message type does `/otp_box/domain_id` use?**

A: `std_msgs/UInt8` — a single unsigned byte. Valid range is `0` to `232` (the full ROS2 domain ID range). Values above `232` are silently ignored.

---

**Q: How do I change the ROS2 Domain ID?**

A: Publish a `std_msgs/UInt8` message to `/otp_box/domain_id` with the desired ID (0–232):

```bash
ros2 topic pub --once /otp_box/domain_id std_msgs/msg/UInt8 "data: 42"
```

The device will:
1. Validate the value (IDs above 232 are silently ignored)
2. Save the new ID to EEPROM immediately
3. Display a confirmation splash on the LCD
4. Restart automatically to reconnect on the new domain

The Domain ID **cannot** be changed from the web portal or the keypad — only via the ROS2 topic.

---

**Q: Will the Domain ID be lost if the device restarts or loses power?**

A: No. The Domain ID is saved to non-volatile EEPROM memory and is restored on every power-on. The device always displays the active Domain ID on the LCD for 3 seconds during startup so you can confirm the correct value.

---

**Q: The device restarted on its own. Did something go wrong?**

A: Not necessarily. Publishing to the `/otp_box/domain_id` topic causes an intentional restart — this is required to reconnect the microROS agent on the new domain. The LCD will show the new Domain ID before restarting. If you did not publish to that topic, see the Troubleshooting section of the User Guide.

---

**Q: What happens if the ROS2 agent disconnects mid-operation?**

A: The OTP Box continues operating normally. Any active OTP countdown continues, and the keypad and web portal remain fully functional. The device reconnects to the ROS2 agent automatically when it becomes available again.

---

*For further assistance, contact **Rigbetel Labs**.*

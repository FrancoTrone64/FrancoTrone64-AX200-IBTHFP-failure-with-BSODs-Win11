# Intel AX200 – BSOD 0x65 and Phone Link Failure (`IBTHFP.SYS`)

## 📌 Technical Summary

This repository documents a reproducible system crash involving the Intel AX200 Bluetooth stack under Windows 11 24H2 (build 26100.6584).  
The failure occurs during HFP (Hands-Free Profile) negotiation with Phone Link, particularly when PAN and Bluetooth tethering are active.

## 🧠 Crash Analysis

- 5 BSODs documented locally on the PC  
- 4 additional traces from Event Viewer  
- All point to `IBTHFP.SYS` as the faulting module  
- BugCheck: `0x00000065 – MEMORY1_INITIALIZATION_FAILED`

In one selected crash episode (out of 9 total between July 17 and September 24), we documented a direct correlation with Phone Link pairing while PAN was active (PC side) and Bluetooth tethering enabled (mobile side).  
The driver failed to initialize memory structures correctly, resulting in a kernel-level crash.

## 🧪 Observed Behavior (with PAN and Bluetooth tethering disabled)

- Any attempt to establish a stable Bluetooth connection fails with a connection error  
- The failure shows a short time gap between mobile and PC: the PC disconnects a few seconds after the Android device  
- On Android, tapping the settings icon beside the PC name reveals only two toggles: **Calls** and **Sound**, both initially disabled  
- Forcing the **Calls** toggle to ON triggers a connection attempt, but it automatically reverts to OFF within seconds

## 🔍 Comparative Behavior: Dell vs EQR6

Under identical conditions (Galaxy S20 initiating audio channel, PAN and tethering disabled):

- **Dell**: Connection stabilizes as *Connected for calls only*. Android shows two toggles (Calls, Sound), and no need to force Calls ON since it is already active, as suggested by the connection status. With this status, the connection is stable and persistent. Further testing is pending to confirm full Audio Profile functionality.  
- **EQR6**: Identical toggle setup, but forcing Calls to ON triggers a brief connection attempt that fails within seconds. The PC disconnects shortly after the mobile device.

This divergence suggests a difference in how the Bluetooth stack handles HFP negotiation and fallback. The instability observed on EQR6 may point to a failure in `IBTHFP.SYS` to maintain the endpoint under constrained conditions.

## ⚠️ Hardware Sensitivity

Initial assumptions considered the issue as driver-level and hardware-independent.  
However, direct comparison between Dell and EQR6 under identical conditions reveals a critical divergence:

- Dell maintains the HFP connection (*Calls only*), despite limited profile negotiation  
- EQR6 fails to sustain the same connection, resulting in automatic disconnection

It remains unclear whether the *Calls only* connection state represents a valid fallback, a transitional state, or a pathological condition.  
The fact that Dell maintains this state while EQR6 fails to sustain it suggests a hardware-sensitive instability, but does not clarify whether the fallback itself is expected or symptomatic.

This confirms that the issue is hardware-sensitive, and that `IBTHFP.SYS` may behave differently depending on the Bluetooth stack implementation or OEM firmware.  
The escalation must therefore consider hardware-specific behavior and not treat the failure as universally reproducible across all AX200-equipped systems.

## 🧩 Request for Clarification

Formal clarification is requested from Microsoft regarding:
- The current status of HFP (Hands-Free Profile) support in Windows 11  
- The behavior of `IBTHFP.SYS` in PAN + Bluetooth tethering scenarios  
- Whether the documented BugCheck `0x00000065 – MEMORY1_INITIALIZATION_FAILED` is known to occur under such conditions  
- Whether the observed failure pattern — a connection error with a short time gap between mobile and PC (the PC disconnects seconds after the Android device) — reflects a known limitation or a physiological fallback  
- Whether the Android-side behavior — showing only two toggles (Calls and Sound, both initially disabled), and reverting the *Connection for Calls* to OFF with minimal delay compared to the Android device — indicates a misnegotiation or unsupported profile state  
- Whether the instability of HFP negotiation is known to affect specific hardware implementations (e.g., EQR6), despite successful pairing and profile activation on other systems (e.g., Dell)

## 📂 Included Files

- `dumps/` – 5 crash dumps  
- `eventviewer/` – 4 raw event logs (included for completeness, not directly analyzed)  
- `screenshots/` – visual evidence of all BSOD traces, but no scientific proof of direct correlation with Phone Link failure

## 📣 Escalation Status

- ✅ Official Feedback Hub report submitted on **October 1, 2025 at 12:00 CET**  
- Category: *Devices and Drivers > Bluetooth > Other Problems*  
- Status: Awaiting engineering review by Microsoft

## 🔗 External Threads

- [Reddit – Beelink Official Thread](https://www.reddit.com/r/BeelinkOfficial/comments/1ntb0b5/comment/ngysl52)
- [Intel Community – risposta moderatore + SSU](https://community.intel.com/t5/Wireless/AX200-Bluetooth-BSOD-0x65-with-IBTHFSP-SYS-during-Phone-Link/m-p/1720167#M61778) - *(Diagnostic Tool output requested by Intel and included in the link)*
---

This repository consolidates all findings and invites validation from technically competent actors.  
It replaces earlier drafts and ensures full traceability of the escalation process.

While the escalation currently involves Microsoft (driver layer), Beelink (OEM), and Android (mobile pairing), the presence of Intel as the vendor of the AX200 chip is structurally relevant.  
Given the hardware-sensitive nature of the issue, Intel is implicitly part of the diagnostic loop and may hold critical insight into firmware-level behavior or Bluetooth stack implementation.

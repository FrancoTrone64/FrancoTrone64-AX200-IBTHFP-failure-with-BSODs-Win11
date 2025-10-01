# AX200-IBTHFP failure with BSODs – Windows 11

This repository documents a reproducible BSOD (`0x00000065 – MEMORY1_INITIALIZATION_FAILED`) involving `IBTHFP.SYS` during HFP (Hands-Free Profile) negotiation with Phone Link on Intel AX200 under Windows 11 24H2.

## 🔍 Summary

- 5 crash dumps and 4 raw event logs included  
- Comparative analysis between two AX200-equipped systems (Dell vs EQR6)  
- Failure occurs during Bluetooth pairing with PAN and tethering active  
- With PAN disabled, EQR6 still fails to maintain HFP connection  
- Dell maintains a stable *Calls only* connection  
- Intel is implicitly included in the diagnostic loop due to chip-level relevance

## 📄 Full Report

👉 [AX200-HFP-TechnicalReport.md](AX200-HFP-TechnicalReport.md)

## 📂 Included Files

- `dumps/` – 5 crash dumps  
- `eventviewer/` – 4 raw event logs (not directly analyzed)  
- `screenshots/` – visual evidence of BSOD traces and pairing failure

## 📣 Escalation Status

- Feedback Hub report submitted on **October 1, 2025**  
- Category: *Devices and Drivers > Bluetooth > Other Problems*  
- Status: Awaiting engineering review by Microsoft

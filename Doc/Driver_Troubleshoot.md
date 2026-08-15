# Troubleshooting Log — Happy

## Day 2 — ST-LINK not detected on ARM64 Windows (Secure Boot / driver signature issue)

**Symptom:**
STM32CubeIDE Debug session failed with "No ST-LINK detected." Device Manager showed the ST-LINK
under an unknown/warning state — **Code 28: "The drivers for this device are not installed."**
Every attempt to install ST's official driver (STSW-LINK009) failed, either silently or with:

```
Failed to add driver package: The third-party INF does not contain digital signature information.
```

This happened regardless of:
- Reinstalling STM32CubeIDE / CubeMX
- Redownloading the driver installer fresh
- Running installers as Administrator
- Using Zadig with Secure Boot still enabled
- Manually pointing Device Manager at the `.inf` file ("Have Disk")
- Enabling Windows test signing mode (`bcdedit /set testsigning on`) — this itself failed
  at first with `The value is protected by Secure Boot policy and cannot be modified or deleted.`

**Root cause:**
This laptop (Lenovo IdeaPad Slim 3) uses an **ARM64 (Snapdragon) processor**, not a
standard x86_64 (Intel/AMD) one. STMicroelectronics' official ST-LINK USB driver is only built
and signed for x86_64 Windows — **no official ARM64 build exists**. This is a known, documented
limitation (see ST Community forum threads on "STLink / STCubeProgrammer support on Windows
ARM64"). Every signature-related error was a symptom of this, not a Secure Boot misconfiguration.

**Fix — WinUSB + OpenOCD (works on ARM64):**

1. **Disable Secure Boot** in BIOS (required first — Windows blocks `bcdedit testsigning` and some
   driver operations while Secure Boot is on). On this laptop: restart → tap `F2` repeatedly →
   Security tab → Secure Boot → Disabled → Save & Exit.
   *(Saved BitLocker recovery key beforehand as a precaution — account.microsoft.com/devices/recoverykey)*

2. **Bind WinUSB (a generic, built-in Windows driver) to the ST-LINK USB interface using Zadig:**
   - Download Zadig: https://zadig.akeo.ie/
   - Options → List All Devices
   - Select **"ST-Link Debug (Interface 0)"** (USB ID `0483:374B`)
   - Driver dropdown → **WinUSB**
   - Install Driver

   This succeeds only once Secure Boot is disabled — with Secure Boot on, Zadig fails with
   "Operation not supported or not implemented."

3. **Switch CubeIDE's debug probe from "ST-LINK (ST-LINK GDB server)" to "ST-LINK (OpenOCD)":**
   - Project → Debug As → Debug Configurations
   - Select your project's debug config → **Debugger** tab
   - **Debug probe** dropdown → change to **ST-LINK (OpenOCD)**
   - Apply → Debug

   ST's own GDB server requires the official (missing-on-ARM64) driver, so it can't use a
   WinUSB-bound device. OpenOCD is an independent, open-source GDB server that supports
   ST-LINK over a plain WinUSB connection, so it works where ST's own tooling can't.

**Result:** Debug session connects, flashes, and halts correctly:
```
Info : STLINK V2J48M35 (API v2) VID:PID 0483:374B
Info : [STM32F407VGTx.cpu] Cortex-M4 r0p1 processor detected
Info : device id = 0x101f6413
Info : flash size = 1024 KiB
```

**Note for future setup on this machine (or any ARM64 Windows machine):** this only needs to be
done once. After Zadig + the OpenOCD debug probe setting are in place, every subsequent Debug
session in CubeIDE routes through OpenOCD automatically — no repeat setup needed.

---


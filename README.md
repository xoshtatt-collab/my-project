# OPPO A38 - ColorOS 15 HeyTap Shelf Troubleshooting & Optimization Guide

This repository contains diagnostic steps, configuration fixes, and workarounds for resolving widget rendering issues in the **HeyTap Shelf** (OPPO Shelf / Quick Glance) on the **OPPO A38** running **ColorOS 15 (Android 15)**.

---

## ⚠️ Important Note About HeyTap Shelf

**HeyTap Shelf** (package name `com.coloros.assistantscreen`, also listed as **Shelf**, **Smart Assistant**, or **Breeno Feed**) is a proprietary, closed-source system application built by OPPO / OPLUS. Its core source code is embedded within the ColorOS firmware and cannot be directly modified or patched from this repository.

However, widget failure on ColorOS 15 is typically caused by:
1. **A-series ColorOS 15 feature stripping**: OPPO frequently disables or limits native Shelf widget hosting on entry-level A-series builds (such as the OPPO A38) to reduce RAM usage.
2. **Sideloaded / mismatched APK builds**: Installing a non-matching APK (such as a Chinese Breeno Feed or OnePlus build on a Global ROM) often leaves widgets blank or without `BIND_APPWIDGET` permission.
3. **Migration cache corruption** following the ColorOS 15 upgrade.
4. **System Launcher / Shelf process mismatch** or **missing background/widget permissions**.

---

## 🛠️ Step-by-Step Fixes to Restore Widgets

### 1. Clear Cache & Data for Shelf & System Launcher
After major Android OS upgrades (such as ColorOS 14 to ColorOS 15), cached layout templates often fail to render widgets:

1. Open **Settings** > **Apps** > **App Management**.
2. Tap the **three-dot menu (⋮)** in the top-right corner and select **Show system**.
3. Locate **Shelf** (or **Quick Glance / Breeno Feed**):
   - Tap **Force Stop**.
   - Tap **Storage Usage** > **Clear Cache**, then **Clear Data**.
4. Go back to App Management and locate **System Launcher** (or **Home Screen**):
   - Tap **Storage Usage** > **Clear Cache** *(Note: Do NOT clear data on System Launcher unless necessary, as it resets your home screen icon placement)*.
5. Restart your device.

---

### 2. Configure Background & RAM Permissions (Helio G85 Optimization)
The OPPO A38 is powered by the MediaTek Helio G85 chipset. ColorOS 15 includes aggressive background task killing to conserve RAM, which frequently terminates widget background update services:

1. Open **Settings** > **Apps** > **App Management** > **Shelf**.
2. Tap **Permissions** and ensure:
   - **Location**: Set to *Allow while using app* or *Always allow* (required for weather/nearby widgets).
   - **Notifications**: *Allowed*.
3. Go back to Shelf App Info:
   - Tap **Battery Usage**: Set to **Unrestricted** / **Allow background activity** and **Allow auto-launch**.
   - Tap **Display over other apps**: Toggle **Allowed**.

---

### 3. Re-initialize the Shelf Gesture
Sometimes re-toggling the home screen pull-down gesture re-registers the Shelf widget host provider:

1. Go to **Settings** > **Home Screen & Lock Screen**.
2. Tap **Swipe down on Home screen**.
3. Switch it to **Global Search** or **Notification Drawer**, then switch it back to **Shelf**.
4. Swipe down from the top of your home screen to open Shelf.
5. Tap the **Gear (Settings)** icon in the top-right corner inside Shelf.
6. Check whether widgets are toggled on, and try adding a default system widget (e.g., Clock or Weather) using the **"+"** button.

---

### 4. Roll Back or Update HeyTap Shelf App
If you manually updated HeyTap Shelf via APK or if the ColorOS 15 package has a regional bug:

- **Uninstall Updates**:
  1. Go to **Settings** > **Apps** > **App Management** > **Show system** > **Shelf**.
  2. Tap the **three-dot menu (⋮)** in the top right.
  3. If **Uninstall updates** is available, select it to revert to the factory ColorOS 15 release version.
- **Check for Official Updates**:
  1. Open the OPPO **App Market**.
  2. Go to **General / Manage** > **Updates**.
  3. Update **HeyTap Shelf**, **System Launcher**, and **HeyTap Health** if updates are listed.

---

### 5. Check System Software Version
OPPO frequently pushes hotfix OTA updates addressing widget and launcher bugs after major ColorOS version rollouts:

1. Go to **Settings** > **About Device**.
2. Tap the **ColorOS** banner at the top to check for incremental patches.
3. If an update is available, download and install it.

---

### 6. Advanced Fixes (If All Steps Above Still Don't Work)

If widgets still do not appear after clearing cache/data, toggling permissions, and rebooting:

#### A. Force-Stop System Launcher + Verify Shelf Is Enabled
1. Go to **Settings** > **Apps** > **App Management** > **(⋮) Show system**.
2. Search for **Shelf** / **Smart Assistant** (`com.coloros.assistantscreen`) and confirm it is **Enabled** (not Disabled).
3. Search for **System Launcher** and tap **Force Stop** (not just Clear Cache), so the launcher reloads the Shelf widget host connection.

#### B. Check for Sideloaded / Mismatched Shelf APK
If you manually installed a **HeyTap Shelf** APK (for example, from APKMirror):
- A Chinese/Breeno build or ColorOS 14/16 build installed on a Global ColorOS 15 ROM will often open with empty widgets.
- Uninstall the manually installed updates/APK from **App Management** > **Shelf** > **(⋮) Uninstall updates** (or uninstall it completely if it is a user-installed app), then test a **Global ColorOS 15** build of `com.coloros.assistantscreen`—or rely on the stock launcher's widget drawer.

#### C. Grant Widget Binding & Assistant Screen via ADB (PC Required)
If Shelf was sideloaded or stripped of widget-host privileges on ColorOS 15:
1. Enable **Developer Options** and **USB Debugging** on your OPPO A38.
2. Connect to a PC with ADB and check the installed package:
   ```bash
   adb shell pm list packages | grep -E "assistantscreen|shelf|widget|card"
   adb shell dumpsys package com.coloros.assistantscreen | grep -E "versionName|enabled="
   ```
3. Grant Android `BIND_APPWIDGET` permission to Shelf:
   ```bash
   adb shell appwidget grantbind --package com.coloros.assistantscreen --user 0
   ```
4. Map the assistant/shelf screen setting (if hidden):
   ```bash
   adb shell settings put secure assistant_screen_type 1
   ```
5. Force-stop System Launcher so changes take effect:
   ```bash
   adb shell am force-stop com.android.launcher
   ```

#### D. OPPO A38 Hardware/Firmware Workaround
If **Shelf** does not appear under **Settings** > **Home Screen & Lock Screen** > **Swipe down on Home screen**, or remains completely empty even with `grantbind`, your regional ColorOS 15 firmware for the OPPO A38 has likely stripped native Shelf widget hosting. In that case:
- **Use Home Screen Widgets directly**: Pinch your Home Screen > **Widgets** to place ColorOS/Android widgets directly on your home pages.
- **Use a third-party launcher** (such as **Lawnchair** or **Nova Launcher**) if you want full widget drawer/feed customization.

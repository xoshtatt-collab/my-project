# OPPO A38 - ColorOS 15 HeyTap Shelf Troubleshooting & Optimization Guide

This repository contains diagnostic steps, configuration fixes, and workarounds for resolving widget rendering issues in the **HeyTap Shelf** (OPPO Shelf / Quick Glance) on the **OPPO A38** running **ColorOS 15 (Android 15)**.

---

## ⚠️ Important Note About HeyTap Shelf

**HeyTap Shelf** (`com.heytap.habit.analysis` / `com.coloros.widget.smallworld`) is a proprietary, closed-source system application built by OPPO / OPLUS. Its core source code is embedded within the ColorOS firmware and cannot be directly modified or patched from external repositories. 

However, widget failure on ColorOS 15 is typically caused by:
1. **Migration cache corruption** following the ColorOS 15 upgrade.
2. **Aggressive RAM management** on the Helio G85 platform killing background widget renderers.
3. **Missing system permissions** (Display over apps, Autostart, Battery Unrestricted).
4. **Mismatched HeyTap Shelf APK versions**.

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

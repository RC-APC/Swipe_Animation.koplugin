[简体中文](./README.md) | **English**

# Swipe_Animation

A software page-turn animation patch for KOReader that provides a smooth "wipe / erase and reveal" effect.

This patch brings fluid page turn animations to devices that lack native hardware support (or as an enhanced experience on supported devices).

## Features

* Smooth and faster page-turn animations
* Reduces screen flickering during page turns
* Customizable refresh interval
* Supports page-turn gestures in all directions
* Improved experience in Night Mode
* **New:** MTK device support (Kobo, Kindle 2022 and newer)
* **New:** Page-turn animation support for fixed-layout formats such as PDF, DjVu, and CBZ
* **New:** Adjustable animation delay (in milliseconds) for both portrait and landscape orientations through  
  **Settings (⚙) → Gesture Manager → Swipe Animation Settings**, eliminating the need to edit Lua files manually
* **New:** Customizable refresh mode with two options: **UI**, **Fast**
* **New:** Mild Global Refresh option for an improved text-only reading experience.
* **New:** Support for non-touch devices (e.g. Kindle 3 / Kindle Keyboard) — physical page-turn buttons trigger the animation the same way as touch swipes.

## Upgrade Notes

Starting from **V4.0**, this plugin no longer depends on or modifies `ffi/framebuffer.lua`.

**Users upgrading directly from versions prior to V4.0** should follow these steps:

1. Use the files in the `restore-files` folder of this repository to restore the system files (simply overwrite them).
2. Manually delete the following old patch files under `koreader/patches/`:
   - `1-mtk-swipe-direction.lua`
   - `2-mtk-swipe-direction.lua`
   - `2-swipe-full-refresh-judgment.lua`

## Installation

> **Important:** Back up your `koreader` directory before installing.

### Kindle / Kobo (Linux Version)

1. Connect your device to your computer via USB.
2. **Back up** your existing `koreader` folder.
3. Copy the `frontend` and `patches` folders from the extracted package into your device's `koreader` directory, and **merge/overwrite** the existing folders.  
   **Do not delete the original folders.**
   * Typical path: `D:\.adds\koreader\`
   * **Note:** If your device already supports native hardware page-turn animations and you only want to enable native animations for PDF files, simply copy `2-pdf-animation.lua` from the `patches` folder into the `koreader/patches/` directory instead of installing the full patch.
4. Safely eject the device and restart KOReader.
5. Enable the animation:
   * Open any book.
   * Go to **Settings (⚙) → Taps and gestures → Page turns**.
   * Enable **Page turn animations**.
   * **Non-touch devices (e.g. Kindle 3):** the path is **Settings (⚙) → Navigation → Page turn animations** (see "Non-Touch Devices" below).
6. *(Optional)* Adjust the animation delay:
   * Open **Settings (⚙) → Taps and gestures → Swipe Animation Settings**.
   * Configure separate animation delays (ms) for portrait and landscape mode. Long-press the option to view its description.
7. *(Optional)* Adjust global refresh mode:
   * Open **Settings (⚙) → Taps and gestures → Swipe Animation Settings**.
   * Enable or disable **Mild global refresh**. Long-press the option to view its description.

### Uninstallation

1. Use the `frontend` files from the `restore-files` folder in this repository to overwrite the corresponding system files on your device.
2. Delete the following files from the `koreader/patches/` directory on your device:
   - `2-pdf-animation.lua`
   - `2-swipe-animation-core.lua`
   - `2-swipe-animation-settings.lua`

### Non-Touch Devices (Kindle 3 / Kindle Keyboard)

This plugin fully supports Kindle 3 (Kindle Keyboard) and other **non-touch, physical-button-only** devices. The animation is triggered by software events, not by touch input — physical page-turn buttons emit the same `PageChangeAnimation` event as touch swipes, producing identical animation effects.

**How it works:** When you press a page-turn button (side buttons, D-pad, etc.), KOReader emits a `PageChangeAnimation` event. This plugin intercepts it and runs the software wipe animation. No touchscreen is required.

**Non-touch device defaults:** Older devices like the Kindle 3 use more conservative animation parameters (higher delays, fewer steps) to accommodate slower e-ink controllers:
- Portrait frame delay: 30ms (vs. 20ms on touch devices)
- Landscape frame delay: 15ms (vs. 10ms on touch devices)
- Portrait animation steps: 6 (vs. 8 on touch devices)
- Landscape animation steps: 4 (vs. 6 on touch devices)

These can be customized under **Settings → Taps and gestures → Swipe Animation Settings** (or **Settings → Navigation → Swipe Animation Settings** on non-touch devices).

**Enabling the animation (Kindle 3):**
1. Open any book
2. Press the D-pad (or Menu button) to bring up the top menu
3. Navigate to **Settings (⚙) → Navigation**
4. Enable **Page turn animations** (a checkbox injected by this plugin, located right above "Swipe animation settings")

> **Note:** The upstream "Page turn animations" toggle lives in the *Taps and gestures → Page turns* submenu, which KOReader only builds on touch devices. This plugin therefore injects an equivalent toggle into the **Navigation** menu on non-touch devices. The fine-tuning entries inside "Swipe animation settings" become selectable once the animation is enabled.

## Version Notes

The bundled `frontend/ui/uimanager.lua` is a snapshot of:

- KOReader release: (2026-07-01)

## Supported Devices

* **Fully tested:** Kobo devices, Kindle devices (including KV, KO, and KPW series), and most Linux-based e-ink devices running KOReader.
* **Non-touch devices:** Kindle 3 (Kindle Keyboard) and other physical-button-only devices — the animation is triggered by page-turn buttons, producing the same effect as on touch devices.
* **Android:** Android devices are **currently not supported**, as the animation performance is not satisfactory on the Android platform.

## Menu Structure

```
Settings (⚙)
├── Touch devices: Taps and gestures
│   ├── Page turns
│   │   └── ☑ Page turn animations (upstream toggle)
│   └── Swipe animation settings
│       ├── Swipe animation refresh mode
│       │   ├── ○ UI refresh
│       │   └── ○ Fast refresh
│       ├── Portrait animation frame delay: ms
│       ├── Landscape animation frame delay: ms
│       └── ☑ Mild global refresh
│
├── Non-touch devices (e.g. Kindle 3): Navigation
│   ├── ☑ Page turn animations (toggle injected by this plugin)
│   └── Swipe animation settings (same sub-items as above)
│
└── Screen
    └── E-ink settings
        └── Full refresh rate
```

## FAQ

### Q: KOReader crashes after installing the patch.

Restore the original files from your backup.

Common causes include:

1. Your KOReader version is outdated. Update KOReader to the latest version and reinstall the patch.
2. You are using macOS to copy the files. Restore the original files first, delete the corresponding original files manually, and then copy the patched files again.
3. The installation was not performed correctly. Restore your backup and repeat the installation steps carefully.

### Q: The "Page turn animations" option doesn't appear / The "Page turn animations" option appears, but nothing happens.

Update KOReader to the latest version and reinstall the patch.

### Q: "Swipe animation settings" is grayed out on a non-touch device (Kindle 3).

This means the master "Page turn animations" switch has not been enabled yet. Enable **Page turn animations** under **Settings → Navigation** (the checkbox right above "Swipe animation settings"); the fine-tuning entries will then become selectable. Older versions of this plugin never injected that toggle on non-touch devices, leaving the animation impossible to enable — if you run into this, update to the latest version of the patch.

### Q: The screen flashes black and white on every page turn.

Adjust the **Full Refresh Rate** under:

**Settings → Screen → E-Ink Settings → Full Refresh Rate**

## Credits

* Original author: `xhs:5699990012`
* Improved version: **nuku**
* v3.x optimization and MTK support:
  * **Echoes**
  * **小红薯6809667F**
  * **斯普特尼克的漫游**

## License

This project is licensed under **GPLv3**, following the same license as KOReader.


# droid-utils

A command-line tool to manage Android device display/control using `adb`, `scrcpy`, and `uhubctl`.

## Features
- `droidctl-open`: Launch `scrcpy` sessions for connected devices not already active
- `droidctl-align`: Arrange all scrcpy windows on screen
- `droidctl-swipe`: Simulate swipe-down on scrcpy windows via `pyautogui` (infinite loop by default, use `-c` to specify count)
- `droidctl-recover`: Auto-recover `unauthorized` devices by toggling USB ports with `uhubctl`

## Quick Start

```bash
# Launch scrcpy for connected devices
droidctl-open

# Arrange scrcpy windows
droidctl-align

# Infinite swipe loop (press Ctrl+C to stop)
droidctl-swipe

# Swipe loop with specific count
droidctl-swipe -c 5

# Recover unauthorized devices
droidctl-recover
```

## Commands

### droidctl-open
Launch scrcpy sessions for all connected Android devices.

**Options:**
- `-s, --small-screen`: Launch with small screen size (120x264 pixels)
- `-m, --mid-screen`: Launch with medium screen size (200x440 pixels) [default]
- `-l, --large-screen`: Launch with large screen size (280x616 pixels)
- `-h, --help`: Show detailed help message

**Examples:**
```bash
droidctl-open              # Launch with default medium size
droidctl-open -s           # Launch with small screen
droidctl-open -l           # Launch with large screen
```

**Notes:**
- Devices already running scrcpy will be skipped
- Window title format: `{model}_{serial_last_4_digits}`
- Audio is disabled by default for performance
- Video bitrate: 128K, Max FPS: 15

---

### droidctl-align
Arrange all active scrcpy windows in a grid layout.

**Options:**
- `-h, --help`: Show detailed help message

**Examples:**
```bash
droidctl-align             # Arrange all scrcpy windows
```

**Details:**
- Windows are sorted alphabetically by title
- Arranged in a 2-column grid layout
- Default window size: 200x440 pixels
- Vertical spacing: 37 pixels (for title bar)

---

### droidctl-swipe
Simulate swipe-down gestures on all scrcpy windows.

**Options:**
- `-c N, --count N`: Number of loop iterations (omit for infinite loop)
- `-h, --help`: Show detailed help message

**Examples:**
```bash
droidctl-swipe             # Run infinite loop (Ctrl+C to stop)
droidctl-swipe -c 5        # Run exactly 5 loops then exit
droidctl-swipe --count 10  # Run exactly 10 loops then exit
```

**Behavior:**
- Detects all scrcpy windows automatically
- Swipes from center to top of each window
- Waits 10-12 seconds (random) between loop sets
- Shows progress bar during wait time

**Safety:**
- Use Ctrl+C to safely interrupt infinite loops
- PyAutoGUI FAILSAFE is disabled

---

### droidctl-recover
Auto-recover unauthorized Android devices by toggling USB ports.

**Options:**
- `-h, --help`: Show detailed help message

**Examples:**
```bash
droidctl-recover           # Recover all unauthorized devices
```

**Process:**
1. Detect unauthorized devices via `adb devices`
2. Map devices to USB hub ports using uhubctl
3. Power off USB port (2 seconds)
4. Power on USB port (5 seconds wait)
5. Device should prompt for authorization again

**Requirements:**
- uhubctl must be installed at `/sbin/uhubctl`
- sudo access required (may prompt for password)
- USB hubs must support per-port power switching

**Troubleshooting:**
- If devices not found: check USB hub compatibility
- After recovery: manually approve authorization on device
- Some USB hubs do not support power switching

---

## Setup

```bash
git clone https://github.com/tm16470/ardctl.git
cd ardctl
pip install .
```

> Requires: `adb`, `scrcpy`, and `uhubctl` installed & configured

## Help

Each command provides detailed help with the `-h` or `--help` flag:

```bash
droidctl-open --help      # Show available screen size options
droidctl-align --help     # Show window alignment information
droidctl-swipe --help     # Show swipe loop options and examples
droidctl-recover --help   # Show recovery requirements and process
```

# Android Debug Bridge (ADB) Cheat Sheet

## Setup & Connection
```bash
adb devices                     # List connected devices/emulators
adb devices -l                  # List with details (model, product, etc.)
adb connect <ip>:5555           # Connect over Wi-Fi/TCP
adb disconnect <ip>:5555        # Disconnect from a device
adb tcpip 5555                  # Restart adbd listening on TCP port
adb kill-server                 # Kill the adb server
adb start-server                # Start the adb server
adb -s <device_id> <command>    # Target a specific device (when multiple attached)
```

## App Installation & Management
```bash
adb install app.apk             # Install an APK
adb install -r app.apk          # Reinstall, keep data
adb install -g app.apk          # Grant all runtime permissions on install
adb uninstall com.package.name  # Uninstall an app
adb shell pm list packages      # List installed packages
adb shell pm list packages -3   # List only third-party (user-installed) packages
adb shell pm clear com.package.name   # Clear app data/cache
adb shell pm path com.package.name    # Show path to installed APK
```

## File Transfer
```bash
adb push local_file /sdcard/    # Copy file from computer to device
adb pull /sdcard/file local/    # Copy file from device to computer
adb pull /sdcard/DCIM ./photos  # Pull a whole directory
```

## Shell Access
```bash
adb shell                       # Open interactive device shell
adb shell <command>             # Run a single shell command
adb shell ls /sdcard/           # Example: list files
adb shell reboot                # Reboot device
adb shell reboot recovery       # Reboot into recovery mode
adb shell reboot bootloader     # Reboot into bootloader/fastboot
```

## Logging & Debugging
```bash
adb logcat                      # Stream device logs
adb logcat -c                   # Clear log buffer
adb logcat *:E                  # Show only error-level logs
adb logcat -s TagName           # Filter by log tag
adb bugreport > report.zip      # Generate a full bug report
```

## App Control
```bash
adb shell am start -n com.package/.MainActivity   # Launch an activity
adb shell am force-stop com.package.name          # Force-stop an app
adb shell am start -a android.intent.action.VIEW -d "https://example.com"  # Open URL
adb shell monkey -p com.package.name -v 1         # Send a random tap event (sanity test)
```

## Screen & Input
```bash
adb shell screencap /sdcard/screen.png       # Take a screenshot (on device)
adb pull /sdcard/screen.png                  # Then pull it to computer
adb shell screenrecord /sdcard/demo.mp4      # Record screen (Ctrl+C to stop)
adb shell input text "hello"                 # Simulate typing text
adb shell input keyevent 4                   # Simulate Back button
adb shell input keyevent 3                   # Simulate Home button
adb shell input tap <x> <y>                  # Simulate a tap
adb shell input swipe <x1> <y1> <x2> <y2>    # Simulate a swipe
```

## Common Key Event Codes
| Code | Button      |
|------|-------------|
| 3    | Home        |
| 4    | Back        |
| 24   | Volume Up   |
| 25   | Volume Down |
| 26   | Power       |
| 82   | Menu        |

## Device Info
```bash
adb shell getprop ro.product.model     # Device model
adb shell getprop ro.build.version.release  # Android version
adb shell dumpsys battery              # Battery info
adb shell wm size                      # Screen resolution
adb shell wm density                   # Screen density
```

## Port Forwarding
```bash
adb forward tcp:8080 tcp:8080    # Forward host port to device port
adb reverse tcp:3000 tcp:3000    # Reverse: device port to host port
```

## Backup & Restore
```bash
adb backup -apk -all -f backup.ab   # Backup all apps and data
adb restore backup.ab               # Restore from backup
```

## Tips
- Use `adb shell` then run multiple commands interactively instead of prefixing each with `adb shell`.
- Enable **USB Debugging** in Developer Options before connecting via USB.
- Use `adb logcat | grep <keyword>` to filter logs quickly on Linux/macOS.
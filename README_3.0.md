# OHDrive 3.0.0

This package is the OHDrive 2.9.27 source with only the Android version metadata updated:
- versionCode: 54 -> 55
- versionName: 2.9.27 -> 3.0.0

No application logic, permissions, host validation, package ID, signing setup, or features have been changed.

Build the familiar debug APK:

```powershell
Set-Location "C:\OHDrive_3.0"
.\gradlew.bat assembleDebug
```

Install on a connected Android phone:

```powershell
$adb = "$env:LOCALAPPDATA\Android\Sdk\platform-tools\adb.exe"
& $adb install -r ".\app\build\outputs\apk\debug\app-debug.apk"
```

Use the same debug signing key to update an existing installation. If signatures differ, Android will reject an in-place update. Do not uninstall before backing up app settings.

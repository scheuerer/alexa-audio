# OH Drive

**OH Drive 2.8 - openHAB for Android Auto**

OH Drive is an independent Android application that provides access to selected openHAB items through Android Auto.

## Features

- Android Auto interface using the AndroidX Car App Library
- Integration with an existing openHAB server
- Sitemap-based item display and controls
- Live relay status updates
- Garage door, garden gate and entrance door controls
- Roller shutter controls with STOP safety handling
- Temperature and other openHAB status information
- German and English user interface
- Automatic language selection based on the Android system language
- Support for configured item labels and icons

## Requirements

- Android 10 or later
- Android Auto-compatible smartphone and vehicle
- An accessible openHAB server
- A configured openHAB sitemap
- Network connectivity, including VPN for remote access where appropriate

## Installation

1. Download the APK from GitHub Releases when available.
2. Install OH Drive on your Android smartphone.
3. Open the settings and configure the openHAB server connection.
4. Configure the sitemap and supported items in openHAB.
5. Connect the smartphone to an Android Auto-compatible vehicle.
6. Check whether OH Drive appears in the Android Auto launcher.

Android Auto availability depends on platform restrictions and app eligibility.
AAEnabler is a separate third-party tool and is not included in OH Drive.

## Languages

OH Drive 2.8 supports:

- Automatic (Android system language)
- Deutsch
- English

The selected language applies to supported smartphone and Android Auto interface messages.

## openHAB Integration

OH Drive connects to the configured openHAB server and reads sitemap information.
Available controls depend on the openHAB configuration.

For remote access, a secure VPN connection is recommended.
Do not expose an unsecured openHAB server directly to the internet.

## Safety

OH Drive can control physical devices including gates, garage doors and roller shutters.
Operate these devices only when it is safe and legal to do so.
Network interruptions may prevent reliable command confirmation.
OH Drive does not replace physical safety mechanisms or interlocks.

## Version 2.8

Version 2.8 introduces German and English language selection and localized Android Auto messages.

See [CHANGELOG.md](CHANGELOG.md) for release details.

German and English were tested in smartphone settings and the Android Auto emulator.
Testing in a physical vehicle is still pending.

## Build from Source

Requirements:

- Kotlin
- Android SDK 35
- Java 17
- AndroidX Car App Library 1.4.0

Build the debug APK using:

    .\gradlew.bat :app:assembleDebug

The APK will be generated in app/build/outputs/apk/debug/.

## Disclaimer

OH Drive is an independent community project.
It is not affiliated with or endorsed by the openHAB Foundation, Google, Android Auto or any vehicle manufacturer.
Use at your own risk.

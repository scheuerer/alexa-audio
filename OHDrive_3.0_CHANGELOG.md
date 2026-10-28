# Changelog

## OHDrive 3.0.0 — October 28, 2026

OHDrive 3.0 marks a milestone for controlling and monitoring openHAB from Android Auto. It brings together the familiar in-car dashboard, configurable sitemap-based controls, a mobile sitemap editor, and remote access through myopenHAB Cloud.

### myopenHAB Cloud connectivity

OHDrive can now connect to an openHAB system through myopenHAB Cloud, allowing users to access their home automation setup when they are away from their local Wi-Fi network. Instead of relying solely on a local network address, users can configure a cloud connection to reach their openHAB installation over the internet.

This is especially useful on the road: the Android Auto interface can display openHAB item states and send supported control commands even when the phone is connected through mobile data. The connection was tested against an online myopenHAB-enabled test system, and a socket was successfully switched using the phone's head-unit emulator. Item values were also reported to display correctly.

Cloud access requires a working openHAB Cloud Connector configuration, an online system registered with myopenHAB, and valid account credentials. Availability and response times depend on the internet connection and cloud service.

### Sitemap editor

The integrated sitemap editor makes it easier to tailor the Android Auto interface without manually rebuilding the app. Users can work with sitemap entries, configure their labels and icons, and adapt the controls shown in OHDrive to their own openHAB items.

The editor is intended to support common smart-home use cases such as switches, impulse controls, status displays, and percentage-based controls. Sitemap export allows an edited configuration to be saved locally on the phone for subsequent transfer to the openHAB system.

Some editor improvements remain under development, including reliable REST saving, item search and suggestions, consistent alignment of newly added entries, and easier discovery of sitemap files stored in the openHAB sitemaps directory. These are not claimed as completed fixes in 3.0.

### Android Auto experience

- Display openHAB item states in a car-friendly tile layout.
- Use configurable icons and labels supplied by the sitemap.
- Operate supported devices such as switches and impulse-controlled relays.
- Monitor home information, including temperatures and weather-related values where configured.
- Use the existing tested Android Auto workflow with the phone's head-unit emulator.

### Release notes

Version 3.0.0 is based on the successfully tested OHDrive 2.9.27 codebase. The final version-number update does not introduce additional functional code changes. The separately tested 3.0 release candidate used a different package identifier and is not the basis of this build.

The APK built using `assembleDebug` is a debug-signed build intended for testing and sideloading. It is not a production-signed Google Play release. Android Auto availability on individual phones and head units may vary.

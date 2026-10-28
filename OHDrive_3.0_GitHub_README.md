# OHDrive 3.0

**Your openHAB smart home on Android Auto.**

OHDrive brings selected openHAB controls and live status information to the Android Auto display. Its streamlined, configurable tile interface is designed for quick access to useful smart-home functions, such as gates, garage doors, lights, sockets and environmental readings.

> **Project status:** Version 3.0 is available as a tested development APK. Android Auto compatibility depends on the phone, Android Auto host and installation method. This project is not affiliated with Google or the openHAB Foundation.

## Highlights

### Smart-home controls in Android Auto

- Configurable tiles for openHAB items and their current states.
- Switching for supported devices such as lights and sockets.
- Impulse-style controls for gates, doors and other momentary actions.
- Status tiles for temperatures, rain and other sensor values.
- Roller-shutter controls where configured and supported.
- Live state updates and connection feedback to help distinguish current information from unavailable controls.

### Sitemap-based customization

OHDrive uses an openHAB sitemap to define the items presented in the car. Tile labels, item assignments, icons and control behavior can be tailored to the user's smart home without hard-coding every device into the Android Auto interface.

### Integrated sitemap editor

The companion phone interface includes a sitemap editor for managing the OHDrive layout. It supports editing entries, changing labels, selecting item references, choosing icons and exporting sitemap content for further editing or transfer. Icon selection includes openHAB Classic and F7-style options.

The editor is still evolving: item discovery, direct saving to openHAB and certain layout and percentage-control details have known limitations. Exporting a sitemap is not the same as automatically deploying it to the openHAB server.

### myopenHAB Cloud access

OHDrive supports connecting to an openHAB system through **myopenHAB Cloud**, enabling access when the phone is away from the home Wi-Fi network. This makes it possible to view configured values and operate supported devices over a mobile-data connection, provided the openHAB Cloud connection and user account are functioning.

The cloud path was tested with a remote openHAB test system. A socket command and the expected status values were successfully exercised through the Android Auto headunit emulator. Availability and responsiveness still depend on the mobile connection, the cloud service and the underlying openHAB system.

## Version 3.0

OHDrive 3.0 builds on the working 2.9.27 feature set. The 3.0 version bump was intentionally kept minimal, without introducing unrelated changes to the application's behavior.

Key capabilities developed across the recent releases include the integrated sitemap editor, expanded icon customization, sitemap export, improved tile configuration and myopenHAB Cloud connectivity.

## Compatibility and known issue: Xiaomi + AA Enabler

**The Xiaomi / AA Enabler issue remains unresolved.**

A tester has reported that OHDrive may be installed on a Xiaomi phone yet fail to appear as an available Android Auto app, or remain unavailable for selection in the Android Auto launcher. Enabling unknown sources, checking Android Auto developer settings and restarting the phone have not yet produced a confirmed general solution.

OHDrive has been tested successfully on a Samsung Galaxy S23 Ultra, including launching in Android Auto, displaying configured tiles and switching a socket. **That result does not establish compatibility with Xiaomi devices.** The cause of the Xiaomi-specific discovery or launcher problem is still under investigation; AA Enabler is not a guaranteed workaround.

## Safety and limitations

OHDrive is intended for brief, appropriate interactions. Driving always takes priority; use controls only when it is safe and lawful to do so. Remote commands can affect physical equipment, so configure gates, doors and other actuators with suitable safeguards on the openHAB side.

OHDrive is an independent project. Android Auto app visibility and behavior may vary across devices and software versions. Some features are under active development and should not be assumed to work on every openHAB setup.

## Project information

- **Current version:** 3.0.0
- **Platform:** Android / Android Auto
- **Smart-home integration:** openHAB
- **Remote access:** myopenHAB Cloud
- **Documentation:** See `CHANGELOG.md` for release history.

---

*OHDrive — your smart home, one glance away.*

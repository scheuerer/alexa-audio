# OHDrive 3.0
### Your openHAB smart home on Android Auto

**OHDrive** brings selected openHAB controls and live information to the Android Auto screen. It is designed around a simple idea: quick access to the smart-home functions that matter when arriving home or leaving, without navigating a full home-automation dashboard.

OHDrive is an independent community project and is not an official openHAB or Google application.

## Highlights

- **Android Auto dashboard:** A configurable grid of controls and information tiles.
- **openHAB integration:** Read item states and send commands to supported items.
- **Gates, garage doors and switches:** Trigger configured impulse actions and toggle devices.
- **At-a-glance information:** Display temperatures, weather information, rain status and other openHAB values.
- **Sitemap-driven configuration:** Decide which controls appear and how they are labelled.
- **Built-in sitemap editor:** Manage entries directly on the phone instead of editing every entry manually.
- **Icon selection:** Choose from openHAB Classic and F7 icon families.
- **myopenHAB Cloud:** Access your openHAB instance away from home, including over mobile data.
- **English interface support:** Use OHDrive beyond German-language installations.

## How OHDrive works

OHDrive connects to an existing openHAB system. Rather than showing every item in a potentially large installation, it uses a dedicated sitemap to select the functions relevant to the car.

On the Android Auto display, configured entries appear as compact tiles. An information tile can show a current item value, while an action tile can send a command. For example, a garage-door entry may send a short impulse, whereas a smart plug can use a regular switch command. The exact result depends on the connected openHAB item, its rules and the selected control mode.

OHDrive does not replace openHAB. The automation logic, permissions and device integrations remain on the openHAB server.

## Android Auto dashboard

The driving interface focuses on clarity and limited interaction:

- Read current values such as outdoor temperature, rain or device status.
- Operate configured gates, doors, lights or sockets.
- Use different control behaviours for information-only entries, switches, impulse controls and supported roller-shutter configurations.
- Display configurable titles and icons instead of exposing raw openHAB item names.
- Refresh information and receive live state changes when the connection supports them.

An impulse action is intended for items that trigger a short operation, such as a garage-door relay. It should not be confused with a permanently enabled switch.

**Safety:** Configure only appropriate controls for in-car use. Keep attention on the road and follow local traffic rules. Availability and presentation depend on the Android Auto host.

## Built-in sitemap editor

OHDrive includes a phone-side editor for the dedicated Android Auto sitemap. This makes it possible to adjust the dashboard without repeatedly changing the application itself.

The editor is intended to help with:

- Adding and editing dashboard entries.
- Changing the visible label of an entry.
- Associating an entry with an openHAB item.
- Choosing the control type and an appropriate icon.
- Working with the openHAB Classic and F7 icon collections.
- Exporting a sitemap file for use with an openHAB installation.

The sitemap determines which items are shown. This means the dashboard can be adapted to different homes without changing the OHDrive application code.

**Current editor limitations:** Some item-search cases, alignment details, percentage-control direction options and direct saving to the server-side sitemap directory have been identified for further improvement. Exporting a sitemap is not the same as automatically writing it to `/etc/openhab/sitemaps`.

## myopenHAB Cloud — access beyond your home Wi-Fi

One of the major capabilities available in OHDrive 3.0 is connectivity through **myopenHAB Cloud**.

Previously, a typical local setup relied on the phone reaching the openHAB server over the home network. With a working myopenHAB connection, OHDrive can also reach the configured openHAB installation when the phone is away from home and using mobile data.

This is particularly useful for Android Auto: a garage-door tile can remain relevant as you approach home, before the phone has joined your home Wi-Fi. Information tiles can likewise continue to show openHAB values remotely.

### What is required?

- An operational openHAB installation.
- A functioning and authorised myopenHAB Cloud connection.
- The corresponding OHDrive connection settings.
- Internet access on the phone and a reachable cloud service.

The cloud connection does **not** make openHAB independent of the internet, and response times may vary with mobile coverage and server availability. It also does not bypass the permissions of the configured openHAB account.

### Tested scenario

In the project's testing, OHDrive successfully connected through myopenHAB, displayed the expected item values and operated a smart plug through the Android Auto head-unit emulator. This is a functional test of that setup, not a guarantee for every device or vehicle.

## Getting OHDrive onto Android Auto

OHDrive is currently distributed as an **APK**, rather than through a standard Google Play listing.

1. Download the OHDrive APK from the project's GitHub release.
2. Install the APK on the Android phone, permitting installation from the chosen download source when Android asks.
3. Open OHDrive on the phone and configure the connection to your openHAB installation and the dedicated sitemap.
4. For Android Auto, enable **Developer mode** in Android Auto and allow **Unknown sources**, where this option is available.
5. Use **AA Enabler** where needed to make the sideloaded app available to Android Auto. Availability and success depend on the phone, Android version and Android Auto release.
6. Check Android Auto's app launcher or launcher customisation settings for OHDrive, then connect to the car or use a head-unit emulator.
7. Start with a harmless test item, such as a smart plug, before configuring access to gates or doors.

**Note:** AA Enabler is a third-party tool. OHDrive cannot guarantee that it works on every device or Android Auto version. Do not download unofficial APKs from untrusted sources.

These are end-user installation steps only. This README intentionally does not include source-code build instructions, Gradle commands or development environment setup.

## Compatibility and test status

| Platform / scenario | Status |
| --- | --- |
| Samsung Galaxy S23 Ultra, Android 16, Android Auto | Successfully used during project testing |
| Android Auto head-unit emulator | OHDrive displayed values and successfully switched a smart plug |
| Local openHAB connection | Supported |
| myopenHAB Cloud connection | Successfully tested, including remote access |
| OHDrive 3.0 RC1 with restricted Android Auto host validation | Successfully tested on the project's Samsung test setup |
| Xiaomi devices with AA Enabler | **Unresolved compatibility issue** |
| Other Android phones, car head units and Android Auto versions | Not comprehensively verified |

### Xiaomi and AA Enabler: known unresolved issue

**There is still no confirmed solution for the Xiaomi / AA Enabler compatibility problem.** Depending on the Xiaomi device, Android software and Android Auto environment, a sideloaded OHDrive installation may fail to appear in the Android Auto launcher or may not be made available through AA Enabler.

The issue should not be presented as fixed. A successful installation on Samsung does not establish Xiaomi compatibility. More device-specific testing is needed before a reliable workaround can be documented.

### Android Auto host compatibility

A separate OHDrive 3.0 release-candidate test used restricted Android Auto host validation and succeeded on the Samsung test setup. That is useful evidence for compatibility in that environment, but it does not establish universal acceptance across all production Android Auto hosts.

The final OHDrive 3.0 release is based on the established 2.9.27 application with an updated version number. Therefore, the RC1-specific host-validation result must **not** be claimed as a verified feature of the final APK.

## Typical uses

**Arriving home:** Display the garage-door control on Android Auto and trigger the configured openHAB relay.

**Checking the weather:** Show an outdoor temperature and rain-status tile without opening a full openHAB dashboard.

**Controlling a device:** Toggle a configured socket or light and observe its reported state.

**Customising the dashboard:** Use the sitemap editor to adjust titles, item associations, control modes and icons to match the installation.

## Privacy and connectivity

OHDrive communicates with the openHAB endpoint configured by the user. When using myopenHAB Cloud, traffic passes through that cloud service rather than staying exclusively on the local network. Keep credentials private, use appropriate access permissions and avoid exposing sensitive controls unnecessarily.

## Known limitations

- Android Auto support depends on host policy, phone firmware and Android Auto version.
- Sideloading and AA Enabler are not guaranteed to work on every phone.
- Xiaomi / AA Enabler compatibility remains unresolved.
- Some sitemap-editor workflows still need refinement.
- Cloud access requires a working internet connection and cloud account.
- A successful test with one phone and emulator is not a universal compatibility certification.

## Release notes

**OHDrive 3.0** continues the established OHDrive 2.9.27 codebase under the 3.0.0 version name. The project's recent development and testing include the sitemap-editing workflow and myopenHAB connectivity. The separate 3.0 RC1 host-validation experiment is documented as a test, not silently merged into the final app.

See `CHANGELOG.md` for the release history.

---

**OHDrive — openHAB controls where you need them: on Android Auto.**

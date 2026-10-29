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

## Sitemap setup and item mapping (step by step)

OHDrive uses an **openHAB sitemap** to determine which tiles appear in Android Auto. This is different from an openHAB Main UI page and from an `.items` file: the `.items` file defines Items, while the `.sitemap` file selects and arranges existing Items for display.

### Prerequisites

1. A running openHAB server accessible from the phone (locally or through a configured myopenHAB Cloud connection).
2. Existing openHAB Items with working states and, for interactive tiles, appropriate command handling. Verify each Item in openHAB before adding it to OHDrive.
3. A dedicated sitemap, for example `ohdrive.sitemap`, or an editable REST-managed sitemap.
4. The exact sitemap name configured in OHDrive (for `ohdrive.sitemap`, enter `ohdrive`, without the extension).
5. A working Android Auto installation; if sideloading is required, consult the AA Enabler compatibility notes below.

### Example: `ohdrive.sitemap`

This example follows the standard sitemap structure and the export format used by the OHDrive editor. **Replace every example Item name with an Item that actually exists on your openHAB server.**

```text
sitemap ohdrive label="OHDrive" {
    Frame {
        Switch item=GardenGarageDoor label="Garden garage door [ohdrive:impuls:live]" icon="garage" mappings=[ON="Impulse"]
        Switch item=GardenGate label="Garden gate [ohdrive:impuls:live]" icon="door" mappings=[ON="Impulse"]
        Switch item=GarageDoor label="Garage door [ohdrive:impuls:live]" icon="garage" mappings=[ON="Impulse"]
        Switch item=OutdoorSocket label="Outdoor socket" icon="power"
        Text item=OutdoorTemperature label="Outdoor temperature" icon="temperature"
        Text item=RainStatus label="Rain status" icon="rain"
        Slider item=AwningPosition label="Awning [ohdrive:markise:0offen] [%s]" icon="rollershutter"
    }
}
```

**Important:** The names above are placeholders, not bundled OHDrive Items. An impulse sends the configured `ON` command; the actual pulse duration and relay behaviour must be implemented safely by the openHAB Item/device/rule. Do not assume OHDrive itself turns the relay off.

### Understanding the mapping

| Sitemap syntax | Purpose | OHDrive use |
| --- | --- | --- |
| `sitemap ohdrive` | Sitemap identifier | Select `ohdrive` in the app settings |
| `Frame { ... }` | Groups entries | Contains the dashboard entries |
| `Text item=...` | Read-only Item | Shows an Item's current state |
| `Switch item=...` | Commandable Item | Allows switching/toggling |
| `mappings=[ON="Impulse"]` | Explicit command mapping | Offers an `ON` impulse action |
| `[ohdrive:impuls:live]` | OHDrive label marker | Selects the live impulse behaviour |
| `Slider item=...` | Numeric/percentage control | Used for supported roller/percentage tiles |
| `[ohdrive:markise:0offen]` | OHDrive label marker | Percentage direction: 0% means open |
| `[ohdrive:markise:100offen]` | OHDrive label marker | Percentage direction: 100% means open |
| `icon="..."` | Icon identifier | Chooses the tile icon; Classic and `f7:` names are supported by the editor |
| `label="..."` | Visible name | Text shown for the tile (plus optional OHDrive markers) |

The editor currently exports `Text`, `Switch` and `Slider` widgets. Other openHAB sitemap constructs are not necessarily supported by OHDrive's editor/exporter. The precise device behaviour depends on the underlying Item and its binding/rules. Percentage direction options are present in the editor but should be tested with the actual device before use.

### How to create and install a file-based sitemap

1. Create a UTF-8 text file named `ohdrive.sitemap` containing the example above, adjusted to your Item names.
2. Copy it to `/etc/openhab/sitemaps/ohdrive.sitemap` on your openHAB server. On openHABian this is normally the file-based sitemap directory; use an account with permission to write there.
3. Open OHDrive on the phone, configure your openHAB connection, and set/select the sitemap name `ohdrive`.
4. Load the sitemap and verify that the displayed values match openHAB. Test a harmless switch before enabling door/gate actions.
5. If an Item does not appear, confirm its **exact Item name**, type, and state in openHAB and verify that OHDrive can reach the server.

**Do not confuse the two file types:** `ohdrive.sitemap` is a text sitemap file, not an `.items` file and not a JSON export.

### Creating and editing the sitemap on your phone

OHDrive's built-in sitemap editor can search for Items, add tiles, change their order and labels, choose Classic or F7 icons, and set the tile type (status, toggle, impulse or supported percentage/roller modes). The editor offers two distinct saving workflows:

**File export:** Choose **Export .sitemap to Downloads**. The app writes a file to the phone's Downloads folder. Transfer that file to `/etc/openhab/sitemaps/` yourself, and select its sitemap name in OHDrive. Exporting does **not** modify your openHAB server automatically.

**REST-managed sitemap:** The editor can save a **separate** REST-managed sitemap named `ohdrive_live` in openHAB. This does **not** overwrite `/etc/openhab/sitemaps/ohdrive.sitemap`. Select `ohdrive_live` in OHDrive afterwards if you want to use it. Direct REST saving has had issues in testing; verify the result on your server before relying on it.

### Troubleshooting sitemap mapping

- **No tiles:** Check the server URL/credentials, sitemap name, and whether the sitemap is available through the openHAB REST API.
- **Missing or blank value:** Check that the Item exists, has a usable state, and is linked to a working Thing/channel or rule.
- **Button does not operate a device:** Verify that the Item accepts the intended command (`ON`, `OFF`, or numeric value) and that its device-side handling is configured.
- **Unexpected percentage direction:** Confirm whether the device treats 0% or 100% as open and select the corresponding editor option; test with a safe position first.
- **Exported file does not change the dashboard:** Copy it to the server's sitemap directory and reload/select that sitemap; an export alone does not update openHAB.
- **REST changes do not appear:** Check whether you are viewing the file-based sitemap or the separate `ohdrive_live` REST sitemap.

## MAP Transformation for rain status (important setup step)

OHDrive can display an openHAB rain Item such as `Rain_Status_Verifiziert`. A `Switch` Item usually reports raw states such as `ON` and `OFF`. If you want readable labels such as **Rain** and **No rain**, openHAB can translate those states with a **MAP transformation**.

**Important distinction:** MAP Transformation is an openHAB **transformation add-on/service**, not a device binding and not an OHDrive-specific plugin. It must be installed on the **openHAB server**. The `.map` file also lives on the server, not on the Android phone. You do not need this add-on merely to display the raw `ON`/`OFF` state; you need it for the example translated labels below.

### Step 1 — Install the MAP Transformation add-on

1. Open your openHAB web interface.
2. Go to **Settings → Add-ons → Transformations** (the wording may vary by openHAB version).
3. Find and install **MAP Transformation**.
4. Verify that the add-on is installed before using a `MAP(...)` expression.

This is **not** the same as installing a Shelly, MQTT, or other device binding. Your existing binding supplies the Item state; MAP only changes its presentation.

### Step 2 — Create the transformation table

On a typical openHABian installation, create the text file:

`/etc/openhab/transform/ja_nein.map`

Example file contents (UTF-8):

```properties
# ja_nein.map — translate a rain-status Switch Item
ON=Rain
OFF=No rain
NULL=Unknown
UNDEF=Unavailable
```

The text on the right-hand side is what users see. For a German display, use `ON=Regen` and `OFF=Kein Regen` instead. The keys on the left must match the actual Item states; `ON`/`OFF` apply to a Switch Item, whereas a String Item may need different keys. Keep the file name and capitalization consistent with the sitemap reference.

For example, to create the English file from an openHABian terminal:

```bash
sudo tee /etc/openhab/transform/ja_nein.map >/dev/null <<'EOF'
ON=Rain
OFF=No rain
NULL=Unknown
UNDEF=Unavailable
EOF
```

### Step 3 — Reference the MAP file in your sitemap

Use the standard openHAB sitemap label transformation syntax:

```text
sitemap ohdrive label="OHDrive" {
    Frame label="Weather" {
        Text item=Rain_Status_Verifiziert label="Rain [MAP(ja_nein.map):%s]" icon="rain"
        Text item=OutdoorTemperature label="Outdoor temperature [%.1f °C]" icon="temperature"
    }
}
```

The `MAP(ja_nein.map)` expression tells openHAB which lookup file to use; `%s` inserts the transformed text. The actual Item name here, `Rain_Status_Verifiziert`, comes from the example installation and must exist on **your** openHAB server. If your rain Item has another name, replace it in the sitemap.

**Expected translation:**

| Raw Item state | `ja_nein.map` lookup | Displayed text |
| --- | --- | --- |
| `ON` | `ON=Rain` | Rain |
| `OFF` | `OFF=No rain` | No rain |
| `NULL` | `NULL=Unknown` | Unknown |
| `UNDEF` | `UNDEF=Unavailable` | Unavailable |

**Please note:** This is standard openHAB sitemap MAP syntax. Whether a particular OHDrive 3.0 screen renders a transformed label exactly like openHAB's own sitemap viewer depends on how that screen handles the sitemap response. Verify the result in openHAB first, then in OHDrive. If OHDrive displays the raw state instead, that is a client-side display limitation to report rather than a reason to change the Item's actual state.

### Step 4 — Test and troubleshoot

1. In openHAB, check that the Item exists and reports the expected raw states.
2. Confirm that **MAP Transformation** is installed and that `/etc/openhab/transform/ja_nein.map` exists.
3. Open the sitemap in openHAB's own sitemap viewer or through its REST representation to check the transformed label.
4. Load the same sitemap in OHDrive and check the rain tile.
5. If you see `ON`, `OFF`, a missing transformation or an unchanged label, check the add-on, exact filename, Item state and sitemap syntax. If openHAB transforms correctly but OHDrive does not, collect the OHDrive version and the relevant sitemap entry for a bug report.

**MAP vs. `mappings`:** A `.map` file translates a displayed **state**. The `mappings=[ON="Impulse"]` attribute on a sitemap `Switch` widget instead defines a **command button**. They serve different purposes; you do not need a MAP transformation add-on just to use `mappings=[...]` for a gate or garage-door impulse.

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

**OHDrive 3.0** continues the established OHDrive 2.9.27 codebase under the 3.0 version name. The project's recent development and testing include the sitemap-editing workflow and myopenHAB connectivity. The separate 3.0 RC1 host-validation experiment is documented as a test, not silently merged into the final app.

See `CHANGELOG.md` for the release history.

---

**OHDrive — openHAB controls where you need them: on Android Auto.**

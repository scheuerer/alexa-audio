# OHDrive 3.1 — Complete User Guide

**Control and monitor your openHAB installation through Android Auto.**

OHDrive is a companion application for an existing openHAB server. Set up connections, choose a sitemap, configure controls and favorites on your phone, then use the supported Android Auto interface in your vehicle.

> OHDrive is an independent project, not an official openHAB or Google application. Android Auto compatibility depends on your phone, installation method and vehicle display.

## Contents
1. [What OHDrive does](#what-ohdrive-does)
2. [Requirements and installation](#requirements-and-installation)
3. [First-time setup](#first-time-setup)
4. [Understanding sitemaps](#understanding-sitemaps)
5. [Sitemap editor — step by step](#sitemap-editor--step-by-step)
6. [File sitemaps versus live REST sitemaps](#file-sitemaps-versus-live-rest-sitemaps)
7. [Android Auto views and favorites](#android-auto-views-and-favorites)
8. [Control types and status](#control-types-and-status)
9. [Local and remote connections](#local-and-remote-connections)
10. [Troubleshooting](#troubleshooting)
11. [Building from source](#building-from-source)
12. [Safety and release notes](#safety-and-release-notes)

## What OHDrive does

OHDrive shows selected openHAB item values and controls on Android Auto. It offers a **Classic view** for sitemap entries and a **Favorites view** with one to four large tiles. You can switch views on the Android Auto screen, select favorites on the phone, and choose whether the favorites appear in the upper or lower row on a compatible four-column host.

Supported content includes text and numerical states, temperatures, weather/status values, switches, short impulse commands (for example, gate triggers), and roller/shutter controls. The exact commands depend on your configured item and control mode. The app also includes German and English interface options.

## Requirements and installation

You need:
- An Android phone running Android Auto and a compatible car display or head-unit emulator.
- A working openHAB installation accessible from your phone.
- At least one openHAB sitemap containing the items you want to show.
- The correct server address and authentication details (where required).

**APK installation:** Obtain the APK from the project's trusted GitHub release or build it from source. Install it using a method supported by your Android Auto configuration. A sideloaded APK may not automatically appear on every Android Auto device. Some community installation methods (such as AAEnabler or KingInstaller) have been reported by testers, but their availability and compatibility vary; use them at your own discretion. OHDrive is not currently a Play Store release.

After installation, open OHDrive **on the phone first** to configure the connection and sitemap. If it is missing from Android Auto, check the Android Auto launcher customization and the host's support for non-Play-Store apps.

## First-time setup

1. Open **OHDrive** on your phone.
2. Select the connection method: **Local** (your openHAB server or VPN) or **Cloud** (myopenHAB).
3. Enter the server details requested by the settings screen. For local access, use the host/IP, port and HTTP/HTTPS settings appropriate to your installation. Do not include a `.sitemap` suffix in the sitemap name.
4. Configure the authentication fields offered by the app for that connection. Use credentials or a token with only the permissions needed.
5. Select an existing sitemap, such as `ohdrive`, and save your settings.
6. Open the Android Auto interface and verify that values load before testing any controls.
7. Choose **Classic** or **Favorites** as your preferred view.

**Example:** If your openHAB server is reachable at `http://192.168.1.9:8080`, the host is `192.168.1.9` and the port is `8080`. Your own setup may instead use HTTPS and a reverse proxy.

## Understanding sitemaps

A **sitemap** is an openHAB definition of which items to display and how they should appear. It is **not** the same as an openHAB Main UI page. OHDrive uses the selected sitemap to populate the Android Auto controls and information entries.

A sitemap refers to existing **Items** in openHAB. Creating a sitemap entry does not create a new Item or configure the underlying device. Make sure the Item already exists and works in openHAB.

A simple illustrative file sitemap:

```text
sitemap ohdrive label="OHDrive"
{
    Frame label="Home" {
        Text item=Innentemperatur label="Indoor temperature [%.1f °C]"
        Switch item=GartenGaragentor_AA label="Garden garage gate"
        Text item=Rain_Status_Verifiziert label="Rain status [%s]"
    }
}
```

The item names above are examples from one installation; replace them with **your own** openHAB Item names. Not every openHAB sitemap widget or formatting option necessarily maps to an Android Auto control.

## Sitemap editor — step by step

The phone-side editor is intended to let you configure an OHDrive sitemap without hand-editing every line on your server.

1. Open the **Sitemap Editor** in OHDrive's phone settings.
2. Load/select the sitemap you want to edit.
3. Use the **Item search** to find an existing openHAB Item by name or label (for example, “temperature” or “power”).
4. Select the desired Item and configure its display name, icon and control mode.
5. Choose a position in the list. When inserting into the middle, later entries move down.
6. Use the **Classic** openHAB icon library or the **F7 / Framework7** icon library, where offered, and preview your selection.
7. Apply the entry and repeat for other Items.
8. Choose **Export** or **Save to openHAB (REST)**, depending on where you want the sitemap stored.
9. Return to the connection/sitemap selection and explicitly select the newly saved sitemap if necessary. Save settings and test it in Android Auto.

### Control mode selection

- **INFO / Text:** Read-only display of an Item state.
- **TOGGLE / Switch:** Send a switching command to an Item configured to accept it.
- **IMPULSE:** Send a brief command (commonly `ON`) to trigger an action. It should not be treated as a reliable indication that a gate or door is physically open or closed.
- **ROLLER:** Shutter/roller control, including the percentage direction option where available. Check whether your device interprets 0% as open or closed before using it.

### Icons and ordering

The editor can offer two icon libraries: **Classic** (openHAB) and **F7** (Framework7). The icon selection changes appearance; it does not change the Item's command behavior. Position numbers determine the sitemap entry order. **Sitemap entry order and Favorites selection are separate settings.**

## File sitemaps versus live REST sitemaps

This distinction is essential:

| | File-based sitemap | REST / live sitemap |
|---|---|---|
| Location | `/etc/openhab/sitemaps/<name>.sitemap` | Managed by openHAB via REST / JSONDB |
| How it is created | Create or copy a `.sitemap` file onto the openHAB server | Save through the OHDrive editor's REST option |
| How to update | Edit/replace the server file | Save the REST-managed definition again |
| Does REST saving overwrite it? | **No** | REST creates/updates its own managed sitemap |
| Selection in OHDrive | Select its sitemap name | Select the REST sitemap's name |

**What “live sitemap” means:** It is a sitemap stored through openHAB's REST-managed configuration rather than as a text file under `/etc/openhab/sitemaps/`. “Live” does **not** mean that it is automatically synchronized with an identically named file sitemap. The term also does not mean the Item states are different: both sitemap types refer to your live openHAB Items.

For example, you may have a file sitemap named `ohdrive` and save an edited REST sitemap named `ohdrive_live`. They are **two different definitions**. Saving `ohdrive_live` does not modify `/etc/openhab/sitemaps/ohdrive.sitemap`. To use the new definition, select `ohdrive_live` in OHDrive's sitemap settings and save.

**Export versus REST Save:** Export produces a `.sitemap` file for manual transfer to the openHAB server; exporting alone does not install that file on openHAB. REST Save sends the definition to the server's managed sitemap storage; it does not write a `.sitemap` file into `/etc/openhab/sitemaps/`.

**Useful checks (advanced):** openHAB exposes `GET /rest/sitemaps` to list available sitemaps and `GET /rest/sitemaps/<name>` for details. File and REST-managed sitemaps may both appear in this list. If a saved sitemap is not selectable, refresh the list and verify the server connection, name and permissions.

## Android Auto views and favorites

### Classic view

Shows the configured sitemap entries in the app's supported Android Auto layout. This is useful when you want more than four entries or a broader set of status information.

### Favorites view

Displays **one to four** selected sitemap Items as large tiles. Configure favorites on the phone:

1. Open **Android Auto view** settings.
2. Choose **Favorites** and open **Show favorites list**.
3. Tick or untick up to four Items. The list **stays open** when you select or deselect entries.
4. Tap **Close favorites list** explicitly when finished.
5. Save settings and open the Favorites view in Android Auto.

### Favorites position: Top or Bottom

Choose **Favorites position → Top / Bottom** in the phone settings.

- **Top:** No transparent tiles are inserted before the favorites.
- **Bottom:** On the tested four-column Android Auto display, four transparent, non-interactive placeholders are inserted before four favorites so the visible tiles occupy the lower row.

The Android Auto host controls the actual grid. Results can differ by car, display size, split-screen mode and number of selected favorites. This is a layout workaround, **not guaranteed vertical centering**.

### Changing favorites order

In **3.1**, the order of favorites is not independently rearrangeable with arrow controls. A separate reorder feature was planned for **3.1.1**; consult that release's documentation when available. Do not confuse this with the sitemap editor's entry-position setting.

## Control types and status

- **Read-only tiles** display current Item states, such as temperature or rain.
- **Toggle controls** send commands to compatible openHAB Items.
- **Impulse controls** send an action command, show a sending/waiting state and avoid presenting the impulse as a confirmed physical gate position.
- **Roller controls** use the configured percentage direction; test open/close semantics before use.

OHDrive relies on openHAB for Item state and device control. A successful command submission does not guarantee that a physical device moved. If live connectivity is missing, safety-related actions may be unavailable. Verify actual device status independently.

## Local and remote connections

**Local:** Connect directly to your openHAB host on the home network or through a VPN. Use the actual address, protocol and port configured on your system.

**Cloud:** With an active myopenHAB connection, OHDrive can access the configured openHAB installation remotely when the selected authentication method and network permit it. Check connectivity in the phone app before driving.

Avoid exposing an unsecured openHAB HTTP port to the public internet. Prefer HTTPS and properly secured remote access. Keep passwords and tokens out of public screenshots and GitHub issues.

## Troubleshooting

**App not visible in Android Auto:** Check installation compatibility, Android Auto launcher settings, and whether your head unit permits the installed app. A successful APK copy is not the same as an installation.

**No values / disconnected:** Check local versus cloud selection, host, port, HTTPS, authentication and sitemap name. Test the openHAB REST endpoint from a trusted device on the same network.

**New sitemap not shown:** Refresh sitemap selection and select the REST-created name. Exporting a file does not upload it automatically.

**REST save did not change the file:** Expected behavior. REST-managed sitemaps do not overwrite `/etc/openhab/sitemaps/*.sitemap`.

**Wrong command behavior:** Confirm the Item type and the selected INFO / TOGGLE / IMPULSE / ROLLER mode. For rollers, check percentage direction.

**Favorites appear too low / in a different grid:** Try Top or Bottom and test with four favorites. The host decides the final layout.

**Gate tile shows waiting:** This indicates an impulse/command workflow, not confirmation of physical movement.

## Building from source

On Windows, extract the source archive to `C:\OHDrive_3.1`, install the Android SDK and a compatible Java toolchain, and run PowerShell:

```powershell
Set-Location "C:\OHDrive_3.1"
$ErrorActionPreference = "Stop"
$sdk = Join-Path $env:LOCALAPPDATA "Android\Sdk"
"sdk.dir=$($sdk.Replace('\','/'))" | Set-Content ".\local.properties" -Encoding ASCII
.\gradlew.bat assembleDebug --console=plain
if ($LASTEXITCODE -ne 0) { throw "BUILD FAILED" }
$apk = ".\app\build\outputs\apk\debug\app-debug.apk"
if (-not (Test-Path $apk)) { throw "APK not found" }
Copy-Item $apk (Join-Path $env:USERPROFILE "Downloads\OHDrive_3.1.apk") -Force
```

This creates a **debug APK** in your Downloads folder. It is not a production-signed Google Play release. Copying the APK to a phone and installing it are separate steps.

## Safety and release notes

Only operate doors, gates, shutters and other equipment when it is safe and legal to do so. Do not interact with controls while driving if doing so would distract you. The app cannot independently establish whether a device is physically in a safe state.

Version **3.1** incorporates the tested Top/Bottom favorites position from 3.0.14. For detailed history see [CHANGELOG.md](CHANGELOG.md).

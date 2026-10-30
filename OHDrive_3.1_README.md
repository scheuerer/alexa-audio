# OHDrive 3.1

**openHAB controls and live information on Android Auto.**

OHDrive is an Android Auto companion app for an existing openHAB installation. It reads items from an openHAB sitemap and presents selected information and controls on the vehicle display. Configuration and sitemap editing are handled on the Android phone.

> **Release 3.1 (30 October 2026):** Promotes the successfully tested 3.0.14 favorite-position feature to version 3.1. No functional changes were made to the 3.0.14 application logic.

## Features

- **Two Android Auto views:** Classic (sitemap entries) and Favorites (up to four large tiles).
- **Favorite selection on the phone:** Choose one to four sitemap items. The selection list stays open until you explicitly close it.
- **Favorite position:** Choose **Top** or **Bottom** in the phone settings. On the tested four-column Android Auto host, Bottom uses transparent grid placeholders to move the favorites into the lower row; Top adds no placeholders. Actual positioning is controlled by the Android Auto host and may vary by vehicle/display.
- **Live values:** Display openHAB item states, including temperatures and other status information.
- **Controls:** Supports information tiles, toggles, impulse commands and roller controls (subject to the sitemap configuration and available connection).
- **Impulse feedback:** Impulse tiles show a waiting/command status instead of treating ON/OFF as the physical state of a door.
- **View switch:** Switch between Classic and Favorites directly in Android Auto, or choose the default view on the phone.
- **Local or remote access:** Connect via a local openHAB server/VPN or myopenHAB Cloud.
- **Sitemap editor:** Find and arrange items on the phone, adjust icons and control types, export a sitemap file or save a REST-backed sitemap where supported.
- **German and English interface.**

## Requirements

- An Android phone with Android Auto and a compatible Android Auto host/head unit.
- A reachable openHAB installation with a configured sitemap.
- Appropriate openHAB credentials or API token for the selected connection method.
- For building from source: Android SDK, Java 17, and the included Gradle wrapper (`gradlew.bat` on Windows).

## Configuration

1. Install the built APK on a compatible Android phone using an installation method supported by your Android Auto setup.
2. Open **OHDrive** on the phone and configure your local server/VPN or myopenHAB Cloud connection.
3. Enter the openHAB sitemap name (for example, `ohdrive`, without `.sitemap`) and configure authentication.
4. Test the connection and save settings.
5. Under **Android Auto view**, choose **Classic** or **Favorites**. For Favorites, load sitemap items and select up to four.
6. Choose **Favorites position: Top / Bottom**, save settings, and reopen the Android Auto view to check the result.

### Important notes about favorite positioning

Android Auto renders `GridTemplate` layouts according to the host's available space. OHDrive's Bottom option inserts transparent, non-interactive grid entries before the actual favorites. On the tested four-column display, four placeholders move four favorites to the second row. Other displays may arrange or limit grid items differently; precise vertical centering is not guaranteed.

### Sitemap storage

A file-based sitemap under `/etc/openhab/sitemaps/` is distinct from a sitemap saved via openHAB REST/JSONDB. Saving a REST sitemap does **not** overwrite the file-based sitemap. The phone's export function provides a file that can be copied to the openHAB server separately.

## Build on Windows (PowerShell)

Extract this source archive to `C:\OHDrive_3.1`, then run:

```powershell
Set-Location "C:\OHDrive_3.1"
$sdk = Join-Path $env:LOCALAPPDATA "Android\Sdk"
"sdk.dir=$($sdk.Replace('\','/'))" | Set-Content ".\local.properties" -Encoding ASCII
.\gradlew.bat assembleDebug --console=plain
```

The debug APK is created at `app/build/outputs/apk/debug/app-debug.apk`. This is a **debug build**, not a production-signed Play Store release. Installing an APK does not by itself guarantee that Android Auto will list it; availability depends on the Android Auto host and installation method.

## Safety

Only operate gates, doors, shutters or other connected devices when doing so is safe and permitted. Do not interact with the app in a way that distracts from driving. OHDrive relies on the underlying openHAB configuration and connection; it does not independently verify the physical state of a controlled device.

## Release history

See [CHANGELOG.md](CHANGELOG.md) for release notes.

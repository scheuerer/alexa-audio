# OHDrive 3.0 🚗🏠

**Your openHAB dashboard on Android Auto — at home, over VPN, or through myopenHAB Cloud.**

OHDrive brings a carefully selected set of openHAB items to your car's Android Auto display. Check temperatures and weather conditions, view device states, and operate supported controls from a configurable, tile-based interface. Rather than hard-coding your devices, OHDrive reads an **openHAB sitemap**, so you decide what appears in the car.

> **Version:** 3.0.0 · **Platform:** Android / Android Auto · **Backend:** openHAB · **Package:** `de.ohdrive.app`

## ✨ What's new in 3.0

### ☁️ myopenHAB Cloud — your home while you're away

OHDrive is no longer limited to a server reachable on your home Wi-Fi or through a VPN. Select **myopenHAB Cloud** as the connection type and use your myopenHAB account to connect to your online openHAB installation through `https://myopenhab.org`.

That means the same dashboard can show live item values and send supported commands while your phone uses **mobile data**. For example, you can check the temperature at home or operate a configured socket without first connecting to your local network.

**How to set it up:**

1. Configure the **openHAB Cloud Connector** in your openHAB installation and make sure your system is online in your myopenHAB account.
2. Open **OHDrive → Settings → Connection type**.
3. Choose **myopenHAB Cloud** and enter your account email and password.
4. Enter the name of the sitemap you want OHDrive to display (for example, `ohdrive`, without `.sitemap`).
5. Save the settings and test the connection. Open Android Auto to check your tiles.

The cloud password is stored encrypted on the phone. A working internet connection, an online openHAB system, and valid credentials are required. Cloud access depends on the availability of the myopenHAB service.

**Tested:** remote access to an online openHAB test installation, correct item values, and successful socket switching through the phone's Android Auto head-unit emulator.

### 🧩 Integrated sitemap editor

Customize your in-car dashboard directly from the phone instead of editing everything by hand:

- Load a sitemap and inspect its entries.
- Choose from available sitemaps reported by the openHAB REST API.
- Search openHAB items by name or label, or enter an item name manually.
- Add, edit, and remove supported tiles; adjust their display names and icons.
- Choose icons from **Classic** and **F7** libraries, including icon search and selection.
- Configure supported display and control modes, including switch, impulse, and percentage-oriented controls.
- **Export a `.sitemap` file to the phone's Downloads folder** for manual transfer to your openHAB server.

**Important distinction — export vs. REST save:**

- **File export:** saves a `.sitemap` file under **Downloads**. You then copy it to `/etc/openhab/sitemaps/` yourself. Export does **not** automatically switch the active sitemap in OHDrive.
- **REST save:** the editor provides an option to write a **separate** REST-managed sitemap named `ohdrive_live`. It does **not** overwrite the original file-based sitemap. REST saving has had unresolved reliability issues and should be treated as experimental; use file export for the more predictable workflow.

The editor supports a defined subset of sitemap widgets; complex existing sitemaps may contain constructs the exporter cannot reproduce. Keep a backup of your original sitemap.

### 🚘 Android Auto dashboard

- A car-oriented **tile/grid view** of selected openHAB items.
- Item labels, values, and configurable icons derived from your sitemap.
- Supported **switches**, **momentary/impulse controls**, and other configured device controls.
- Read-only status tiles for values such as temperature, humidity, rainfall, and other measurements.
- Support for sitemap-driven icons, including Classic and F7 choices.
- Local-network, VPN, and myopenHAB Cloud connection options.
- German and English interface text in relevant app screens.

The exact set of tiles and actions depends on your sitemap and the types of items configured in openHAB. Android Auto and the vehicle/head unit may impose their own display and interaction restrictions.

## 🏁 Getting started

### Requirements

- An operational **openHAB** server with accessible REST API.
- A configured openHAB sitemap containing the items you want to show.
- An Android phone and a compatible Android Auto environment.
- For remote cloud access: an online openHAB Cloud Connector and a myopenHAB account.

### 1. Prepare your sitemap

Create or edit a sitemap on your openHAB server, typically under:

```text
/etc/openhab/sitemaps/ohdrive.sitemap
```

A simple starting point:

```java
sitemap ohdrive label="OHDrive" {
    Frame label="Home" {
        Text item=Outdoor_Temperature label="Outside temperature [%.1f °C]" icon="temperature"
        Switch item=Garage_Light label="Garage light" icon="light"
        Switch item=Garden_Gate_Pulse label="Garden gate" icon="door"
    }
}
```

Replace these **example item names** with actual items from your installation. The example illustrates the sitemap structure; configure command behavior for a momentary relay in the editor and verify it carefully before use. A standard `Switch` line alone does not guarantee a safe timed pulse.

### 2. Configure OHDrive

Open the app on your phone and enter:

| Setting | Local network / VPN | myopenHAB Cloud |
|---|---|---|
| Connection type | Local server / VPN | myopenHAB Cloud |
| Server | Your openHAB host/IP | Managed by cloud mode |
| Port | Your openHAB port (commonly `8080`) | Managed by cloud mode |
| Credentials | As required by your server | myopenHAB email and password |
| Sitemap | e.g. `ohdrive` | e.g. `ohdrive` |

Save your settings and use the connection test. You can change the connection type later without rebuilding the app.

### 3. Open Android Auto

Connect your phone to a compatible Android Auto head unit or use a development head-unit emulator. Open OHDrive and confirm that your sitemap items appear. Test harmless, non-critical controls first.

**Installation note:** The currently shared `assembleDebug` APK is a **debug-signed sideload build**, not a Play Store production release. Depending on the phone, Android Auto version, and installation method, sideloaded apps may not appear in the Android Auto launcher. Some testers use AA Enabler; visibility is **not guaranteed** across devices. Do not disable device protections without understanding the implications.

## 🧭 Known limitations and roadmap

- **Sitemap editor:** REST saving, some item-search cases, newly added tile alignment, and discovery of file-based sitemaps have been reported as areas for improvement. A sitemap selection UI exists, but discovery may depend on what the REST API exposes.
- **Sitemap export:** supports the editor's known widget types, not arbitrary complex sitemap syntax.
- **Android Auto discovery:** compatibility can differ by phone vendor, Android Auto version, and sideloading method. A successful install does not always mean the app appears in the car launcher.
- **Release signing:** the provided debug APK is for testing/sideloading, not a production-signed public Play Store distribution.
- **Apple CarPlay:** not supported; the project currently focuses on Android Auto.

## 🔒 Safety and privacy

Use OHDrive only where operation is safe, lawful, and permitted by your vehicle's interface. **Do not interact with the app while driving if it would distract you.** For garage doors, gates, locks, and other physical actuators, ensure appropriate safeguards, visibility, and authorization before enabling remote commands. Never expose your openHAB server to the internet without proper authentication and transport security; prefer a properly configured VPN or myopenHAB Cloud.

## 📦 Version history

**3.0.0 — October 28, 2026**

- Highlights the integrated sitemap editor and remote **myopenHAB Cloud** connectivity.
- Builds on the tested **2.9.27** codebase; the final 3.0.0 change updates version metadata only, without additional feature-code changes.
- See [`CHANGELOG.md`](CHANGELOG.md) for release details.

---

**OHDrive is an independent community project for openHAB and Android Auto.** It is not an official openHAB, Google, or Android Auto product.

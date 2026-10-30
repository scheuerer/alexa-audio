# Changelog

## 3.1 — 2026-10-30

- Promoted the user-tested 3.0.14 behavior to the 3.1 release.
- Added configurable **Favorites position: Top / Bottom** (introduced in 3.0.14): Top starts the grid with favorite tiles; Bottom places transparent placeholders ahead of the tiles to move them down on the tested four-column host.
- Retained up to four configurable favorites, live status text, impulse controls, the Classic/Favorites switch, sitemap editor and local/myopenHAB Cloud connectivity.
- Updated GitHub README and changelog documentation.
- Updated Android app `versionName` to `3.1` and `versionCode` to `73`.
- No additional application behavior changes compared with 3.0.14.

## 3.0.14 — 2026-10-30

- Added the phone setting **Favorites position: Top / Bottom**.
- Kept Bottom as the default, preserving earlier behavior.
- Used four transparent grid placeholders before four favorites in Bottom mode; Top mode uses none.

## 3.0.13 — 2026-10-30

- Improved the favorites selection workflow: selecting items no longer automatically closes the list.
- The selection list can be closed explicitly using its own button.

## 3.0.11 (test)

- Combine horizontal and vertical grid positioning: five inert transparent slots before two gate favorites.
- Preserve API 8, large grid item size, and existing impulse commands.
- Diagnostic marker GRID311_DIAG.

## 3.0.10 (vertical positioning test)
- Four inert transparent slots before the two gate favorites to test second-row placement.
- No changes to impulse commands, settings, editor or connectivity.
- GRID310_DIAG.

## 3.0.8 (experimental favorite positioning)
- Added one inert transparent grid placeholder before two gate favorites to test horizontal placement.
- Retained Car API 8, large grid request, and existing impulse logic.
- Removed unsupported setImageType call.

## 3.0.7 (Car API 8 large grid test)
- Requested large grid tiles and large icons for two gate impulse favorites.
- Preserved command logic, settings and connection behavior.

## 3.0.6 (API 8 negotiation test)
- AndroidX Car App Library 1.4.0 -> 1.8.0-rc01.
- Added API8_DIAG logs; PaneTemplate and impulse commands unchanged.
- No UI/editor/cloud changes.


## 3.0.1 — 2026-10-30

- Corrected system-bar handling on the entire settings screen, including Android 16 three-button navigation, gestures, display cutouts and the keyboard.
- Moved "Save settings" directly below the sitemap editor into the scrollable form.
- Unified outer gaps between cards and action blocks to 16 dp.
- Kept readable light text in the connection type and language selectors.
- Version 3.0.1 correction, versionCode 58.

## 3.0.0 — 2026-10-28

- Renamed the tested OHDrive 2.9.27 source to version 3.0.0 (versionCode 55).
- No application logic, permissions, package identifier, host-validation behavior, or signing configuration changed compared with 2.9.27.
- Debug APK remains intended for testing and sideloading, not as a production-signed public release.

## 2.9.27

- Prepared host validation for a future release: debug builds accept development hosts; release builds use an AndroidX host allowlist.
- Preserved openHAB, cloud, sitemap, and diagnostic behavior.

## 2.9.26

- Added Android Auto lifecycle and host-validation diagnostic logging under `OHDriveAA-DIAG`.

## 2.9.24

- Added a local/VPN versus myopenHAB Cloud connection option.
- Added cloud credentials protected using Android Keystore AES/GCM.
- Shared connection settings across the app, sitemap editor, REST, and SSE.

## 2.9.21

- Corrected the default position when inserting a new sitemap tile.

## 2.9.20

- Improved default placement for new sitemap tiles and synchronized the position field after edits.

Note: Entries above summarize only changes documented in the bundled version notes. They are not a complete history of all earlier OHDrive releases.

## 3.0.5 (test)
- PaneTemplate test with two gate impulse actions, preserving existing command handling.
- PANE_DIAG logging for negotiated Car API and host-rendered presentation.

## 3.0.12 (test)
- Configurable Android Auto view: Classic or Favorites.
- Favorites selection from the selected openHAB sitemap (up to 3 items).
- Existing two-favorite positioning retained; three favorites use four leading spacers.
- Dark settings card with uniform spacing, white labels, and matching controls.
- Car API 8 and impulse commands retained.

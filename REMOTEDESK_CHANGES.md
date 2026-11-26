# RemoteDesk changes

This repository is a fork of [RustDesk](https://github.com/rustdesk/rustdesk) (AGPL-3.0),
used as the engine of RemoteDesk. Every file we add or change is listed here, with the reason,
so upstream merges and AGPL reviews stay easy.

- Upstream base: tag `1.5.3`
- Work branch: `remotedesk/main` (rebased onto each new upstream release tag)
- `libs/hbb_common` fork: https://github.com/tguankheng016/hbb_common

## Changed files

| File | Change | Why |
|---|---|---|
| `.gitmodules` | `libs/hbb_common` URL points at our hbb_common fork | Branding and default-server changes live in hbb_common |
| `.github/workflows/remotedesk-build.yml` | New. Windows x64 build, trimmed from `flutter-build.yml` | CI for our engine; uploads the portable exe, the MSI and `SHA256SUMS` |
| `REMOTEDESK_CHANGES.md` | New (this file) | Change log for merges and AGPL review |
| `src/common.rs` | Added `get_app_display_name()` | Customer-facing name, kept apart from the technical `APP_NAME` |
| `src/lang.rs` | Translated text replaces "RustDesk" with the display name | "MochoDesk Remote" in every UI string |
| `src/flutter_ffi.rs`, `src/flutter.rs` | `main_get_app_name(_sync)` and `get_rustdesk_app_name` return the display name | Window titles; `main.cpp` `FindWindowW` matches the Flutter title |
| `src/tray.rs` | Tray tooltip uses the display name | Branding |
| `src/platform/windows.rs` | Apps & Features DisplayName/Publisher and the service DisplayName use the display name | Branding; service, folder and exe names stay `MochoDeskRemote` |
| `flutter/lib/desktop/widgets/tabbar_widget.dart` | Title bar text from `mainGetAppNameSync()` instead of a literal "RustDesk" | Branding |
| `flutter/lib/desktop/pages/desktop_setting_page.dart` | About: removed rustdesk.com privacy/website links and slogan, added "Based on RustDesk (AGPL-3.0)" and a link to this repo | AGPL attribution and source offer |
| `flutter/windows/runner/Runner.rc` | Company, product, description and copyright strings | Branding (Explorer file properties) |
| `res/icon.png`, `res/mac-icon.png`, `res/32x32.png`, `res/64x64.png`, `res/128x128*.png`, `res/icon.ico`, `res/tray-icon.ico`, `flutter/windows/runner/resources/app_icon.ico` | MochoDesk logo | Branding |
| `flutter/assets/icon.png` | New. MochoDesk logo (256 px); `loadIcon` prefers it over `icon.svg` | Branding |
| `res/msi/preprocess.py` | New `--display-name` (MSI ProductName, shortcuts, installer text); Apps & Features links point at this repo; `update_license_file` disabled | Customers see "MochoDesk Remote"; the tray shortcut keeps `MochoDeskRemote Tray` because the engine deletes it by that name |
| `res/msi/Package/Package.wxs` | `Package Name` uses `$(var.DisplayName)` | Apps & Features shows "MochoDesk Remote" |
| `res/msi/Package/License.rtf` | Replaced RustDesk's EULA with our AGPL-3.0 notice, RustDesk attribution and source link | Upstream's rewrite made the EULA read as if we hold RustDesk's copyright |
| `src/platform/windows/msi_registry.rs` | MSI uninstall entries match the app name or the display name | The MSI's DisplayName is now the display name |

## Changes in `libs/hbb_common`

Branch `remotedesk/main` of the hbb_common fork.

| File | Change | Why |
|---|---|---|
| `src/config.rs` | `APP_NAME` = `MochoDeskRemote`; new `APP_DISPLAY_NAME` = `MochoDesk Remote`; macOS `ORG` = `com.mochodesk` | Separate service, folders, registry keys and URI scheme (`mochodeskremote://`) from a real RustDesk install. No spaces in `APP_NAME`: it is passed unquoted to `taskkill`, `reg` and `sc` |

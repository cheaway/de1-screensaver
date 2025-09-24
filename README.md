# Unsplash Coffee Screensaver

This plugin pulls random Unsplash photography and shows it full-screen as the Decent DE1 screensaver. The Unsplash access key, search term, and refresh cadence are all configurable from the plugin settings page.

## Setup

1. Create an Unsplash developer application and copy the Access Key.
2. Copy this plugin folder into the DE1 `plugins` directory and enable it from the plugin manager.
3. Open the plugin's settings page (requires the DGUI plugin) and enter your access key, preferred search term, and refresh interval. Tap **Done** to save.
4. Optionally set the environment variable `UNSPLASH_ACCESS_KEY` before launching the DE1 app. The plugin uses the UI-supplied key first and falls back to the environment variable when left blank.
5. Wire the `start`/`stop` handlers in `plugin.json` into your screensaver lifecycle if your DE1 build requires manual registration.

### Deploying from GitHub

- **Manual download** – grab the latest archive from your GitHub repository (`Code` ▸ `Download ZIP`), unzip it on the tablet, and copy the folder into `de1app/plugins/coffee_screensaver`.
- **Via the GitHub Plugin Manager** – if you have Enrique Bengoechea's _GitHub plugins_ manager installed, add your repository URL to its list and install/refresh the plugin directly on the tablet.
- **Git clone (developers)** – on a desktop, `git clone https://github.com/<your-account>/coffee-screensaver.git`, then sync the folder to the tablet's `de1app/plugins` directory (e.g. over `adb push` or network share).

## Configuration options

- **Unsplash access key** – stored with the plugin settings. Leave empty to read `UNSPLASH_ACCESS_KEY` from the environment instead.
- **Search term** – defaults to `coffee`. Any valid Unsplash query works.
- **Refresh interval (minutes)** – defaults to 5 minutes. Values below 0.5 minutes are coerced to 0.5.

## Runtime behaviour

- Downloaded images are cached under `cache/` inside the plugin directory.
- Each interval the plugin requests `orientation=landscape` images tagged with the configured search term.
- Photographer attribution (name + Unsplash link) is rendered across the bottom edge. Any pointer or key input exits the screensaver and stops the refresh loop until the screensaver is invoked again.

## Troubleshooting

- Ensure Tcl packages `http`, `tls`, and `json` are installed in the DE1 runtime. Without TLS, HTTPS downloads will fail.
- If no image appears, double-check that the access key is valid and that the device has network access. Errors are logged to the Tcl console (stderr).
- When DGUI is unavailable the settings UI is skipped; you can still supply configuration via the environment variable or by editing `settings.tcl` after the plugin runs once.

## Notes

Plugin wiring can vary between DE1 releases. Adjust the entry points in `plugin.json` to match your firmware's expectations if necessary. The Tcl code is self-contained and can be adapted to other DE1 contexts that want a rotating image background.

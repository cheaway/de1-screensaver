# Unsplash Coffee Screensaver

This plugin pulls random Unsplash photography and points the built-in DE1 screensaver at those downloads. The Unsplash access key, search term, and refresh cadence are configurable from the plugin's native settings page.

## Setup

1. Create an Unsplash developer application and copy the Access Key.
2. Copy this plugin folder into the DE1 `plugins` directory; the bundled `plugin.tcl` shim makes the app register it automatically on the next launch.
3. Restart the DE1 tablet app, open **Settings → Advanced → Extensions**, and enable **Unsplash Coffee Screensaver**.
4. Tap the plugin's **Configure** button, enter your access key, preferred search term, and refresh interval, then tap **Done** (or **Apply**) to save.
5. Optionally set the environment variable `UNSPLASH_ACCESS_KEY` before launching the DE1 app. The plugin uses the UI-supplied key first and falls back to the environment variable when left blank.

### Deploying from GitHub

- **Manual download** – grab the latest archive from your GitHub repository (`Code` ▸ `Download ZIP`), unzip it on the tablet, and copy the folder into `de1app/plugins/coffee_screensaver`.
- **Via the GitHub Plugin Manager** – if you have Enrique Bengoechea's _GitHub plugins_ manager installed, add your repository URL to its list and install/refresh the plugin directly on the tablet.
- **Git clone (developers)** – on a desktop, `git clone https://github.com/<your-account>/coffee-screensaver.git`, then sync the folder to the tablet's `de1app/plugins` directory (e.g. over `adb push` or network share).

## Configuration options

- **Unsplash access key** – stored with the plugin settings. Leave empty to read `UNSPLASH_ACCESS_KEY` from the environment instead.
- **Search term** – defaults to `coffee`. Any valid Unsplash query works.
- **Refresh interval (minutes)** – defaults to 5 minutes. Values below 0.5 minutes are coerced to 0.5.

## Runtime behaviour

- Downloaded images are cached under `cache/2560x1600/` inside the plugin directory. The DE1 app rescales them for the current screen resolution the first time they are used.
- The plugin refreshes the cache on the configured interval and resets the built-in `saver` image list so the next screensaver cycle picks up the newest photo.
- A short status line on the configuration page shows the last successful download (timestamp, photographer, and Unsplash link).

## Troubleshooting

- Ensure Tcl packages `http`, `tls`, and `json` are installed in the DE1 runtime. Without TLS, HTTPS downloads will fail.
- If no image appears, double-check that the access key is valid and that the device has network access. Status and error messages are shown at the bottom of the configuration page and logged to stderr.
- You can edit the fallback `settings.tcl` manually if you prefer not to use the UI or if you need to pre-seed values before the first launch.

## Notes

Plugin wiring can vary between DE1 releases, but the supplied `plugin.tcl` and `main` entry point follow the layout used in current firmware. The Tcl code is self-contained and can be adapted to other DE1 contexts that want a rotating image background.

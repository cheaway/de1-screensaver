# Unsplash Coffee Screensaver

This plugin displays beautiful random images from Unsplash as your DE1 XXL espresso machine's screensaver. The Unsplash access key, search term, and refresh cadence are all configurable from the plugin's intuitive settings page.

## Setup

1. Copy this plugin folder into the DE1 `plugins` directory; the bundled `plugin.tcl` shim makes the app register it automatically on the next launch.
2. Restart the DE1 tablet app, open **Settings → Advanced → Extensions**, and enable **Unsplash Coffee Screensaver**.
3. Tap the plugin's **Configure** button to access the settings page.
4. The plugin comes pre-configured with a default API key, but you can optionally:
   - Enter your own Unsplash API key (create a free account at https://unsplash.com/developers)
   - Customize the search term (default: "coffee")
   - Adjust the refresh interval (default: 5 minutes)
   - Set the maximum number of cached images (default: 20)
5. Tap **Apply** to save settings or **Fetch Now** to immediately download a new image.
6. Advanced: Set the environment variable `UNSPLASH_ACCESS_KEY` before launching the DE1 app to use a different default key.

### Deploying from GitHub

- **Manual download** – grab the latest archive from your GitHub repository (`Code` ▸ `Download ZIP`), unzip it on the tablet, and copy the folder into `de1app/plugins/coffee_screensaver`.
- **Via the GitHub Plugin Manager** – if you have Enrique Bengoechea's _GitHub plugins_ manager installed, add your repository URL to its list and install/refresh the plugin directly on the tablet.
- **Git clone (developers)** – on a desktop, `git clone https://github.com/<your-account>/coffee-screensaver.git`, then sync the folder to the tablet's `de1app/plugins` directory (e.g. over `adb push` or network share).

## Configuration options

- **Unsplash access key** – stored with the plugin settings. Leave empty to read `UNSPLASH_ACCESS_KEY` from the environment instead.
- **Search term** – defaults to `coffee`. Any valid Unsplash query works.
- **Refresh interval (minutes)** – defaults to 5 minutes. Values below 0.5 minutes are coerced to 0.5.
- **Max cached images** – defaults to 20. The plugin keeps only this many photos on disk, always replacing the oldest when new shots arrive.

## Runtime behaviour

- Downloaded images are cached under `cache/2560x1600/` inside the plugin directory. The DE1 app rescales them for the current screen resolution the first time they are used.
- The plugin enforces the configured cache limit like a ring buffer: it prunes the oldest photo before saving a new one and resets the built-in `saver` image list so the next screensaver cycle picks up fresh content.
- A short status line on the configuration page shows the last successful download (timestamp, photographer, and Unsplash link).

## Troubleshooting

### Requirements
- **Tcl packages**: `http`, `tls` (1.7+ recommended), and `json` must be installed
- **TLS version**: Unsplash API requires TLS 1.2+ with SNI (Server Name Indication) support
- **Internet connection**: Active network connection required for downloading images
- **Unsplash API key**: Obtain a free API key from https://unsplash.com/developers

### Common Issues

#### No images appear
1. **Check API key**: Ensure you've entered a valid Unsplash API key in the settings page
2. **Verify network**: Confirm the tablet has internet access
3. **Check logs**: Status messages appear at the bottom of the configuration page
4. **Test manually**: Press the "Fetch Now" button to trigger an immediate download

#### TLS/SSL errors ("software caused connection abort", "protocol version")
- The Unsplash API requires modern TLS with SNI support (TLS 1.7+)
- If you see connection errors, your Tcl/TLS version may be too old
- The DE1 tablet should have a compatible version; this typically only affects local development
- Error messages are logged and displayed on the settings page for debugging

#### Images download but don't display
- Check that the cache directory exists: `[plugin-dir]/cache/2560x1600/`
- Verify images are actually downloaded (check the cache directory)
- The DE1 app may need to be restarted to recognize the new screensaver directory

#### Settings not saved
- Settings are automatically saved when you press "Apply" or "Done"
- Settings are stored in `settings.tcl` as a fallback
- You can manually edit `settings.tcl` if needed

## Notes

Plugin wiring can vary between DE1 releases, but the supplied `plugin.tcl` and `main` entry point follow the layout used in current firmware. The Tcl code is self-contained and can be adapted to other DE1 contexts that want a rotating image background.

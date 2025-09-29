# Changelog

## Version 1.4.0 (2025-09-29)

### Major Improvements

#### 🔧 Fixed Core Functionality
- **Fixed HTTPS/TLS handling**: Completely rewrote the `https_socket` handler with proper list expansion and SNI support
- **Improved settings initialization**: Better handling of missing or empty settings
- **Enhanced error handling**: Added comprehensive try-catch blocks with detailed error messages
- **Better logging**: Added extensive logging throughout the entire flow for debugging
- **TLS compatibility**: Added fallback for older TLS versions while supporting modern SNI for security

#### 🎨 UI/UX Improvements
- **Completely redesigned settings page** with modern, clean layout:
  - Better color scheme (professional blues, greens, and grays)
  - Improved typography and spacing
  - Larger, more visible input fields with proper borders
  - Three well-styled buttons: "Fetch Now" (green), "Apply" (blue), "Done" (dark)
  - Better visual hierarchy with clear labels and descriptions
  - Status messages now show success with checkmarks
  - More helpful help text at the bottom

#### 📝 Enhanced Feedback
- **Real-time status updates**: Shows what's happening during image fetch
  - "Fetching image from Unsplash..."
  - "✓ [timestamp] — Photo by [photographer]"
  - Clear error messages when something goes wrong
- **Verbose logging**: Every step is logged for troubleshooting
  - HTTP status codes and responses
  - Settings values
  - Success/failure of each operation

#### 🔍 Debugging Features
- Added detailed logs for:
  - Plugin activation sequence
  - API key resolution (shows first 11 characters for security)
  - HTTP request/response status
  - Image download success
  - Storage operations

### Technical Changes

1. **coffee_screensaver.tcl**:
   - `https_socket()`: Complete rewrite using proper `{*}` list expansion instead of `concat`
   - `https_socket()`: Added SNI support with fallback for older TLS versions
   - `check_settings()`: Simplified settings initialization
   - `fetch_random_image()`: Added detailed logging for API calls and responses
   - `refresh_image()`: Enhanced with step-by-step logging and better error messages
   - `activate()`: Wrapped entire activation in try-catch with detailed logging
   - `build_ui()`: Complete redesign with modern layout, better spacing, and improved styling

2. **plugin.json**:
   - Updated version to 1.4.0

3. **README.md**:
   - Added comprehensive troubleshooting section
   - Documented TLS requirements (1.7+ recommended)
   - Added common issues and solutions
   - Better organization and clarity

4. **settings.tcl**:
   - No changes (API key must be entered via UI or environment variable)

### Bug Fixes
- **Fixed critical HTTPS connection issue**: The original `https_socket` handler used `concat` which broke list expansion, preventing proper TLS connections
- **Fixed SNI support**: Proper handling of Server Name Indication for modern TLS servers
- **Fixed initialization**: Better error handling during plugin activation
- **Improved error messages**: All errors now provide actionable information for troubleshooting

### Known Limitations
- Requires internet connection to fetch images
- Requires Tcl packages: http, tls, json
- TLS 1.7+ required for SNI support (Unsplash API requirement)

---

## Version 1.3.0 (Previous)
- Initial implementation
- Basic UI
- Unsplash integration
- Cache management

# Disroot Webchat for Android
A simple tool to convert the  [Disroot Webchat](https://webchat.disroot.org) (Movim) into an Android APK without requiring Android Studio or Java programming knowledge; as with the original [website-to-apk](https://github.com/Jipok/website-to-apk) by Jipok, the app acts as a WebView wrapper around the Disroot Webchat Instance.

## Features

- Simple command-line interface with colorful output
- Automatic Java 17 downloading option
- Automated Android SDK tools installation
- APK signing and building process
- Userscripts support

## Quick Start

1. Clone this repository:
```bash
git clone https://github.com/dhitchenor/disrootwebchat-android
cd disrootwebchat-android
```

2. Generate signing key (only needed once, keep the generated file safe):
```bash
./make.sh keygen
```

3. Apply configuration and build:
```bash
./make.sh build
```

The final APK will be created in the app/build directory.


## Available Commands

- `./make.sh build` - Apply configuration and build
- `./make.sh keygen` - Generate signing key
-
- `./make.sh apk` - Build APK without apply_config
- `./make.sh apply_config` - Apply settings from configuration file
- `./make.sh get_java` - Download OpenJDK 17 locally

## App Links / Deep Links

You can make your app handle links to the website by setting the `deeplink` option in your configuration file. When set, clicking links to your website on the device will open them in your app instead of a browser.

For example, if your website is `https://example.com`, set:
```ini
deeplink = example.com
```

## Userscripts Support

The app supports userscripts (similar to Tampermonkey/Violentmonkey scripts) through the `scripts` configuration option:

```ini
scripts = scripts/*.js             # Load all .js files from scripts directory
# OR
scripts = site-*.js                # Load all files matching pattern
# OR
scripts = script1*.js script20.js  # Load specific script files
```

### How Userscripts Work

- Scripts can use Tampermonkey/Violentmonkey/etc [`@match`](https://violentmonkey.github.io/api/metadata-block/#match--exclude-match) and [`@run-at`](https://violentmonkey.github.io/api/metadata-block/#run-at) directives, other ignored
- If no `@match` is specified, the script will run on all pages
- Only `GM_addStyle` supported from Greasemonkey API
- There are `toast("short message")` function
- Script console output (console.log/alert/warn) can be monitored using:
```bash
./make.sh test
```

Common use cases include:
- Adding dark mode to websites
- Customizing website appearance
- Adding new functionality
- Fixing mobile compatibility issues

Example of some useful scripts:
- [dark-mode.js](https://gist.github.com/Jipok/01d12591491816625649a467db898518) - Universal dark theme that respects system preferences
- [instant.js](https://raw.githubusercontent.com/instantpage/instant.page/refs/heads/master/instantpage.js) - Speed up page loads by preloading pages when the user taps

## Additional WebView Options
The following advanced options can also be configured:
```ini
jksFile = my-release-key.jks          # Signing file for the android app
jksAlias = my                         # Signing alias for the android app
jksStorepass = 123456                 # Password for the Signing File Store
jksKeypass = 123456                   # Password for the Individual Key

cookies = "key1=value1; key2=value2"  # Cookies for mainURL host
basicAuth = login:password            # HTTP Basic Auth credentials for mainURL host
userAgent = "MyCustomUserAgent/1.0"   # Custom UserAgent header
JSEnabled = true                      # Enable JavaScript execution
JSCanOpenWindowsAutomatically = true  # Allow JS to open new windows/popups

DomStorageEnabled = true              # Enable HTML5 DOM storage
DatabaseEnabled = true                # Enable HTML5 Web SQL Database
SavePassword = true                   # Allow saving passwords in WebView
AllowFileAccess = true
AllowFileAccessFromFileURLs = true
forceLandscapeMode = false            # Lock screen orientation to landscape

showDetailsOnErrorScreen = false      # Show connection error details for user
confirmOpenExternalApp = true         # Show confirmation before opening external app
blockLocalhostRequests = true         # Block requests to 127.0.0.1
trustUserCA = false                   # Allow app to trust user-installed SSL certs
geolocationEnabled     = false        # Block geolocation API
```

## Technical Details

- Target Android API: 33 (Android 13)
- Minimum Android API: 24 (Android 7.0)
- Build tools version: 33.0.2
- Gradle version: 7.4
- Required Java version: 17

## Notes

- All app data is stored in the app's private directory
- Internet permission is required and automatically included
- If you need to support [different Android versions](https://apilevels.com/), edit `app/build.gradle` accordingly
- Based on the original work from: https://github.com/successtar/web-to-app

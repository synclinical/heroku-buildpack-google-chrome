# heroku-buildpack-google-chrome

This buildpack downloads and installs (headless) Google Chrome.

### Using a specific Chrome major version

If you need to use a specific major version of Chrome (for example, if a newer version is causing compatibility issues), you can set the `GOOGLE_CHROME_MAJOR_VERSION` config variable.

Set `GOOGLE_CHROME_MAJOR_VERSION` to the desired major version number, such as:

- `127` for Chrome 127.x
- `126` for Chrome 126.x

When a specific major version is requested:

1. The buildpack queries the Chrome Version History API to find the latest minor version within the specified major version.
2. It then constructs a URL to download the specific `.deb` package for that version from Google's servers.

For example, if you set `GOOGLE_CHROME_MAJOR_VERSION=127`, the buildpack might install version 127.0.6350.0 (the latest 127.x version at the time of the build).

If `GOOGLE_CHROME_MAJOR_VERSION` is not set, the buildpack will use the latest stable version by default, downloading it from Google's direct download link for the current stable release.

## Channels

Alternatively, you can choose your release channel by specifying `GOOGLE_CHROME_CHANNEL` as
a config var for your app, in your app.json (for Heroku CI and Review Apps),
or in your pipeline settings (for Heroku CI).

Valid values are `stable`, `beta`, and `unstable`. If unspecified, the `stable`
channel will be used.

## Shims and Command Line Flags

This buildpack installs shims that always add `--headless`, `--disable-gpu`,
`--no-sandbox`, and `--remote-debugging-port=9222` to any `google-chrome`
command as you'll have trouble running Chrome on a Heroku dyno otherwise.

You'll have two of these shims on your path: `google-chrome` and
`google-chrome-$GOOGLE_CHROME_CHANNEL`. They both point to the binary of
the selected channel.

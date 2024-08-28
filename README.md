# heroku-buildpack-google-chrome

This buildpack downloads and installs (headless) Google Chrome from your choice
of release channels.

## Using a Specific Chrome Major Version

If you need to use a specific major version of Chrome (for example, if a newer version is causing compatibility issues), you can set the `GOOGLE_CHROME_MAJOR_VERSION` config variable.

Set `GOOGLE_CHROME_MAJOR_VERSION` to the desired major version number, such as:

- `127` for Chrome 127.x
- `126` for Chrome 126.x

During build, this version number is used to fetch the latest minor version within the specified major version from the Chrome for Testing API [`latest-versions-per-milestone.json`](https://googlechromelabs.github.io/chrome-for-testing/latest-versions-per-milestone.json).

If `GOOGLE_CHROME_MAJOR_VERSION` is not set, the buildpack will use the latest stable version by default.

## Channels

You can choose your release channel by specifying `GOOGLE_CHROME_CHANNEL` as
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

# One-Tap Bird Logger

A single-page web app for logging bird sightings while traveling and exporting them as eBird checklists. Each tap records one sighting, its time, and the device's latest GPS coordinates when available.

## Running the app

Open `index.html` in a browser, or serve it as a static page (for example, with GitHub Pages). No installation, dependencies, or build process are required.

GPS requires location permission and generally works only over HTTPS or `localhost`. To run a local server:

```sh
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

## Controls

- **Enable GPS**: start watching the device's location. The display shows the accuracy of the latest fix.
- **Species buttons**: tap once to record one observation. Long-press to remove the most recent observation of that species.
- **Voice**: say a species name to record it, if the browser supports speech recognition. Approximate matching is used against the configured species list.
- **Add / Settings**: add or edit species buttons or reset the default species list.
- **Undo**: remove the most recent observation.
- **Clear**: delete saved observations after confirmation.
- **Export CSV**: download observations in eBird Record Format.
- **Copy CSV**: copy the same eBird-formatted CSV to the clipboard.

The observation table shows time, species, latitude, longitude, GPS accuracy, and input method. Species counters reflect observations recorded during the current session.

## eBird CSV

Both CSV controls produce **eBird Record Format**, with 19 columns and no header row. Each observation with valid GPS coordinates is exported as a separate **Incidental** checklist using a unique location name, its latitude and longitude, the observed date and time, and a count of `1`. The “all species reported” field is `N`.

Observations without valid GPS coordinates remain in the app but cannot be exported. If some are missing coordinates, the app asks for confirmation before exporting the remaining observations. If none have coordinates, export is unavailable.

To import the CSV into eBird, go to **Submit → Import Data** and select **Record Format**. eBird may prompt you to check species names and locations during the import. CSV uploads are limited to 1 MB.

This app always assigns the **Incidental** protocol, which is intended for situations where birding was not the main purpose of the activity. eBird's protocol guidance: https://support.ebird.org/en/support/solutions/articles/48000950859

eBird's file format and import instructions: https://support.ebird.org/en/support/solutions/articles/48000907878

## Data and limitations

- Observations and species settings are stored in the browser's `localStorage`; there is no account, synchronization, or server-side storage.
- A recorded GPS position is the device's last available fix and may be missing or outdated. The position represents the device, not necessarily the bird.
- Each tap is one observation with a count of one. The app does not group multiple taps into a single count.
- New observations save the device's local date and time. Older logs without these values use the exporting browser's timezone.
- Speech recognition may be unavailable or may incorrectly match a name.
- Species counters reset on page reload, even though the observations remain saved.
- On some touch devices, a long press may also trigger a regular tap.
- The CSV export does not submit anything to eBird automatically and has not been tested against a live eBird import.

For safety, the app should be operated by a passenger or while stopped, not by someone actively driving.

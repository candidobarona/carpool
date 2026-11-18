# 🦁 Carpool Map

A zero-setup, single-file carpool planner for match days. Create an event, share the link, and everyone can see cars on a live map, add their own car, and grab seats in real time.

No accounts, no API keys, no database. It's one `index.html` file.

## Features

- **Create an event** with a match name, destination (address autocomplete), and date/time.
- **Live map** (Leaflet + OpenStreetMap) showing the destination and every driver's departure point. Click a driver's pin to highlight their card.
- **Car cards** with big seat slots. Tap `+ Join` to take a seat, `Leave` to free it.
- **Add Car** form with driver name, departure address, and seat count (1–7).
- **Edit / Delete Event** to change the match name, destination, or time, or delete the event for everyone.
- **Light / Dark mode** toggle (🌙 / ☀️), remembered on your device.
- **Real-time sync** for everyone with the link, with local caching so the page loads instantly and works offline with the last known state.

## How it works

- Each event has a random ID, passed in the URL as `?event=<ID>`.
- Every action (create, add car, join, leave, edit, delete) is published as a small JSON message to a public [ntfy.sh](https://ntfy.sh) topic named `carpool_v1_<ID>`.
- Every browser subscribes to that topic and replays the actions in order to compute the same live state.
- Map data comes from OpenStreetMap, and address search from Nominatim.

Because the topic name is the event ID, **anyone with the link can view and change the event.** Treat the link like a shared document.

## Deploy to GitHub Pages

1. Create a new public repository on GitHub (for example, `carpool`).
2. Upload `index.html` and `README.md` to the root of the repository.
3. Go to **Settings → Pages**.
4. Under **Build and deployment**, set **Source** to *Deploy from a branch*, choose `main` and `/ (root)`, then click **Save**.
5. After a minute or two, your app is live at `https://<your-username>.github.io/<repo-name>/`.

## Usage

1. Open the site and fill in the match name, destination, and date/time, then click **Create Event**.
2. Copy the link from the **Copy Link** button and send it to your group.
3. Anyone with the link can add a car (driver name, departure address, seats) or join a seat.
4. Use **Edit Event** to update details or **Delete Event** to remove the event for everyone.

## Notes and limitations

- Requires an internet connection for live sync and map tiles.
- The ntfy.sh public service is free but has rate limits. Very heavy use may cause temporary delays.
- Passenger names are entered via a browser prompt and are not verified.
- Actions cannot be undone, except by re-editing or removing a car or seat yourself.
- Event data lives on ntfy.sh's public message cache, so don't put sensitive personal information in the fields.

## Tech stack

- React 18 and Babel Standalone (via CDN)
- Tailwind CSS (via CDN)
- Leaflet 1.9.4 (via CDN)
- ntfy.sh for real-time pub/sub
- OpenStreetMap and Nominatim for maps and geocoding

## License

Free to use and modify.

# BAYMAX MAX Desktop

A browser-based desktop command-centre foundation the standalone desktop application. Existing DOOM HAND, HEAD, BODY, shared protocol, and ESP32-CAM firmware are not modified.

## Start

Open `index.html` in a desktop browser. For best results, use a local static server from this directory:

```sh
python3 -m http.server 8080
```

Then open `http://localhost:8080`.

## Camera connection

The camera panel defaults to `http://192.168.4.1`, matching the ESP32-CAM access point documented in the camera repository. Connect the computer to the camera's `DOOM_CAM` Wi-Fi network (AP credentials are configured in the camera firmware), then press **CONNECT**. If the camera uses a different address, edit CAMERA BASE URL.

This client uses the existing camera endpoints without changing their behavior:
- `GET /capture` — JPEG frame, refreshed periodically for a live-preview effect.
- `GET /status` — optional status check; browser CORS restrictions may prevent reading its response even when image preview works.
- `GET /photo` — opens the existing save-photo endpoint.
- `GET /flash?state=1` or `state=0` — requests flash state.

The camera firmware's current API uses individual JPEG frames rather than a continuous MJPEG stream, so the preview refreshes frames periodically. Camera and computer must be able to reach one another over Wi-Fi. Some browser/network combinations may restrict requests across origins; if so, the next step is a small local bridge/proxy, not changing the existing firmware blindly.

## What's real vs planned

**Implemented in this first desktop phase**
- Dark orange/amber BAYMAX MAX command-centre layout.
- Configurable ESP32-CAM base URL and periodically refreshed real `/capture` preview.
- Connection/offline UI states and a local activity log.
- Photo and flash endpoint shortcuts.
- Explicit unavailable states for hardware telemetry, GPS, vision, and identity memory.

**Not implemented yet**
- Offline person detection, multi-object tracking, annotated boxes, or tracking IDs.
- Identity recognition or profile database.
- HEAD/BODY/DOOM HAND telemetry bridge and robot command controls.
- GPS, assistant, or voice modules.

No simulated detection counts, fake sensor values, or fabricated identity matches are shown.

## Next integration step

Add a separate local Python service on the desktop to receive camera frames, run an offline detector/tracker, and expose actual results to this UI. Keep this service separate from the ESP32 firmware. Start with person detection and temporary tracking IDs; add voluntary identity enrollment only after the detection/tracking path is validated. GPS can later be added as another optional data provider.

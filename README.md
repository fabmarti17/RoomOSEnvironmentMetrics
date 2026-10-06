# EnvironmentMetrics – RoomOS Macro

A Cisco RoomOS macro that creates a **Room Environment** panel displaying real-time room data: presence, people count, temperature, humidity, air quality and ambient noise.

## Features

- Panel automatically created by the macro (no manual UI Extension import)
- Button color reflects room occupancy:
  - 🔴 Red: room occupied
  - 🟢 Green: room free
  - No color: presence unknown
- Color-coded thresholds per metric (🟢 OK / 🟡 Warning / 🔴 Alert / ⚪ N/A)
- On-screen notifications with anti-spam logic
- Real-time updates (presence, people count, noise) **only while the panel is open**
- Background refresh every 30 s when the panel is closed
- Automatic sensor source detection: local device or Room Navigator

## Supported devices

| Device | Temp / Humidity / Air Quality source | Presence / Count / Noise source |
|---|---|---|
| Desk, Desk Pro, Board Pro | Device | Device |
| Room Bar, Room Bar Pro, Room Kit EQ, Room Kit Pro G2 | Room Navigator (Controller mode) | Codec |

Tested on RoomOS 11 and RoomOS 26 (CE26.7).

## Installation

1. Open the device web interface → **Macro Editor**.
2. Create a new macro named `EnvironmentMetrics`.
3. Paste the content of `macro/EnvironmentMetrics.js`.
4. **Save** and **Enable**.
5. The **Room Environment** button appears on the home screen.

## Configuration

All settings are at the top of the macro:

| Constant | Purpose |
|---|---|
| `THRESHOLDS` | Warning / alert levels for temperature, humidity, air quality and noise |
| `COLOR_OCCUPIED` / `COLOR_FREE` / `COLOR_DEFAULT` | Button colors by occupancy |
| `PANEL_ID` / `PAGE_ID` | UI Extension identifiers |

## Device configurations applied by the macro

- `RoomAnalytics PeopleCountOutOfCall: On`
- `RoomAnalytics PeoplePresenceDetector: On`
- `RoomAnalytics AmbientNoiseEstimation Mode: On`

## Known limitations

- `RoomAnalytics ReportFrequency` does not exist on CE26.7. Real-time behavior relies on status subscriptions instead.
- Button color changes rebuild the panel (`Panel.Save`), which may close it if occupancy changes while it is open.
- **Microsoft Teams Rooms (MTR):** the `HomeScreenAndCallControls` location is not visible in MTR. Use `ControlPanel` to access the button from the RoomOS menu.

## Troubleshooting

| Console message | Cause | Fix |
|---|---|---|
| `Could not find module 'jsapi'` | Wrong import | Use `import xapi from 'xapi';` |
| `Path argument was ambiguous` | String path with spaces/dots | Use direct access: `xapi.Config.RoomAnalytics.X.set()` |
| `Invalid or missing Path argument` | Setting not available on this build | Check with `xConfiguration RoomAnalytics` |
| Temp / humidity / air show N/A on Room Series | Sensors are on the Navigator | Check `xStatus Peripherals ConnectedDevice` |

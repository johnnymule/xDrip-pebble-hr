# xDrip+HR Pebble Watchface Project

## Overview
Modified xDrip Pebble watchface for Pebble 2 HR that adds heart rate display and data persistence.

## Hardware
- **Watch:** Pebble 2 HR (has heart rate sensor)
- **Platform:** Pebble SDK 3.x
- **Target:** xDrip app integration

## Project Status

### ✅ Completed
- Base watchface functional (glucose, delta, time, battery)
- `health` capability enabled in appinfo.json
- Build system working (wscript, multi-platform builds)
- **Heart rate display** - Heart icon (PNG) + number in middle row between CGM time and app status
- **Heart icon** - 16x16 PNG, pure black on white, 8-bit RGB format matching other assets
- **Heart icon layer** - GRect(45, 63, 18, 16) - no vertical padding, aligned with other status icons
- **Data persistence** - Uses Pebble persistent storage to cache BG, delta, CGM time, app time, icon, and battery level. Data survives menu trips.
- Health service integration - subscribes to HR updates
- GitHub release v1.4-hr published with updated README and PBW

### 🔄 In Progress / TODO
1. **Testing** - Needs to be tested on actual Pebble 2 HR hardware

2. **Send HR to xDrip** (optional enhancement)
   - Add new appKey for HR data in appinfo.json
   - Send HR in `send_cmd_cgm()` or separate timer
   - xDrip Android app side needs to receive and process HR data

## Changes Made (v1.4-hr)
- **Heart icon fixed** - Converted to pure black/white 8-bit RGB format
  - 16x16 pixel image centered in 18x16 layer
  - Layer position: GRect(45, 63, 18, 16) - y=63 matches CGM time and app status
  - 2px gap between layer end (63) and HR numerals start (65)
- **README updated** - Added changelog, moved download section, corrected coordinates

## Changes Made (v1.7)
- **Heart icon** - Replaced Unicode heart (showed as square) with PNG heart bitmap
  - Added `IMAGE_HEART` resource
  - Heart icon at x=45, HR number at x=65
- **Fixed persistence loading** - Moved persist reads BEFORE `app_sync_init` so cached data is used as initial values instead of "LOAD"

## Changes Made (v1.6)
- Added `hr_layer` TextLayer for heart rate display
- Added `load_heart_rate()` function using `health_service_peek_current_value(HealthMetricHeartRateBPM)`
- Added `health_handler()` callback for HR updates
- Added persistent storage keys: PERSIST_BG_KEY, PERSIST_DELTA_KEY, PERSIST_CGM_TIME_KEY, PERSIST_APP_TIME_KEY, PERSIST_ICON_KEY, PERSIST_BAT_KEY
- Modified `window_load_cgm()` to load persisted data after AppSync init
- Modified `sync_tuple_changed_callback_cgm()` to save data to persistent storage
- Added health service subscribe/unsubscribe in init/deinit

## UI Layout (Pebble 144x168 screen)
```
y=-5:  BG number (large, left) + arrow icon (right)
y=33:  Delta message (center)
y=58:  [CGM time] [HR text] [App status]  <- HR numerals at x=65
       ^ Heart icon at x=45 (y=63, 18x16 layer, 16x16 image)
y=82:  Watch time (large, full width bar)
y=120: Date
y=148: Phone batt (left) + Watch batt (right)
```

## Coordinate Reference (Current)

| Element | Position | Size | Notes |
|---------|----------|------|-------|
| CGM time | (5, 58) | 40x24 | Left status area |
| Heart icon | (45, 63) | 18x16 layer | 16x16 image centered, no vertical padding |
| HR number | (65, 58) | 36x24 | Right of heart icon, 2px gap from layer end |
| App status | (77, 58) | 40x24 | Right status area |

## Files
- `src/xdrip.c` - Main watchface code
- `appinfo.json` - App config (already has health capability)
- `resources/images/heart.png` - Heart icon (16x16, 8-bit RGB, black on white)
- `wscript` - Build config

## Build Commands
```bash
cd /home/claw/.openclaw/workspace/xdrip-hr
pebble build                    # Build all platforms
pebble install --emulator basalt # Test in emulator
pebble install --phone <ip>      # Install to watch
```

## Release Process
1. `pebble build` - Generate build/xdrip-hr.pbw
2. Commit changes to master
3. Delete and recreate tag `v1.4-hr` pointing to latest commit
4. Update release notes on GitHub
5. Upload PBW as `xDrip-pebble-hr.pbw`

## Pebble Health API Reference
```c
#include <pebble.h>

// Check if HR available
HealthValue hr = health_service_peek_current_value(HealthMetricHeartRateBPM);
if (hr > 0) {
    // Valid HR reading
}

// Subscribe to HR updates (optional)
health_service_events_subscribe(health_handler, NULL);

static void health_handler(HealthEventType event, void *context) {
    if (event == HealthEventHeartRateUpdate) {
        // HR changed
    }
}
```

## Persistent Storage API
```c
// Write
persist_write_string(PERSIST_BG_KEY, last_bg);
persist_write_string(PERSIST_DELTA_KEY, current_bg_delta);
persist_write_int(PERSIST_CGM_TIME_KEY, current_cgm_time);

// Read  
if (persist_exists(PERSIST_BG_KEY)) {
    persist_read_string(PERSIST_BG_KEY, last_bg, sizeof(last_bg));
}
```

## Last Updated
2026-02-23 - Heart icon fixed (8-bit RGB format, layer adjusted), README updated, v1.4-hr released.

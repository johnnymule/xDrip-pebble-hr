# ❤️ xDrip+HR Pebble Watchface

A modified xDrip watchface for Pebble smartwatches that adds **heart rate display** and **persistent glucose data caching**—so your readings survive menu trips without showing "LOADING..."

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 💓 **Heart Rate Display** | Live BPM from Pebble's optical HR sensor, shown with a cute heart icon |
| 💾 **Data Persistence** | Last glucose reading, delta, and timestamp are cached—no more "LOADING" when you return from the menu |
| 📊 **Classic xDrip UI** | Same layout you know: BG value, trend arrow, delta, time ago, battery levels |

---

## ⌚ Compatible Devices

| Model | Heart Rate | Notes |
|-------|------------|-------|
| **Pebble 2 HR** | ✅ Yes | Full support |
| Pebble 2 (no HR) | ❌ No | Works as regular xDrip watchface, no HR display |
| Pebble Time / Time Steel / Time Round | ❌ No | No HR sensor |
| Pebble Classic / Steel | ❌ No | No HR sensor |

---

## 📥 Download

**Latest release: [xDrip-pebble-hr.pbw](../../releases/latest/download/xdrip-pebble-hr.pbw)**

Or grab it from the [Releases](../../releases) page.

---

## 🔧 Building from Source

### Prerequisites
- [Pebble SDK](https://developer.rebble.io/developer.pebble.com/sdk/index.html)
- Pebble 2 HR (for HR features)
- xDrip+ app on Android

### Build
```bash
git clone https://github.com/johnnymule/xDrip-pebble-hr.git
cd xDrip-pebble-hr
pebble build
```

### Install
```bash
# Via IP (Pebble app → Developer → IP address)
pebble install --phone 192.168.1.xxx

# Or copy build/xdrip-pebble-hr.pbw to your phone
```

---

## 🛠️ For Developers

### Key Files
| File | Purpose |
|------|---------|
| `src/xdrip.c` | Main watchface code (~2200 lines) |
| `appinfo.json` | App config, resources, capabilities |
| `wscript` | Build configuration |
| `resources/images/` | All the PNG assets |

### Important Variables

**HR-related:**
```c
TextLayer *hr_layer;           // The text showing "72"
BitmapLayer *heart_icon_layer; // The heart icon
GBitmap *heart_bitmap;         // The actual heart PNG
static char hr_text[12];       // Buffer for HR string
```

**Persistence keys (stored in flash):**
```c
#define PERSIST_BG_KEY 1        // Last glucose value
#define PERSIST_DELTA_KEY 2     // Delta string
#define PERSIST_CGM_TIME_KEY 3  // Timestamp of last reading
#define PERSIST_APP_TIME_KEY 4  // App timestamp
#define PERSIST_ICON_KEY 5      // Trend arrow icon
#define PERSIST_BAT_KEY 6       // Phone battery level
```

**Health API usage:**
```c
// Read current HR
HealthValue hr = health_service_peek_current_value(HealthMetricHeartRateBPM);

// Subscribe to updates
health_service_events_subscribe(health_handler, NULL);
```

### Coordinate Reference (Pebble 144x168)

| Element | Position | Size |
|---------|----------|------|
| BG value | (0, -5) | 95x47 |
| Trend arrow | (85, -7) | 78x50 |
| Delta | (0, 33) | 144x50 |
| CGM time | (5, 58) | 40x24 |
| **Heart icon** | **(45, 60)** | **20x20** |
| **HR number** | **(65, 58)** | **24x24** |
| App status | (77, 58) | 40x24 |
| Watch time | (0, 82) | 144x44 |
| Date | (0, 120) | 144x29 |
| Battery | (0, 148) | varies |

### Modifying the Layout

The HR display is created in `window_load_cgm()`:
```c
// Heart icon first (left)
heart_icon_layer = bitmap_layer_create(GRect(45, 60, 20, 20));

// Then HR number (right of icon)
hr_layer = text_layer_create(GRect(65, 58, 24, 24));
```

Adjust these `GRect` values to move things around. Just keep the total width within ~32px or you'll overlap the app status.

---

## 🙏 Credits

- **Original xDrip-pebble** by [jstevensog](https://github.com/jstevensog/xDrip-pebble)
- **cgm-pebble** (Nightscout upstream) by the Nightscout community
- **Heart icon** from [Twemoji](https://github.com/twitter/twemoji) (CC-BY 4.0)

---

**Version:** 1.4-hr (based on xDrip-pebble 1.4)

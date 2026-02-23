# xDrip+HR Pebble Watchface - Project Summary

## Project Objective
Modify the xDrip Pebble watchface to add:
1. **Heart rate display** with icon (for Pebble 2 HR)
2. **Persistent glucose data caching** - survives menu trips without showing "LOADING"

## Current Status
- ✅ Heart rate display implemented (text shows BPM)
- ✅ Data persistence working (caches BG, delta, times, icon, battery)
- ✅ Heart icon fixed - now pure black/white 8-bit RGB format, layer adjusted to 18x16
- ✅ GitHub release v1.4-hr published with updated README

## GitHub Repository
**URL:** https://github.com/johnnymule/xDrip-pebble-hr
**Release:** https://github.com/johnnymule/xDrip-pebble-hr/releases/tag/v1.4-hr

## Credentials
**GitHub Token:** See local `TOOLS.md` in workspace root (DO NOT commit tokens)

## Key Technical Details

### File Locations
- **Main code:** `src/xdrip.c`
- **App config:** `appinfo.json`
- **Heart icon:** `resources/images/heart.png` (16x16, 8-bit RGB, pure black on white)
- **Build output:** `build/xdrip-hr.pbw`

### Heart Icon Final Specs
- **Size:** 16x16 pixels (image), 18x16 pixels (layer)
- **Format:** 8-bit RGB PNG (matches other project PNGs like phoneon.png)
- **Colors:** Pure black (#000000) heart on white background
- **Layer position:** GRect(45, 63, 18, 16) - y=63 matches other status icons
- **Gap to HR numerals:** 2px layer-to-layer, 3px visible heart-to-numerals

### Version Strategy
- Fork versioning: `1.4-hr` (based on original xDrip-pebble 1.4)

### Build Process
```bash
cd /home/claw/.openclaw/workspace/xdrip-hr
pebble build
# Output: build/xdrip-hr.pbw
```

### Release Process
1. Commit changes to master
2. Delete and recreate tag `v1.4-hr` pointing to latest commit
3. Update release notes and ensure PBW is uploaded as `xDrip-pebble-hr.pbw`

## Completed Changes (v1.4-hr - 2025-02-22)
1. ✅ Converted heart icon to pure black/white 8-bit RGB format
2. ✅ Adjusted heart icon layer from 18x18 to 18x16 (removed vertical padding)
3. ✅ Positioned at y=63 to align with other status icons
4. ✅ Updated README with correct coordinates, changelog, and download section placement
5. ✅ PBW renamed to `xDrip-pebble-hr.pbw` to match README

## Next Steps
1. Test on actual Pebble 2 HR hardware
2. Verify data persistence works across menu trips
3. User to investigate GadgetBridge mention claim

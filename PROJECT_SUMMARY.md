# xDrip+HR Pebble Watchface - Project Summary

## Project Objective
Modify the xDrip Pebble watchface to add:
1. **Heart rate display** with icon (for Pebble 2 HR)
2. **Persistent glucose data caching** - survives menu trips without showing "LOADING"

## Current Status
- ✅ Heart rate display implemented (text shows BPM)
- ✅ Data persistence working (caches BG, delta, times, icon, battery)
- ❌ Heart icon needs fixing - currently showing in color, needs to be black/white for Pebble
- ⏳ GitHub release process working

## GitHub Repository
**URL:** https://github.com/johnnymule/xDrip-pebble-hr
**Release:** https://github.com/johnnymule/xDrip-pebble-hr/releases/tag/v1.4-hr

## Credentials
**GitHub Token:** See local `TOOLS.md` in workspace root (DO NOT commit tokens)

## Key Technical Details

### File Locations
- **Main code:** `src/xdrip.c`
- **App config:** `appinfo.json`
- **Heart icon:** `resources/images/heart.png` (NEEDS FIX)
- **Build output:** `build/xdrip-hr.pbw`

### Heart Icon Requirements
- **Size:** 18x18 pixels
- **Format:** 1-bit or 8-bit grayscale PNG (NOT color/indexed)
- **Colors:** Black heart on transparent/white background
- **Current issue:** Using Twemoji which is color - needs proper B&W conversion

### Version Strategy
- Fork versioning: `1.4-hr` (based on original xDrip-pebble 1.4)

### Build Process
```bash
cd /home/claw/.openclaw/workspace/xdrip-hr
pebble build
cp build/xdrip-hr.pbw ~/Desktop/
```

### Release Process
1. Commit changes to master
2. Delete and recreate tag `v1.4-hr` pointing to latest commit
3. Delete old release, create new one from tag
4. Upload PBW to release

## Outstanding Issues
1. **Heart icon is color** - needs to be converted to black/white grayscale
2. Heart position may need adjustment after fixing colors

## Next Steps for New Session
1. Fix heart icon colors (convert Twemoji to proper Pebble format)
2. Test on actual Pebble 2 HR
3. Verify data persistence works across menu trips
4. Final release

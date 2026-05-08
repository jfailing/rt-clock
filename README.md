# RT Clock

A real-time scheduling clock application for power grid operations, built as a modern web-based alternative to legacy Excel/PI Datalink solutions. Displays deadline countdowns, PI tag monitoring, and escalating alarms for critical operational windows.

## Overview

RT Clock visualizes a one-hour operational cycle divided into seven critical time windows (T-75, T-57, T-55, T-40, plus ancillary intervals). Each window triggers specific operational tasks with visual indicators, audio alerts, and optional speech notifications. The clock pulls live data from PI (OSIsoft's Process Information) systems to monitor asset imbalances and submission deadlines.

**Current Status:** v2.0 (Production-ready, mock data mode)  
**Next Release:** v2.1/v3.0 (Live PI Web API integration)

## Features

### Main Display (rt-clock.html)
- **Six-ring analog clock face** with color-coded deadline zones
- **Live digital readout** (date and time)
- **Real-time hand movement** synchronized to system clock
- **Escalating alarm system** — sound/speech intensity increases as deadline approaches
- **PI tag monitoring** — displays live imbalance values (T-40, T-55)
- **Indicator shapes** — visual feedback for alarm states
- **Pause and mute controls** — for testing and flexibility
- **Fully responsive** — scales to any screen size with crisp text on all displays

### Configuration Interface (rt-clock-config.html)
- **Ring editor** — create/modify deadline rings with custom colors, thresholds, and labels
- **Alarm configuration** — set time windows, enable/disable escalation, configure speech
- **Dial customization** — 9 texture options (guilloché, sunburst, Nemo, Goldeneye, etc.)
- **Font selection** — Kalinga for clean, modern typography
- **Hand shape picker** — 6 styles (Baton, Triangle, Sword, Skeleton, Submariner, Black Bay)
- **Live preview** — see changes in real-time
- **Local storage** — settings persist between sessions
- **Mock data mode** — test without PI server access

## Visual Design

The clock uses a **concentric ring architecture**:
- **Outer rings:** Color-coded deadline periods (T-75, T-57, T-55, T-40)
- **Face:** Segmented quadrants with optional textures
- **Center:** Rotating hour/minute/second hands
- **Top indicator:** Dynamic alerts and status messages

**Color Scheme:**
- Deep blue (#1A3A6A, #1E50A8) — T-75, T-77
- Forest green (#245020, #367A28) — T-55, T-57
- Purple (#3A1E5C, #58309A) — T-40, T-42
- Gold (#F4BA0C) — Status indicators

## Getting Started

### Quick Start (Local Testing)

1. **Download both files** from this repository:
   - `rt-clock.html` (main display)
   - `rt-clock-config.html` (configuration)

2. **Save to the same folder** on your computer

3. **Open in your browser:**
   - Configuration: Right-click `rt-clock-config.html` → Open with → Your browser
   - Display: Right-click `rt-clock.html` → Open with → Your browser

4. **Configure:**
   - In the config page, adjust rings, alarms, and appearance
   - Settings auto-save to your browser
   - Click the "⚙ Config" button in the main clock to toggle settings

5. **Test audio:**
   - Click "Test Audio" in the config page to verify sound output
   - Enable/disable speech and alarm sounds as needed

### Browser Compatibility

- Chrome 90+ (recommended)
- Firefox 88+
- Safari 14+
- Edge 90+

Requires JavaScript enabled. No server or external dependencies.

## Architecture

### Technology Stack
- **Pure HTML5/CSS3/JavaScript** — no frameworks or build tools
- **Canvas API** — crisp rendering at any DPI (device pixel ratio aware)
- **Web Audio API** — procedural alarm tones
- **Web Speech API** — text-to-speech notifications
- **localStorage** — persistent configuration

### Key Components

**rt-clock.html:**
- Clock rendering engine (canvas-based)
- Real-time hand animation
- Alarm evaluation logic
- Audio/speech synthesis
- Digital readout display

**rt-clock-config.html:**
- Interactive ring editor
- Shape and color pickers
- Texture selector
- Mock PI data simulator
- Settings persistence

## Configuration Reference

### Rings

Each ring represents a deadline period. Configure:
- **Label** — display name (e.g., "T-75")
- **Type** — `interval` or `deadline`
- **Start/End Minutes** — time window within the hour
- **Color** — visual appearance
- **Thickness** — ring width
- **Caret** — optional marker at deadline moment
- **Label style** — font size, color, curved or straight

### Alarms

Alarms trigger when certain conditions are met:
- **Time window** — only active during specified minutes
- **PI condition** — optional tag threshold
- **Escalating** — increase alarm frequency as deadline nears
- **Speech** — optional announcement
- **Enable/disable** — toggle individual alarms

### Dial Textures

Nine built-in textures for visual variety:
- `none` — plain color
- `guilloché` — fine radial lines
- `sunburst` — burst pattern
- `fumé` — radial fade
- `sector` — pie slices
- `grand-seiko` — fine vertical lines
- `regal-oak` — checkerboard (Patek Aquanaut style)
- `nemo` — horizontal emboss (Nautilus style)
- `goldeneye` — wave pattern (Seamaster style)

### Hand Shapes

Six hand styles for different operational aesthetics:
- `baton` — simple rectangular
- `triangle` — pointed tip
- `sword` — tapered
- `skeleton` — outline only
- `submariner` — disc and needle
- `twodoor` — Black Bay style (disc with shoulder step)

## Data Integration

### Current (v2.0)

**Mock Mode:**
- Simulates PI tag values
- No server required
- Perfect for testing and training

**Configuration:**
- Edit mock values in the config page
- Test alarm logic without live PI

### Coming (v2.1/v3.0)

**PI Web API Integration:**
- Connect directly to OSIsoft PI servers
- Real-time tag polling (configurable rate)
- Support for PI tags and AF (Asset Framework) attributes
- Multiple simultaneous tag monitoring
- Automatic value caching

**Alarm Architecture:**
Four alarm classes:
1. **Time window only** — fires/silences based on clock position
2. **Time + PI gate** — suppressed if PI condition met mid-window
3. **Time + PI trigger** — fires only if PI condition true
4. **PI-only** — triggers whenever PI threshold crossed

**Expression Engine:**
- Simple math: `TAG_A < 5`, `TAG_B > 10`
- Comparisons: `<`, `>`, `==`, `!=`, `<=`, `>=`
- Logical: `AND`, `OR`
- Example: `EIM_IMBALANCE < 5 AND T40_SUBMITTED == true`

## File Structure

```
rt-clock/
├── rt-clock.html              # Main clock display
├── rt-clock-config.html       # Configuration interface
└── README.md                  # This file
```

Both files are self-contained and can be deployed anywhere — no build process, no dependencies.

## Usage Notes

### For Operations Teams
- Open `rt-clock.html` on a wall monitor or control room display
- Keep `rt-clock-config.html` on a separate device for adjustments
- Enable audio and speech during live operations
- Use "Pause" checkbox to temporarily silence alarms during training

### For IT/Developers
- Settings stored in browser localStorage (no database needed)
- All code is human-readable and heavily commented
- Modify colors, fonts, and timing in the config interface
- No server required for v2.0; API integration planned for v2.1+

### Performance
- Lightweight — runs smoothly on any modern device
- Canvas rendering optimized with DPR awareness
- Audio synthesis on-demand (no prerecorded files except system sounds)
- ~50KB total size (both HTML files combined)

## Roadmap

### v2.1 (In Development)
- [ ] PI Variables definition UI
- [ ] Alarm class selector (Classes 1-4)
- [ ] Expression evaluator for complex conditions
- [ ] Configurable poll rates (5-60 seconds)
- [ ] Confirmation chimes for alarm resolution

### v3.0 (Planned)
- [ ] Live PI Web API integration
- [ ] AF (Asset Framework) path support
- [ ] Multi-server support
- [ ] Historical alarm logging
- [ ] Advanced analytics dashboard
- [ ] Mobile app (React Native)

### v3.1+ (Future)
- [ ] Machine learning for anomaly detection
- [ ] Predictive deadline warnings
- [ ] Integration with SCADA systems
- [ ] Custom alarm workflows
- [ ] Slack/email notifications

## Known Limitations

### v2.0
- Mock data only (no live PI yet)
- Single-user (no multi-seat sync)
- Browser-dependent (requires JavaScript)
- Settings per-browser (no cloud sync)

### Planned Fixes (v2.1+)
- All above resolved with PI Web API integration

## Troubleshooting

**Settings not saving?**
- Check browser localStorage is enabled
- Clear browser cache and reload
- Try a different browser

**Audio not working?**
- Check system volume
- Verify browser hasn't muted audio (check browser settings)
- Click "Test Audio" in config to verify Web Audio API

**Clock hands look blurry?**
- This was a v1.0 bug — v2.0 uses DPR-aware rendering
- If still seeing blur, update your browser

**Configuration page won't load?**
- Ensure both files (.html) are in the same directory
- Check browser console for JavaScript errors (F12 → Console)
- Try clearing browser cache

## Technical Details

### Canvas Rendering
The clock face is rendered on HTML5 canvas with **devicePixelRatio-aware scaling**. This ensures crisp text and graphics on all displays (including Retina/4K screens) without blur from scaling artifacts.

### Time Calculations
Time fractions drive the entire system:
- **MinFrac** (0-1): Minute within the hour
- **SecFrac** (0-1): Second within the minute
- **HourFrac** (0-1): Hour within the 12-hour period

All alarm windows reference these fractions, making the system immune to timezone or DST issues.

### Alarm Logic
Alarms evaluate in priority order each second:
1. Check if within time window
2. Check if manual clear flag is set
3. Check if PI condition is met (if applicable)
4. Calculate escalation level
5. Play sound/speech if enabled
6. Update visual indicators

## Credits

**Original Concept:** Excel/PI Datalink clock for grid operations  
**Web Implementation:** HTML5/Canvas modernization  
**Design Reference:** Tudor Black Bay, Patek Nautilus, Omega Seamaster (dial textures)

## License

This project is open-source. Feel free to fork, modify, and deploy in your own organization.

## Support & Feedback

### Found a Bug?
1. Note the exact steps to reproduce
2. Check if it occurs in multiple browsers
3. Report with screenshots if applicable

### Have a Feature Request?
1. Describe the use case
2. Explain how it improves operations
3. Sketch any UI changes needed

### Questions?
Refer to the Configuration Reference section above, or open an issue with detailed context.

---

**Last Updated:** May 2026  
**Version:** 2.0  
**Status:** Production-ready (mock data mode)

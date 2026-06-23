# Match Tagger

A video-based Progressive Web App (PWA) for post-match analysis of GAA (Gaelic Athletic Association) sports games. Tag events from match videos with precise timestamps for detailed performance analysis.

## Overview

Match Tagger allows coaches and analysts to review match videos and tag key events (kickouts, turnovers, shooting) with precise timestamps and contextual data. Perfect for post-match analysis and performance review.

## Features

- **Embedded Video Player**: Load and play match videos directly in the app
- **Video Time Sync**: Set offset to sync game start with video timestamp
- **Event Categories**: Kickouts, Turnovers, and Shooting with detailed sub-options
- **Timestamp Capture**: Automatic capture of video/game time when tagging events
- **Edit & Delete**: Full editing capabilities for correcting tagged events
- **Real-Time Scoreboard**: Auto-calculates score from shooting events
- **CSV Export/Import**: Export data for analysis in Excel or other tools
- **Offline PWA**: Works completely offline with local storage persistence
- **Keyboard Controls**: Spacebar to play/pause video

## Getting Started

### Installation

1. Open `index.html` in a modern web browser (Chrome, Safari, Firefox, Edge)
2. For mobile use, add to home screen for full PWA experience

### Quick Start

1. **Load Video**: Click "📁 Load Video" and select your match video file
2. **Set Game Start**: Enter the video timestamp when the game actually starts (e.g., "02:30" if game starts 2:30 into the video)
3. **Configure Teams**: Click ⚙️ Settings to enter team names
4. **Tag Events**:
   - Play/pause video as you watch
   - Click a category tab (Kickouts, Turnovers, Shooting)
   - Select the relevant options (team, type, result, location, etc.)
   - Click "📌 Tag Event at XX:XX" to save the event
5. **Edit if Needed**: Click ✏️ on any event to edit details or timestamps
6. **Export**: Click "Download CSV" to export your analysis

## Event Categories

### Kickouts
Track all kickout situations with:
- **Team**: Which team took the kickout
- **Type**: Short, Long, Medium
- **Result**: Won, Lost, Breaking Ball, Contested
- **Location**: Left, Center, Right

### Turnovers
Track possession changes with:
- **Team**: Which team lost possession
- **Type**: Forced, Unforced, Interception, Tackle, Dispossessed, Block Down
- **Location**: Defensive Third, Middle Third, Attacking Third

### Shooting
Track all shot attempts with:
- **Team**: Which team shot
- **Type**: Free, Play, 45, Penalty
- **Location**: Inside 21m, 21-45m, Beyond 45m
- **Outcome**: Goal, Point, Wide, Short, Saved, Blocked

### Throw Up
Track throw-up situations with:
- **Won By**: Which team won the throw-up (Us/Opposition)

## Workflow

### Typical Analysis Session

1. **Preparation**
   - Have your match video file ready
   - Open Match Tagger in your browser
   - Load the video

2. **Setup**
   - Set the game start offset (sync video to game time)
   - Configure team names

3. **First Pass** - Tag major events
   - All kickouts
   - All scores and shot attempts
   - Major turnovers

4. **Second Pass** - Review and refine
   - Edit any incorrect timestamps
   - Add missed events
   - Verify categorizations

5. **Export**
   - Download CSV for deeper analysis
   - Import into Excel, Google Sheets, or analytics tools

## Video Time vs Game Time

**Understanding the Offset:**

Match videos often don't start exactly when the game starts. The video might include pre-game footage, anthems, or delays.

**Example:**
- Video shows game starting at 2:30
- Set offset to "02:30"
- When you tag an event at video time 15:32
- Game time is calculated as 15:32 - 2:30 = 13:02
- Event is saved as happening at 13:02 game time

This ensures all your event timestamps reflect actual game time, not video file time.

## Keyboard Shortcuts

- **Spacebar**: Play/Pause video

## Data Format

Events are stored with comprehensive metadata:
- Video timestamp (when it appears in the video)
- Game timestamp (actual game time)
- Category and team
- All selected options for that event type
- ISO timestamp of when event was tagged

## CSV Export

The exported CSV includes:
- Event number
- Game time (MM:SS)
- Video time (MM:SS)
- Category
- Team
- Team name
- All category-specific fields (kickout type, shot outcome, etc.)

Perfect for importing into:
- Microsoft Excel
- Google Sheets
- R / Python for statistical analysis
- Sports analytics software

## Architecture

- **Single-File App**: Entire application in one HTML file with embedded CSS and JavaScript
- **No Dependencies**: Pure vanilla JavaScript, no build process required
- **LocalStorage**: Event data persists locally in the browser
- **PWA Ready**: Manifest, service worker, and offline capability
- **HTML5 Video**: Native browser video playback (no encoding or processing)

## Technical Details

### Video Files
- App does NOT store video files
- Videos loaded as temporary object URLs during session
- You'll need to reload the video each time you open the app
- Supported formats: MP4, WebM, OGG (whatever your browser supports)

### Browser Support
- Chrome/Edge (recommended)
- Safari (iOS/macOS)
- Firefox
- Requires: HTML5 video, FileReader API, localStorage, ES6 JavaScript

### Storage
- Events data: ~200-300 bytes per event
- 100 events ≈ 20-30KB
- Well within localStorage limits
- Video files NOT stored (only referenced during session)

## Development

No build process required:
1. Edit `index.html`
2. Refresh browser to see changes
3. Test on mobile using browser dev tools or local server

## Future Features

**Productivity Metrics** (Coming Soon):
- Automatic calculation of:
  - Possession counts and types
  - Possessions → Attacks conversion %
  - Attacks → Shots conversion %
  - Shots → Scores conversion %
  - Overall productivity scoring

These will be calculated automatically from your tagged events.

## Tips for Best Results

### Efficient Tagging
- Use keyboard spacebar to quickly pause/play
- Tag events in real-time as you watch (don't try to memorize)
- Do multiple passes: kickouts first, then turnovers, then shooting
- Use the edit feature liberally - it's easier to fix than to remember

### Accuracy
- Pause the video at the exact moment of the event
- Check the game time before clicking "Tag Event"
- Review your events list periodically
- Edit timestamps if you notice errors

### Organization
- Use clear, consistent team names
- Export CSV after each session as backup
- Name your CSV exports with date and teams for easy reference

## Use Cases

- **Coach Review**: Analyze team performance after matches
- **Player Development**: Track individual patterns (if augmented with player data)
- **Opposition Analysis**: Study opponent tendencies for next match
- **Season Trends**: Combine multiple match CSVs to identify patterns
- **Performance Reports**: Generate data-driven insights for players and management

## License

Copyright © 2026 Match Tagger. All rights reserved.

## Support

For questions, feedback, or issues, please create an issue in the repository.

---

**Built for GAA coaches, analysts, and teams who want data-driven performance insights from match video.**

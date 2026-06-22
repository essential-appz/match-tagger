# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Match Tagger is a mobile-first web application for **post-match video analysis** of GAA (Gaelic Athletic Association) sports games. The entire application is contained in a single `index.html` file with embedded CSS and JavaScript.

**Key Characteristics:**
- Single-file application (no build process, no dependencies)
- Pure vanilla JavaScript, HTML, and CSS
- LocalStorage-based persistence
- Offline-capable PWA design
- **Video-based tagging** - tag events from match videos after the game
- Embedded HTML5 video player with timestamp capture
- Focus on Kickouts, Turnovers, and Shooting analytics

## Development

### Testing
- **Browser Testing**: Open `index.html` directly in a browser (Chrome, Safari, Firefox)
- Load a video file to test tagging functionality
- No build, compile, or bundle steps required

### Data Storage
All data persists in browser `localStorage`:
- `matchEvents`: Current session including events array, team names, video offset

## Architecture

### State Management
Global state variables in `<script>` section:
- `events`: Array of all tagged match events
- `currentCategory`: Selected event category ('Kickouts', 'Turnovers', 'Shooting')
- `currentEvent`: Current event being built (accumulates selected options)
- `team1Name`/`team2Name`: Team names
- `videoOffset`: Seconds offset between video start and game start
- `editingIndex`: Index of event being edited (-1 if not editing)
- `video`: Reference to HTML5 video element

### Event Data Structure
Each event contains:
```javascript
{
  category: "Kickouts" | "Turnovers" | "Shooting",
  team: "OUR" | "OPP",
  teamName: actual team name,
  videoTime: video timestamp in seconds,
  gameTime: calculated game time (videoTime - offset),
  gameTimeFormatted: "MM:SS" format,
  timestamp: ISO string when event was tagged,
  // Category-specific fields:
  // Kickouts:
  kickoutType: "Short" | "Long" | "Medium",
  kickoutResult: "Won" | "Lost" | "Breaking Ball" | "Contested",
  kickoutLocation: "Left" | "Center" | "Right",
  markTaken: "Yes" | "No",
  // Turnovers:
  turnoverType: "Forced" | "Unforced" | "Interception" | "Tackle" | "Dispossessed" | "Block Down",
  turnoverLocation: "Defensive Third" | "Middle Third" | "Attacking Third",
  // Shooting:
  shotType: "Free" | "Play" | "45" | "Penalty",
  shotLocation: "Inside 21m" | "21-45m" | "Beyond 45m",
  shotOutcome: "Goal" | "Point" | "Wide" | "Short" | "Saved" | "Blocked"
}
```

### Event Categories & Options

**Kickouts:**
- Team: OUR, OPP
- Kickout Type: Short, Long, Medium
- Kickout Result: Won, Lost, Breaking Ball, Contested
- Kickout Location: Left, Center, Right
- Mark Taken: Yes, No

**Turnovers:**
- Team: OUR, OPP
- Turnover Type: Forced, Unforced, Interception, Tackle, Dispossessed, Block Down
- Turnover Location: Defensive Third, Middle Third, Attacking Third

**Shooting:**
- Team: OUR, OPP
- Shot Type: Free, Play, 45, Penalty
- Shot Location: Inside 21m, 21-45m, Beyond 45m
- Shot Outcome: Goal, Point, Wide, Short, Saved, Blocked

### UI Components

**Video Player**
- HTML5 `<video>` element with standard controls
- Custom control buttons (Play/Pause, Skip ±5s, Load Video)
- Real-time video timestamp display
- Keyboard shortcut: Spacebar to play/pause

**Video Offset Sync**
- Input field for setting game start time in video (MM:SS format)
- `videoOffset` stored in seconds
- Game time = video.currentTime - videoOffset
- Allows syncing when game doesn't start at video 00:00

**Category Tabs**
- Three tabs: Kickouts, Turnovers, Shooting
- Selecting a tab shows category-specific option buttons
- Only one category active at a time

**Option Buttons**
- Each category has multiple option groups (Type, Result, Location, etc.)
- Single-select within each group (clicking deselects previous)
- Visual feedback with `.selected` class
- Data stored via `data-field` and `data-value` attributes

**Tag Event Button**
- Captures current video timestamp when clicked
- Calculates game time from video time and offset
- Creates event object from selected options
- Events auto-sorted by game time after insertion

**Events List**
- Displays all tagged events in chronological order
- Each event shows: game time, category, team, and all selected options
- Edit (✏️) and Delete (×) buttons for each event
- Clicking edit opens modal with editable fields

**Scoreboard**
- Tracks only Shooting category events
- Goals: Count of shotOutcome='Goal'
- Points: Count of shotOutcome='Point'
- Display format: "Goals:Points (Total)" e.g., "2:15 (21)"

### Key Functions

**Video Management:**
1. `loadVideo(event)` - Loads video file from file input, creates object URL
2. `togglePlayPause()` - Play/pause control
3. `skipBackward()` / `skipForward()` - Skip ±5 seconds
4. `updateVideoTime()` - Updates time display on timeupdate event
5. `updateOffset()` - Parses offset input and saves to state
6. `getGameTime()` - Returns video.currentTime - videoOffset

**Event Tagging Workflow:**
1. `selectCategory(category)` - Sets current category, shows option panel, clears selections
2. `selectOption(btn)` - Captures option selection, stores in `currentEvent` object
3. `updateTagButton()` - Updates button text with current game time
4. `tagEvent()` - Creates complete event object, adds to events array, sorts by time

**Event Editing:**
1. `editEvent(index)` - Opens edit modal, populates with event data
2. `saveEditedEvent()` - Updates event in array, re-sorts, saves
3. `deleteEvent(index)` - Removes event from array

**Storage:**
- `saveToStorage()` - Saves entire state to localStorage as JSON
- `loadFromStorage()` - Restores state on page load

**Scoreboard:**
- `updateScoreboard()` - Counts Goals and Points from Shooting events

### Important Patterns

**Video Time vs Game Time:**
- Video has its own timestamp (video.currentTime)
- Game time = video.currentTime - videoOffset
- All events store both videoTime and gameTime
- Events sorted by gameTime for chronological ordering

**Single-Selection Options:**
- Each option group allows only one selection
- Clicking a new option deselects the previous in that group
- Uses `data-field` to identify option groups
- Selected values stored in `currentEvent[field]`

**Event Persistence:**
- Events stored with both formatted ("MM:SS") and numeric (seconds) time values
- gameTimeFormatted for display, gameTime for sorting/calculations
- CSV export uses formatted times for readability

**Edit vs Create:**
- Create: Options selected via buttons, timestamp auto-captured
- Edit: Modal with text inputs for all fields, manual time editing allowed
- Both workflows result in same event structure

## Common Modifications

### Adding Event Options
1. Add new button to appropriate category's option group in HTML
2. Set `data-field` and `data-value` attributes
3. No JavaScript changes needed - `selectOption()` is generic

### Adding New Categories
1. Add new tab button calling `selectCategory('NewCategory')`
2. Create new `<div id="newcategoryOptions" class="event-options">` section
3. Add option groups with buttons
4. Update CSV export headers if needed

### Changing Video Controls
- Video element has standard HTML5 controls enabled
- Custom buttons call native video methods (play(), pause(), currentTime)
- Can add more controls using video API

### CSV Export Format
Modify `downloadCSV()` function:
- Dynamically builds headers from all unique event fields
- Events exported with: Event#, Game Time, Video Time, Category, Team, Team Name, + all custom fields
- Uses double quotes for CSV cell values

## Important Notes

**No Build Process**: This is intentionally a single-file app for offline portability. Don't split into separate JS/CSS files unless explicitly requested.

**Video Files**: The app does NOT store video files. Only a local object URL reference during the session. Users must:
- Load video from local file each session, OR
- Keep video file in a known location and load it repeatedly

**Browser Compatibility**: Requires modern browser with:
- HTML5 video support
- FileReader API for video loading
- localStorage for persistence

**Performance**: Video playback happens natively in browser. Event tagging is lightweight (just JSON data). No video processing or encoding.

## Keyboard Shortcuts

- **Spacebar**: Play/Pause video (when not focused on input)

## Workflow Example

1. User opens app
2. Click "Load Video" → Select match video file
3. Set "Game Start Offset" (e.g., if game starts at 2:30 in video, enter "02:30")
4. Play video
5. Pause at kickout → Click "Kickouts" tab
6. Select: Team=OUR, Type=Long, Result=Won, Location=Center, Mark=No
7. Click "Tag Event at 15:32" → Event saved with game time 15:32
8. Continue tagging throughout video
9. Edit any mistakes via Edit button
10. Export CSV when done

## GAA Context

This app is designed for Gaelic football/hurling post-match analysis. Common use cases:
- **Kickout analysis**: Win rates by type/location
- **Turnover tracking**: Forced vs unforced errors by location
- **Shooting efficiency**: Conversion rates by type/location

Scoring system:
- **Goal** = 3 points (ball in net)
- **Point** = 1 point (ball over bar)
- Score format: "Goals:Points (Total)" (e.g., "2:15 (21)" = 2 goals + 15 points = 21 total points)

## Future Enhancements

**Productivity Metrics** (not yet implemented):
- Number of Possessions (calculated from event data)
- Types of Possessions (breakdown by how possession was won)
- Possessions to Attacks % (efficiency metric)
- Attacks to Shots % (conversion metric)
- Shots to Scores % (accuracy metric)
- Productivity Scoring (overall rating)

These will be calculated from tagged events, not manually tagged themselves.

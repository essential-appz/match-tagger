# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Match Tagger is a mobile-first web application for tracking team-level match events during GAA (Gaelic Athletic Association) sports games. The entire application is contained in a single `index.html` file with embedded CSS and JavaScript.

**Key Characteristics:**
- Single-file application (no build process, no dependencies)
- Pure vanilla JavaScript, HTML, and CSS
- LocalStorage-based persistence
- Offline-capable PWA design
- Optimized for mobile/tablet use during live games
- Team-centric (not player-centric) event tracking

## Development

### Testing
- **Browser Testing**: Open `index.html` directly in a browser (Chrome, Safari, Firefox)
- No build, compile, or bundle steps required

### Data Storage
All data persists in browser `localStorage`:
- `matchEvents`: Current match state including events array, period, team names

## Architecture

### State Management
Global state variables in `<script>` section:
- `events`: Array of all logged match events
- `currentPeriod`: Game period state
- `currentCategory`: Selected event category (e.g., 'Possession|OUR')
- `selectedEvents`: Array of event types for current event
- `timerSeconds`: Game clock in seconds
- `team1Name`/`team2Name`: Team names
- `gameName`: Match name for exports

### Event Data Structure
Each event contains:
```javascript
{
  timestamp: ISO string,
  time: "MM:SS" (game clock),
  period: "1st Half Started" | "1st Half Ended" | "2nd Half Started" | "2nd Half Ended",
  category: "Possession" | "Attack Phase" | "Defense Phase" | "Turnover" | "Set Piece" | "Score Attempt" | "Score" | "Foul" | "Card",
  team: "OUR" | "OPP",
  teamName: actual team name,
  events: ["Won", "From Kickout"] (array of event types)
}
```

### Event Categories & Types

**Event Categories** (defined in `categories` object):
- Possession (OUR/OPP)
- Attack Phase (OUR/OPP)
- Defense Phase (OUR/OPP)
- Turnover (OUR/OPP)
- Set Piece (OUR/OPP)
- Score Attempt (OUR/OPP)
- Score (OUR/OPP)
- Foul (OUR/OPP)
- Card (OUR/OPP)

**Event Types** (defined in `eventTypes` object by category):
- Possession: Won, Lost, Long, Short, From Kickout, From Turnover
- Attack Phase: Build-up, Fast Break, Counter, Sustained, Inside 50
- Defense Phase: Structured, Press High, Drop Deep, Force Wide, Turnover Won
- Turnover: Forced, Unforced, Interception, Tackle, Block Down
- Set Piece: Kickout, Free Kick, Sideline, 45, Penalty
- Score Attempt: Shot, Goal Attempt, Point Attempt, Inside 21, Outside 45
- Score: Goal, Point, 2 Pointer, Free, Play
- Foul: Technical, Aggressive, Advantage, Free Against
- Card: Yellow, Red, Black

### UI Components

**Category Buttons**
- Each category has two buttons (OUR team and OPP team)
- Buttons show category name and team name
- Selecting a category enables relevant event type buttons

**Event Type Buttons**
- All possible event types shown, but disabled until category selected
- Only event types valid for the selected category are enabled
- Multiple event types can be selected per event

**Event Matrix**
- Rows: All category-team combinations
- Columns: All unique event types
- Cells show count of events matching that category-team-type combination
- Clicking a cell shows the matching events

**Scoreboard Calculation**
- Tracks only events in the "Score" category
- Goals: Count of events with 'Goal' type
- Points: Count of 'Point' type + (Count of '2 Pointer' type × 2)
- Display format: "Goals:Points (Total)" e.g., "2:15 (21)"
- `updateScoreboard()` iterates through events to calculate totals

### Key Functions

**Event Lifecycle:**
1. `selectCategory(btn, category, team)` - Sets current category, enables valid event buttons
2. `toggleEvent(btn, eventType)` - Adds/removes event types from selection
3. `saveEvent()` - Creates event object with game time, pushes to events array, auto-saves when switching categories
4. `updateMatrix()` - Recalculates and displays event counts in matrix
5. `updateScoreboard()` - Recalculates team scores from Score events
6. `updateRecentEvents()` - Shows last 10 events in reverse chronological order

**Auto-save Behavior:**
- Selecting a new category while events are selected automatically saves the current event
- Ending a half automatically saves any pending event
- Manual save not typically needed (happens automatically on category switch)

**Game Timer:**
- Starts at 00:00 for 1st Half, 30:00 for 2nd Half
- Managed by `timerInterval` (1-second ticks)
- `startTimer(startSeconds)`, `stopTimer()`, `updateClockDisplay()`
- Game time embedded in each event for CSV export

**Storage System:**
- `saveToStorage()` - Saves entire game state to localStorage as JSON
- `loadFromStorage()` - Restores game state on page load
- No multi-game save system (simpler than player-tagger)

## Common Modifications

### Adding Event Categories
1. Add to `categories` object
2. Add corresponding event types to `eventTypes` object
3. UI buttons auto-generate from these objects

### Adding Event Types
1. Add to appropriate category array in `eventTypes` object
2. Event type buttons auto-build from all unique types
3. Matrix columns auto-update

### Changing Team Structure
- Team names are configurable in Settings
- Always two teams: OUR and OPP
- Each category has instances for both teams

### CSV Export Format
Modify `downloadCSV()` function:
- Current headers: Event#, Timestamp, Game Time, Period, Category, Team, Team Name, Events
- Events exported in chronological order

## Important Patterns

**No Separation of Concerns**: This is intentionally a single-file app for offline portability. Don't split into separate JS/CSS files unless explicitly requested.

**Matrix Performance**: The matrix has Category-Team combinations × Event Types. DOM updates:
- Reset all cells to "0"
- Count events into a hash map
- Update only cells with non-zero counts

**Multi-Select Events**: Unlike player-tagger where "Touch" was auto-included, match-tagger allows selecting multiple event types per event (e.g., "Possession Won" + "From Kickout" + "Long").

**Period Validation**: Categories cannot be selected until a half is started (enforced in `selectCategory()`).

## Browser Console Utilities

All global functions are accessible for debugging:
- `events` - View current events array
- `saveEvent()` - Manually trigger event save
- `updateMatrix()` - Force matrix refresh

## GAA Context

This app is designed for Gaelic football/hurling at the team level rather than individual player tracking. Scoring system:
- **Goal** = 3 points (ball in net)
- **Point** = 1 point (ball over bar)
- **2 Pointer** = 2 points (variant scoring)
- Score format: "Goals:Points (Total)" (e.g., "2:15 (21)" = 2 goals + 15 points = 21 total points)

## Differences from Player Tagger

Match Tagger is based on Player Tagger's architecture but focuses on team-level match flow:

1. **Event Model**: Team categories instead of player numbers
2. **Multi-Select**: Can select multiple event types per event (no auto-include like "Touch")
3. **Simplified Saves**: No checkpoint system - single game in progress at a time
4. **No Premium Features**: All features available without licensing
5. **Match Flow Focus**: Tracks phases, possession, and tactical elements rather than individual contributions

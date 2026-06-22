# Match Tagger

A mobile-first Progressive Web App (PWA) for tracking team-level events and match flow during GAA (Gaelic Athletic Association) sports games.

## Overview

Match Tagger takes a holistic view of GAA matches, tracking team-level events like possession phases, turnovers, attack patterns, and defensive structures rather than individual player statistics.

## Features

- **Team-Level Event Tracking**: Track possession, attack phases, defense phases, turnovers, set pieces, score attempts, and more
- **Real-Time Match Flow**: Monitor team performance and match dynamics as they unfold
- **Event Matrix**: Visual breakdown of event counts by category and type
- **Live Scoreboard**: Automatic score calculation from Goal, Point, and 2 Pointer events
- **Game Clock & Periods**: Integrated timer with period management (1st/2nd half)
- **Timeline View**: Chronological display of all match events
- **CSV Export/Import**: Download match data for analysis or share with others
- **Offline PWA**: Works completely offline with local storage persistence
- **Mobile-Optimized**: Touch-friendly interface designed for tablets and phones

## Getting Started

### Installation

1. Open `index.html` in a modern web browser (Chrome, Safari, Firefox, Edge)
2. For mobile use, add to home screen for full PWA experience

### Quick Start

1. **Open Settings**: Click the Settings (⚙️) button
2. **Set Team Names**: Enter your team name and opposition name
3. **Start New Game**: Click "Start New Game"
4. **Begin Match**: Click "Start 1st Half" to start the game clock
5. **Track Events**:
   - Select an event category (e.g., "Possession (Us)")
   - Select event types (e.g., "Won", "From Kickout")
   - Events save automatically when you switch categories
6. **View Analytics**: Check the Event Matrix and Timeline for match insights
7. **End Game**: Click "End Game" when finished - CSV will auto-download

## Event Categories

- **Possession**: Track possession won/lost, long/short, from kickouts or turnovers
- **Attack Phase**: Build-up play, fast breaks, counters, sustained attacks
- **Defense Phase**: Structured defense, high press, drop deep, force wide
- **Turnover**: Forced/unforced, interceptions, tackles, block downs
- **Set Piece**: Kickouts, free kicks, sidelines, 45s, penalties
- **Score Attempt**: Shots, goal/point attempts, distance-based tracking
- **Score**: Goals, points, 2 pointers, from frees or play
- **Foul**: Technical, aggressive, advantage, free against
- **Card**: Yellow, red, black cards

## Architecture

- **Single-File App**: Entire application in one HTML file with embedded CSS and JavaScript
- **No Dependencies**: Pure vanilla JavaScript, no build process required
- **LocalStorage**: All data persists locally in the browser
- **PWA Ready**: Manifest, service worker, and offline capability

## Data Format

Events are stored with:
- Timestamp (ISO format)
- Game time (MM:SS)
- Period (1st/2nd half, started/ended)
- Category (e.g., "Possession", "Score")
- Team (OUR/OPP)
- Team name
- Event types (array of selected events)

## CSV Export

Export includes:
- Event number
- Timestamp
- Game time
- Period
- Category
- Team indicator
- Team name
- Event types (comma-separated)

## Browser Support

- Chrome/Edge (recommended)
- Safari (iOS/macOS)
- Firefox
- Any modern browser with localStorage and ES6 support

## Development

No build process required. Simply:
1. Edit `index.html`
2. Refresh browser to see changes
3. Test on mobile by serving locally or using browser dev tools

## License

Copyright © 2026 Match Tagger. All rights reserved.

## Contact

For questions, feedback, or support, please create an issue in the repository.

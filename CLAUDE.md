# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a web-based puzzle game for "Crack and Stack," an upcoming card game from Triple Point Publishing. The application is a single-page HTML application that lets users solve word puzzles (semordnilaps and anagrams) and then race to find the hidden solution word in the card's border.

## Development Commands

### Local Development
```bash
# Start local HTTP server for development
python3 -m http.server 8001

# Alternative server (if port 8001 is in use)
python3 -m http.server 8000
```

### Testing with Playwright
```bash
# Run Playwright tests
npx playwright test

# Run specific test file
npx playwright test test-app.spec.js
```

### Bounding Box Definition Workflow
The app includes a special mode for defining click areas on puzzle cards:

1. Click "📦 Define Boxes" button in the app
2. Drag bounding boxes around answer words in card borders
3. Use "💾 Save JSON" to download updated `puzzles-updated.json`
4. Replace `puzzles.json` with the updated file

## Architecture

### Core Application Structure
- **Single HTML file** (`index.html`) containing all HTML, CSS, and JavaScript
- **Self-contained** - no build process or external dependencies for runtime
- **Vanilla JavaScript** class-based architecture using `DailyPuzzleGame` class

### Data Structure
- **`puzzles.json`** - Configuration file containing all puzzle definitions
- Each puzzle has: `id`, `image`, `answer`, and optional `clickBox` coordinates
- Only puzzles with complete `clickBox` definitions are used in gameplay
- **Two arrays in memory**: `this.puzzles` (complete) and `this.allPuzzles` (all)

### Game States
The application operates in distinct states:
- `loading` - Initial puzzle load
- `input` - Player typing answer to cropped image
- `searching` - Full image revealed, player finding border word
- `completed` - Puzzle solved, stats displayed

### Image Assets
- **`/images/`** - Puzzle card images (101 total)
  - Naming format: `"CLUE -_ ANSWER[face,1].png"`
  - Answer word is hidden in one of four borders (top 30%, bottom 10%, left 30%, right 30%)
- **`/assets/`** - Game symbols (semordnilap and anagram icons)

### Special Modes
- **Normal Gameplay** - Uses only puzzles with defined click boxes
- **Bounding Box Definition Mode** - Administrative mode for defining click areas
  - Accessible via "📦 Define Boxes" button
  - Auto-saves on mouse-up, keeps data in memory until manual export

### Key Implementation Details
- **Date-based puzzle selection** using day-of-year modulo puzzle count
- **Click area scaling** from natural image size to display size
- **LocalStorage** for instructions dismissal (`crack-and-stack-instructions-seen`)
- **Flicker prevention** via `resetGameQuiet()` for navigation vs `resetGame()` for initial load
- **Cursor preservation** in uppercase input conversion to prevent typing interruption

### Testing and Screenshots
Playwright scripts exist for automated screenshot generation and testing of the bounding box definition workflow. These are primarily used for verifying click area positioning during development.
# CanogaGame

A multi-platform implementation of Canoga, a dice-and-strategy board game.
This repo includes a web UI, a C++ CLI version, and an Android app.

## Features
- Tournament-style play across rounds
- Human vs Computer, Human vs Human, and Computer vs Computer modes
- Configurable board sizes (9-11)
- Manual dice input and queued dice sequences (Web)
- Save and load game snapshots (.txt) (Web)
- Move log and rewind support (Web)

## Screenshots
| Screenshot 1 |
| --- |
| ![Screenshot 1](screenshots/one.png) |
| C++ CLI help screen listing cover/uncover options with an AI recommendation for dice sum 7. |

| Screenshot 2 |
| --- |
| ![Screenshot 2](screenshots/two.png) |
| C++ CLI round-over summary showing a human win, queued advantage, and tournament score totals. |

| Screenshot 3 |
| --- |
| ![Screenshot 3](screenshots/three.png) |
| Android round result screen showing a computer win by cover and updated totals. |

| Screenshot 4 |
| --- |
| ![Screenshot 4](screenshots/four.png) |
| Android in-round screen with dice roll info, board state, controls, and game log. |

| Screenshot 5 |
| --- |
| ![Screenshot 5](screenshots/five.png) |
| Web round setup screen in Computer vs Computer mode with a roll-off result for first player. |

| Screenshot 6 |
| --- |
| ![Screenshot 6](screenshots/six.png) |
| Web rewind dialog showing move history and board previews for the selected turn. |

## Project layout
- `Android/` - Android app source
- `CLI/` - C++ CLI implementation
- `Web/` - Web app source and docs
- `screenshots/` - add UI captures here

## Build

### CLI
Requirements: CMake 3.31+ and a C++20 compiler.

From `CLI/`:

```sh
cmake -S . -B build
cmake --build build
```

### Android
From `Android/`:

```sh
./gradlew assembleDebug
```

### Web
No build step; serve `Web/Source` as static files.

## Run

### Web
Serve `Web/Source` with a local server (ES modules require http):

```sh
cd Web/Source
python3 -m http.server 8000
```

Then open `http://localhost:8000/`.

### CLI
From `CLI/`:

```sh
./build/c__
```

### Android
Open `Android/` in Android Studio and run the app.

## Saved game format
- Save/load uses `.txt` snapshot files.
- Upload saved games from the Web welcome screen.
- Example snapshots live in `Web/Source/samples/`.

## Notes
- Web UI uses ES modules, so it must be served over http.

## Why did you build this?
I built this to practice implementing the same game across multiple platforms and to explore how the same rules and AI can drive CLI, web, and mobile UIs.

## What’s hard about this project?
Keeping the rules consistent across three codebases, validating cover/uncover combinations, and managing turn flow, snapshots, and rewind logic without desyncing the UI.

## Where is the “intelligence” / decision-making?
The AI move selection evaluates possible cover/uncover options for a dice roll, weighs advantage squares, and explains its reasoning to the player.

## What skills does this prove?
Game rules modeling, state management, AI move evaluation, cross-platform UI implementation (CLI/Web/Android), and snapshot serialization.

## Documentation
- `Web/doc/` - notes and reports
- `Web/doc/jsdoc/index.html` - generated JS docs
- `Web/doc/log.md` - development log

## Tech
- JavaScript (ES modules), HTML, CSS
- C++20 with CMake
- Java (Android SDK) with Gradle (Kotlin DSL)

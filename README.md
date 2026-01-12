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

## Screenshots
| Screenshot 1 |
| --- |
| ![Screenshot 1](screenshots/one.png) |
| Add a short caption for the Web UI welcome screen. |

| Screenshot 2 |
| --- |
| ![Screenshot 2](screenshots/two.png) |
| Add a short caption for a round in progress. |

| Screenshot 3 |
| --- |
| ![Screenshot 3](screenshots/three.png) |
| Add a short caption for the end-of-round screen. |

## Saved game format
- Save/load uses `.txt` snapshot files.
- Upload saved games from the Web welcome screen.
- Example snapshots live in `Web/Source/samples/`.

## Notes
- Web UI uses ES modules, so it must be served over http.

## Documentation
- `Web/doc/` - notes and reports
- `Web/doc/jsdoc/index.html` - generated JS docs
- `Web/doc/log.md` - development log

## Tech
- JavaScript (ES modules), HTML, CSS
- C++20 with CMake
- Java (Android SDK) with Gradle (Kotlin DSL)

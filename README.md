# CanogaGame

A multi-platform implementation of Canoga, a dice-and-strategy board game.
This repo includes a web UI, a C++ CLI version, and an Android app.

Built as coursework for my Organization Programming Language class.

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
| C++ CLI help screen listing cover/uncover options with a computer recommendation for dice sum 7. |

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
I built this to practice implementing the same game across multiple platforms and to explore how the same rules and computer logic can drive CLI, web, and mobile UIs.

## What’s hard about this project?
Keeping the rules consistent across three codebases, validating cover/uncover combinations, and managing turn flow, snapshots, and rewind logic without desyncing the UI.

## Decision-making rule (computer player)
- Generate all legal cover and uncover combinations for the current dice sum.
- If no legal combinations exist, take no move.
- If any cover combination would cover all remaining squares, cover immediately to win the round.
- Else if any uncover combination would uncover all opponent squares, uncover immediately to win the round.
- Otherwise prefer covering; if no cover moves exist, uncover.
- Choose the combination with the most squares; if tied, prefer higher-value squares (Web uses higher total sum; CLI/Android use highest square).

Best-move algorithm: once the move type is selected, each candidate combination is ranked by square count first. In the Web code (`_pickBestCombo` in `Web/Source/js/model/ComputerPlayer.js`), ties break by higher total sum; in Android/CLI (`chooseMove` in `Android/app/src/main/java/com/example/oplcanoga/model/ComputerPlayer.java` and `chooseBestComboJava` in `CLI/Source Files/Computer.cpp`), ties break by the highest square value. This favors moves that cover or uncover more squares, and prioritizes larger values when the count is tied.

Pseudo code:
```text
coverOptions = getCoverOptions(diceSum)
uncoverOptions = getUncoverOptions(diceSum)

if coverOptions empty and uncoverOptions empty:
  return NONE

if any coverOption covers all remaining squares:
  return COVER with that option

if any uncoverOption uncovers all opponent squares:
  return UNCOVER with that option

if coverOptions not empty:
  candidates = coverOptions
  action = COVER
else:
  candidates = uncoverOptions
  action = UNCOVER

best = candidates[0]
for each option in candidates:
  if option.count > best.count:
    best = option
  else if option.count == best.count:
    if tieBreak(option) > tieBreak(best):
      best = option

return action with best

tieBreak(option):
  Web: sum(option)
  Android/CLI: max(option)
```

## What skills does this prove?
Game rules modeling, state management, computer move evaluation, cross-platform UI implementation (CLI/Web/Android), and snapshot serialization.

## Documentation
- `Web/doc/` - notes and reports
- `Web/doc/jsdoc/index.html` - generated JS docs
- `Web/doc/log.md` - development log

## Tech
- JavaScript (ES modules), HTML, CSS
- C++20 with CMake
- Java (Android SDK) with Gradle (Kotlin DSL)

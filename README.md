# CanogaGame
> Multi-platform Canoga: CLI, Web, and Android.

[![Demo](https://img.shields.io/badge/demo-live-16a34a?style=for-the-badge)](http://projects.canogagame.danielrajakumar.com/) ![Platforms](https://img.shields.io/badge/platform-web%20%7C%20cli%20%7C%20android-0ea5e9?style=for-the-badge) ![Languages](https://img.shields.io/badge/languages-js%20%7C%20c%2B%2B%20%7C%20java-6366f1?style=for-the-badge) ![Coursework](https://img.shields.io/badge/coursework-OPL%20class-f59e0b?style=for-the-badge)

A multi-platform implementation of Canoga, a dice-and-strategy board game.
Built as coursework for my Organization Programming Language class.

**Demo:** http://projects.canogagame.danielrajakumar.com/

## Table of Contents
- [What it is](#what-it-is)
- [How to use it](#how-to-use-it)
- [How to build it](#how-to-build-it)
- [Why it matters](#why-it-matters)
- [What's hard](#what-s-hard)
- [What skills it proves](#what-skills-it-proves)
- [Documentation](#documentation)
- [Tech Stack](#tech-stack)
- [Screenshots](#screenshots)

## What it is

### Features
- Tournament-style play across rounds
- Human vs Computer, Human vs Human, and Computer vs Computer modes
- Configurable board sizes (9-11)
- Manual dice input and queued dice sequences (Web)
- Save and load game snapshots (.txt) (Web)
- Move log and rewind support (Web)

### Project Layout
- `Android/` - Android app source
- `CLI/` - C++ CLI implementation
- `Web/` - Web app source and docs
- `screenshots/` - UI captures

### Decision-Making (Computer Player)
Implementation lives in:
- `Web/Source/js/model/ComputerPlayer.js`
- `Android/app/src/main/java/com/example/oplcanoga/model/ComputerPlayer.java`
- `CLI/Source Files/Computer.cpp`

Rule summary:
- Generate all legal cover and uncover combinations for the current dice sum.
- If no legal combinations exist, take no move.
- If any cover combination would cover all remaining squares, cover immediately to win the round.
- Else if any uncover combination would uncover all opponent squares, uncover immediately to win the round.
- Otherwise prefer covering; if no cover moves exist, uncover.
- Choose the combination with the most squares; if tied, prefer higher-value squares (Web uses higher total sum; CLI/Android use highest square).

**Best-move ranking:** once the move type is selected, each candidate combination is ranked by square count first. In the Web code (`_pickBestCombo` in `Web/Source/js/model/ComputerPlayer.js`), ties break by higher total sum; in Android/CLI (`chooseMove` in `Android/app/src/main/java/com/example/oplcanoga/model/ComputerPlayer.java` and `chooseBestComboJava` in `CLI/Source Files/Computer.cpp`), ties break by the highest square value. This favors moves that cover or uncover more squares, and prioritizes larger values when the count is tied.

**Pseudo code:**
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

## How to use it

### Quick Start (Web)
Serve `Web/Source` with a local server (ES modules require http):

```sh
cd Web/Source
python3 -m http.server 8000
```

Open `http://localhost:8000/`.

### Run
**CLI:** `./build/c__` from `CLI/`

**Android:** Open `Android/` in Android Studio and run the app.

**Web:** Follow the Quick Start steps above.

### Saved Game Format
- Save/load uses `.txt` snapshot files.
- Upload saved games from the Web welcome screen.
- Example snapshots live in `Web/Source/samples/`.

### Notes
> [!NOTE]
> The Web UI uses ES modules, so it must be served over HTTP.

## How to build it

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

## Why it matters
I built this to practice implementing the same game across multiple platforms, and it matters because it demonstrates how one ruleset and computer strategy can drive CLI, web, and mobile UIs.

## What's hard
Keeping the rules consistent across three codebases, validating cover/uncover combinations, and managing turn flow, snapshots, and rewind logic without desyncing the UI.

## What skills it proves
Game rules modeling, state management, computer move evaluation, cross-platform UI implementation (CLI/Web/Android), and snapshot serialization.

## Documentation
- `Web/doc/` - notes and reports
- `Web/doc/jsdoc/index.html` - generated JS docs
- `Web/doc/log.md` - development log

## Tech Stack
| Layer | Tech |
| --- | --- |
| Web | ![JavaScript](https://img.shields.io/badge/JavaScript-f7df1e?style=flat-square&logo=javascript&logoColor=000) ![HTML5](https://img.shields.io/badge/HTML5-e34f26?style=flat-square&logo=html5&logoColor=fff) ![CSS3](https://img.shields.io/badge/CSS3-1572b6?style=flat-square&logo=css3&logoColor=fff) |
| CLI | ![C++](https://img.shields.io/badge/C%2B%2B-00599c?style=flat-square&logo=c%2B%2B&logoColor=fff) ![CMake](https://img.shields.io/badge/CMake-064f8c?style=flat-square&logo=cmake&logoColor=fff) |
| Android | ![Java](https://img.shields.io/badge/Java-007396?style=flat-square&logo=java&logoColor=fff) ![Gradle](https://img.shields.io/badge/Gradle-02303a?style=flat-square&logo=gradle&logoColor=fff) |

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

# Penalty Shootout

A football penalty-shootout game that runs in the browser on laptop and phone. You are the kicker and the computer is the goalkeeper. Each level has 5 kicks and you need 3 goals to clear it. Every third level is a boss fight. Built by Bryn and two young developers, with English and Swedish text.

The whole game is one HTML file with plain JavaScript and a canvas. There are no libraries, no build step and no server.

## Run it

Open `penalty-shootout-hd.html` in a browser. That's it.

- Pictures load from an `assets` folder next to the HTML file, or from pictures built into the file (see below). If a picture is missing, the game draws a stand-in in code, so nothing breaks.
- The "Russo One" font comes from Google Fonts. Offline, the game falls back to Impact or Arial Narrow.
- On a phone the game asks you to turn the phone sideways.

## Files

| File | What it is |
| --- | --- |
| `penalty-shootout-hd.html` | The game. Code and built-in pictures in one file. |
| `penalty-shootout-spec.md` | The design spec: rules, difficulty, bosses, assets. Read this first for the "why". |
| `embed-assets.html` | Browser tool that builds pictures from `assets` into the game file. No installs. |
| `assets/` | Source pictures (not always in the repo). See the names below. |

## Controls

- **Aim:** move the mouse or drag a finger, or use the arrow keys.
- **Start the timing ring:** click, lift your finger, or press space.
- **Kick:** click, tap, or press space when the white ring is on the green.
- **Language:** `L` on the title screen, or the flag buttons.

## How the game is organised

Everything is in the `<script>` block of `penalty-shootout-hd.html`. Useful landmarks, which you can search for by name:

- **`S`** is the game state. `S.phase` is the state machine: `menu`, `team`, `intro` (boss levels only), `aim`, `ring`, `shot`, `result`, `end`.
- **`P(level)`** sets the difficulty: ring time, green-zone width, keeper reaction time, and how often the keeper picks the right side. This is where to tune.
- **`bossOf(level)`** returns the boss for a level, or -1. Every third level is a boss, cycling through the five bosses in the order rexo, kraken, frostbite, titan, gus.
- **`kick()`** and **`resolve()`** decide the result of a kick (goal, save, miss) and apply each boss's special rule.
- **`TEXT`** holds all on-screen words, in `en` and `sv`. Add new text to both languages.
- **`TEAMS`** lists the teams: id, player name, shirt number, and kit colours.
- **`EMBED`** holds the pictures that are built into the file. **`IMG`** holds every loaded picture.
- **`drawKeeper`, `drawIntro`, `drawBall`, `paintBackground`** draw the keeper and bosses, the boss intro banner, the ball, and the stadium and pitch. The background is painted once and reused. Set `bgDirty=true` to repaint it.
- **`BOSS_DIVE_W`** controls how large a diving boss is drawn.

Saved in the browser (`localStorage`): `best` (best level), `lang`, and `team`.

## Pictures

The game looks for these names in `assets` (PNG for figures, JPEG or JPG for backgrounds):

- `stadium`, `pitch`, `ball`, `keeper-stand`, `keeper-dive-left`
- `kicker-<team>` and `kicker-<team>-kick`, where team is `miami`, `barcelona` or `sweden`
- `boss-<name>` and `boss-<name>-dive`, where name is `rexo`, `kraken`, `frostbite`, `titan` or `gus`
- `stadium-<name>-boss`, a different stadium for each boss level, with the same five names

Notes:

- Figures need a transparent background. Dive pictures show the boss diving to its left, and the game flips them for the right.
- A boss without a dive picture tilts its standing picture instead. A boss without its own stadium uses `stadium`.
- Pictures built into the file win over pictures in `assets`.

### Building pictures into the game file

Pictures in `assets` only show when you open the game from your own computer. A phone, or a link you share, can't read your `assets` folder. To fix this:

1. Open `embed-assets.html` in Chrome or Edge.
2. Choose the game file, then select the pictures from `assets`.
3. Click build. A new game file downloads with the pictures inside and your original is untouched.

The tool crops each picture to the figure, removes the green edge left by background removal, and shrinks it. The result is a bigger file, around 1 to 2 MB depending on how many pictures you add. Run it again whenever you add or change a picture. Use Chrome or Edge, because Safari can't make the picture format the tool needs.

## Adding things

- **A language:** add an entry to `TEXT` with all the same keys, and a button to `LBTN`.
- **A team:** add an entry to `TEAMS` and a name to `TEXT.<lang>.teams`. Add `kicker-<id>` and `kicker-<id>-kick` pictures. The team picker has room for three cards, so a fourth team needs the layout in `TBTN` changed. A team with `soon:true` shows as "Coming soon" and can't be picked.
- **A boss:** the intro text lives in `TEXT.<lang>.bosses`, and each boss's rule is in `kick()`. Bosses are tied to a position in the five-name list, so a sixth boss also needs the `bossOf` cycle and the name lists changed.

## Testing

There are no automated tests. We check the game by playing it, and by taking screenshots with a headless browser. The quickest ways to test a specific situation are to set the state from the browser console, for example `S.level=3; newLevel()` to jump to a boss level.

## Things to know

- **Real player names:** the team picker shows the names of real players (as text only). There are no club crests or sponsor logos. Check the licensing before sharing the game widely.
- **Spec:** `penalty-shootout-spec.md` is the source of truth for the design. Update it when you change how the game plays.
- **Known gaps:** there is no real slow-motion replay of goals, no 2-player mode, and no profile picker yet. These are in the spec as later ideas.

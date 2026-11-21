# Penalty Shootout: Game Design Spec (v0.3)

Prepared for Bryn and the two young game developers, who are building this game together. Status: draft for review before coding starts.

## 1. Vision

A football penalty-shootout game that runs in a web browser on both laptop and mobile. This first version is single player: the human is the kicker and the computer is the goalkeeper. Each level has 5 kicks, and you win the level by scoring 3 or more. The keeper gets harder with every level.

## 2. Developers and player

- **Developers:** Bryn and the two kids design and build the game as a team.
- **Mode:** version 1 is single player, with one human kicker against the computer keeper. There is no 2-player mode yet.
- **Player profile:** a **name**, a chosen **national team look**, and saved **level progress**. Anyone can save a profile on the same device and take turns, one player at a time.
- Progress is saved on the device only, with no accounts and no sign-up. Nothing is sent over the internet.
- A profile picker is the first screen, and players can switch at any time.

## 3. Core loop (one kick)

1. **Aim:** place a target inside the goal.
2. **Time:** a ring closes in on the ball.
3. **Kick:** tap or click when the ring is in the right place.
4. **Result:** the keeper dives, and the kick ends as a goal, a save, or a miss.

After 5 kicks the level ends. 3 or more goals is a win and unlocks the next level. Fewer than 3 goals means you lose the level and must play it again. Play stops early once the result is certain (for example, 3 goals scored).

## 4. Controls (laptop and mobile)

Both platforms use the same two actions, so the game feels identical everywhere.

| Action | Laptop | Mobile |
|---|---|---|
| Aim | Move the mouse (or arrow keys) | Touch and drag on the goal |
| Kick | Click or press space | Tap anywhere |

- Aiming and kicking are separate steps, so a finger never covers the ring at the moment of timing.
- On mobile the game asks the player to turn the phone sideways (landscape). The screen scales to fit any size.
- All buttons are large enough for small fingers.

## 5. The timing ring

A ring closes in on the ball. Where the ring is when you kick decides the shot.

| Ring zone | Timing | Result |
|---|---|---|
| Red (outer) | Far too early | Weak dribble, easy save or a miss |
| Yellow | A little early | A bit soft, easy for the keeper |
| **Green** | Just right | Perfect, powerful shot exactly on target |
| Yellow | A little late | A bit hard, drifts slightly off target |
| Red (inner) | Far too late | Blasted over the bar or wide, a miss |

- Green gives a flash and a "PERFECT!" pop. Red gives a red flash and a crowd groan.
- A perfect shot is much harder for the keeper to reach.
- As levels rise, the ring closes faster and the green zone shrinks.
- Colour is not the only signal: zones also differ in brightness and pattern, so the ring is readable for colour-blind players.

## 6. Keeper difficulty ladder

Starting values to tune during playtesting.

| Level | Reaction delay | Dive speed | How it chooses a side | Green zone | Ring speed |
|---|---|---|---|---|---|
| 1 | Very slow | Slow | Random | Very wide | Slow |
| 2 to 3 | Slow | Slow | Random | Wide | Slow |
| 4 to 5 | Medium | Medium | Random, sometimes a lucky guess | Medium | Medium |
| 6 to 8 | Quick | Fast | Remembers your favourite side | Narrow | Fast |
| 9 and up | Very quick | Very fast | Learns your habits | Very narrow | Very fast |

- Every **3rd level** (3, 6, 9, 12 and so on) is a boss keeper fight, designed together by the whole team. See the boss table below.
- Keepers have small tells (for example, leaning before a dive), so sharp players can learn to read them.
- A keeper can never be perfect: a green shot into a top corner always has a chance of going in.
- A shot aimed straight at the keeper's body is always saved. Kraken Keeper's tentacles make the blocked area wider.

Each boss gets a name, a costume or monster look, and one signature move. Bryn and the kids design them together.

| Level | Boss name | Costume or monster | Signature move |
|---|---|---|---|
| 3 | Rexo the T-Rex | A dinosaur in keeper gloves with tiny arms | Huge leaps for high shots, but weak on low corners |
| 6 | Kraken Keeper | A sea monster with tentacles in the net | Tentacles shoot out to block the corners |
| 9 | Frostbite | An ice giant that freezes over the pitch | Slows your ring down for one kick |
| 12 | The Titan | A giant robot goalkeeper | Teleports to where you aim, unless you hit green |
| 15 | Gargoyle Gus | A stone statue that comes to life | Stays frozen until you kick, then slams across the goal |

## 7. Levels and scoring

- 5 kicks per level. Score 3 or more goals to win it; with fewer than 3 you lose the level and play it again.
- A scoreboard shows 5 dots (green for a goal, red for a miss or a save).
- Each profile remembers the highest level reached and the best score per level.
- Levels never run out: the difficulty keeps rising up to a cap.

## 8. National team looks

- Players choose a national team look, built from that country's flag colours and a simple kit design (stripes, halves, or a plain shirt).
- No official crests, sponsor logos, or real player names are used, so the game stays safe to share and free of licensing problems.
- Starting list: Sweden, Brazil, Argentina, England, Germany, France, Spain, Netherlands, Italy, Japan, USA, Nigeria. This list is easy to extend.
- The kicker's shirt, shorts and socks update on the field and in the profile picker.

## 9. Screens

1. **Who's playing?** The profile picker, with a button to add a new player.
2. **Pick your team.** A grid of team looks.
3. **Level select or Next level.** Shows progress.
4. **The pitch.** Goal, keeper, ball, aim target, timing ring and scoreboard.
5. **Level result.** Win or retry, with confetti on a win.
6. **Settings.** Music on or off, effects on or off.

## 10. Audio

- **Music:** a looping upbeat tune on the menus and a calmer one during play. All music is created for this game, using simple synthesised tracks, so nothing is borrowed from existing songs.
- **Sound effects:** ball kick, net ripple, crowd cheer, crowd groan, post hit, whistle, and a keeper save.
- Music and effects have separate on or off switches.
- Browsers block sound until the first tap, so the first screen asks the player to tap to start.

## 11. Look and feel

- The long-term goal is a realistic look, like a real football match in a FIFA-style game: a floodlit stadium, a detailed pitch, a roaring crowd, and lifelike players and keepers. Boss keepers are the exception, with their own costumes and monster looks.
- Version 1 is drawn entirely in code (shapes, gradients and animation), so there are no image files to manage. Version 2 layers in realistic image files for the stadium, players, keepers and boss artwork.
- Short animations: kick, dive, net ripple, confetti.

## 12. Technical approach

- Version 1 is a single HTML file with plain JavaScript and the canvas, with everything drawn in code and the audio synthesised in the browser. Version 2 adds an assets folder for images. No libraries or installs.
- One input system based on pointer events, which covers mouse and touch.
- The canvas scales to the screen size, in landscape.
- Profiles and progress are stored in the browser on the device.
- The game logic (keeper AI, scoring, difficulty) is kept separate from the drawing, so it is easy to tweak.

## 13. Build plan

1. Pitch, goal and ball on screen, scaling for laptop and phone.
2. Aim target with mouse and touch.
3. Timing ring and kick power and accuracy.
4. Ball flight and goal, save and miss detection.
5. Keeper movement and the difficulty ladder.
6. Five-kick levels, scoreboard and win or retry.
7. Profiles, team looks and saved progress.
8. Music, sound effects and polish.

Version 2 then swaps the drawn shapes for realistic image files and final boss artwork. Each step of version 1 produces something playable, so the kids can test it and give feedback at every stage.

## 14. Assets (version 2)

The game loads images from an `assets` folder placed next to `penalty-shootout.html`. If an image is missing, the game keeps using the drawn version, so images can be added one at a time and the game never breaks. Status: the code that loads these images is not built yet.

| File | What it shows | Notes |
| --- | --- | --- |
| stadium.jpg | Night stadium stands, crowd and floodlights | Wide 16:9, no pitch or goal |
| pitch.jpg | Mown grass from behind the penalty spot | Wide 16:9, no lines |
| goal.png | Goal frame and net from the front | Transparent background |
| ball.png | Football | Transparent background |
| keeper-stand.png | Goalkeeper facing us, arms spread | Transparent background, vertical 1:2 |
| keeper-dive-left.png | Goalkeeper in a dive to his left | Transparent background, wide 2:1. The game flips it for the right side |

Later: five boss images (boss-rexo.png, boss-kraken.png, boss-frostbite.png, boss-titan.png, boss-gus.png) and extra keeper poses.

Rules for all images:

- One style: a photorealistic night match under floodlights, so the pieces match.
- No logos, text, sponsors or real player likeness. Kits are plain colours.
- Transparent PNGs: generate on a solid green background (#00ff00), then remove it with a background-remover.
- Animation comes from code moving, rotating, flipping and swapping still images.
- The published link cannot load an assets folder, so images only appear when the file is run from a computer, unless they are embedded in the file (up to about 16 MB).

## 15. Open questions

- Which national teams should be in the first list?
- Which names, costumes and monsters should the boss keepers have? Design them together using the boss table.
- Later idea: a 2-player mode where one kicks and one keeps.

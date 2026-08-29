# CAIRO ZERO

A browser first-person shooter set in the back streets, market and rooftops of
a fictional Old Cairo. Free-for-all against three bots, first to 10 kills.

Runs on desktop, iPhone and Android. No install, no backend, no build step.

---

## Play

**Desktop**

| Key | Action |
| --- | --- |
| W A S D | Move |
| Shift | Run |
| Space | Jump |
| Ctrl or C | Crouch |
| Left click | Fire |
| Right click | Aim |
| R | Reload |
| Esc | Release the mouse and pause |

**Phone**

Turn the phone sideways. Left thumb anywhere on the left half is the movement
stick — push it to the edge to run. Right thumb drags to look. FIRE, AIM, JUMP,
CROUCH and RELOAD sit under your right hand.

The rifle reloads on its own when the magazine runs dry.

---

## Put it online with GitHub Pages

1. Make a new repository on GitHub, e.g. `cairo-zero`.
2. Upload **everything in this folder**, keeping the folder structure:
   `index.html`, the `js` folder, the `css` folder and `.nojekyll`.
   On the GitHub upload page you can drag the whole folder in at once.
3. Repository → **Settings** → **Pages** → Source: *Deploy from a branch*,
   Branch: `main`, folder: `/ (root)`. Save.
4. Wait a minute, then open `https://<your-username>.github.io/cairo-zero/`.

`.nojekyll` is a required empty file. Without it GitHub ignores some folders.

**One-file version.** `cairo-zero-standalone.html` is the entire game in a
single file. Upload just that one file if you'd rather not deal with folders,
or open it straight off your phone to try it. It is generated from the source
in `js/` by running `node build.js`, so edit the modules, not the bundle.

---

## What's in here

```
index.html            the page: canvas, menus, HUD, touch buttons
css/style.css         all styling
js/config.js          every tuning number in one place
js/main.js            boot and wiring
js/core/              game loop, settings, input, audio, match rules, maths
js/world/             the map, collision, navigation
js/player/            player movement and health
js/weapons/           the rifle
js/ai/                enemy bots
js/render/            scene building, models, effects
js/controls/          desktop and touch schemes
js/ui/                screens and HUD
js/net/               message shapes for future multiplayer (unused)
build.js              makes the single-file version
```

Three.js is downloaded from a CDN when the page opens. If one CDN is
unreachable the next is tried automatically.

---

## Changing how it plays

Almost everything worth tuning lives in `js/config.js`:

- `match.killTarget` — kills needed to win (10)
- `match.botCount` — number of bots (3)
- `bot.accuracy`, `bot.reactionTime`, `bot.damage` — how hard the bots are
- `weapon.damage`, `weapon.rpm`, `weapon.magSize` — how the rifle feels
- `player.walkSpeed`, `player.runSpeed`, `player.jumpSpeed` — movement

The map is data, not a model file. `js/world/mapdata.js` builds Old Cairo out
of boxes with small helpers (`building`, `stairs`, `plank`, `stall`, `car`).
Add a building and the collision, line of sight, lighting and AI navigation all
pick it up automatically.

If you move things around, keep two rules in mind: don't park a car in a 3m
alley (it seals it), and any new roof access needs an `opening()` cutting the
parapet, or players will hit an invisible wall.

---

## Multiplayer later

Version 1 is single player, but nothing is built in a way that blocks
multiplayer. The player is plain state driven by an input object, the bots run
on the same interfaces, and `js/net/netstate.js` already defines the command and
snapshot shapes plus interpolation. Adding a server means feeding those inputs
from the network instead of the keyboard.

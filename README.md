# CAIRO ZERO

A browser first-person shooter set in the back streets, market and rooftops of
a fictional Old Cairo. Free-for-all against three bots, first to 10 kills. Three weapons.

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
| 1 2 3, Q, scroll | Change weapon |
| Esc | Release the mouse and pause |

**Phone**

Turn the phone sideways. Left thumb anywhere on the left half is the movement
stick — push it to the edge to run. Right thumb drags to look. FIRE, AIM, JUMP,
CROUCH, RELOAD and SWAP sit under your right hand. You can slide your thumb
off FIRE and keep turning, so you don't have to choose between shooting and
looking.

Weapons reload on their own when the magazine runs dry.

## Weapons

| | Feel | Good for |
| --- | --- | --- |
| MK-7 RIFLE | 30 rounds, steady, accurate | Everything, especially rooftops |
| HORUS SMG | 40 rounds, very fast, sprays | Alleys and market stalls |
| SAQR-12 | 6 shells, 9 pellets a shot | Corners, doorways, point blank |

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
- `weapons[]` — one entry per weapon: `damage`, `rpm`, `magSize`, `pellets`, spread and recoil
- `aimAssist.touchPull` — how much the crosshair helps on a phone; set to 0 to turn it off
- `adaptive.targetFps` — the frame rate the game tries to hold by lowering resolution
- `player.walkSpeed`, `player.runSpeed`, `player.jumpSpeed` — movement

The map is data, not a model file. `js/world/mapdata.js` builds Old Cairo out
of boxes with small helpers (`building`, `stairs`, `plank`, `stall`, `car`).
Add a building and the collision, line of sight, lighting and AI navigation all
pick it up automatically.

If you move things around, keep two rules in mind: don't park a car in a 3m
alley (it seals it), and any new roof access needs an `opening()` cutting the
parapet, or players will hit an invisible wall.

---

## Graphics

The whole city is still a single draw call, so detail is nearly free while
pixels are not. Buildings have windows, shutters, sills, awnings and doorways;
roofs have satellite dishes and washing lines; the streets have kerbs, shop
signs, palms and a string of lamps over the market. Sunset direction is baked
into the geometry as a warm/cool tint, there is a glow and cloud bands around
the sun, dust hangs in the air, and characters cast a soft contact shadow.

All of the decoration is non-colliding, so it changes how the map looks without
changing how it plays. LOW graphics skips the decoration entirely.

## Performance

Phones render at a lower resolution than the screen's full pixel density, and
if the frame rate drops below about 50 the game quietly lowers it further
rather than stuttering. If it still feels heavy, switch Graphics to LOW in
Settings — that cuts the resolution and the draw distance together.

## Aiming help

On a phone the crosshair slows slightly and pulls gently towards an enemy it is
already nearly on, but only one you can actually see, and it never fires for
you. Desktop gets a much weaker version. Both are in `aimAssist` in
`js/config.js`.

## Multiplayer later

Version 1 is single player, but nothing is built in a way that blocks
multiplayer. The player is plain state driven by an input object, the bots run
on the same interfaces, and `js/net/netstate.js` already defines the command and
snapshot shapes plus interpolation. Adding a server means feeding those inputs
from the network instead of the keyboard.

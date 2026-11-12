# Thin Air

A single-file flight combat game (Three.js r128). Everything is in `index.html`; the only other file it loads is the menu music (`tunetank-epic-warrior-music-413439.mp3`).

To play, open the GitHub Pages link, or serve this folder locally and open it in Chrome, Edge or Firefox:

```
npx serve .
```

Opening `index.html` straight from disk also works; if the browser will not load the music file that way, the game falls back to its built-in synth track.

## Operation Iron Rail (mission 18, Expert)

A train is carrying a mobile missile launcher across the Thar desert on the far side of the border. Cross the border, find the train, and destroy the launcher wagon before it reaches its launch point and fires. Two SAM wagons ride with the train, three waves of fighters scramble from an enemy air base, and an AI wingman (SHADOW 2) flies with you.

### How to start it

1. Hangar → **Single player** → mission list → **Operation Iron Rail** (the last card, Expert badge).
2. Pick the **Rafale** or the **Su-30MKI**. Your wingman flies the other one.
3. Optional: **Settings → Difficulty** (Easy / Normal / Hard). This applies to every mission in the game.
4. **Start sortie**.

### Controls (desktop)

| Key | What it does |
| --- | --- |
| A / D, W / S or arrows | Bank, pitch |
| Mouse wheel, V | Throttle, airbrake |
| T | Designate the next ground target (the launcher is designated for you when the train is found) |
| L | Fire a laser-guided missile at the designated target, 1.5 to 10 km. It follows the moving train |
| Space, G | Drop a bomb; switch CCIP / CCRP |
| M or left click | Air-to-air missile, once the bandit reads LOCK |
| X | Gun |
| Z or right click | Flares and chaff |
| K | Targeting pod on / off |
| **1** | Wingman: engage my target (your locked bandit, or the one nearest your nose) |
| **2** | Wingman: cover me (he takes whoever is hunting you) |
| **3** | Wingman: attack the SAM wagons (he fires his own laser missiles) |
| **4** | Wingman: rejoin formation |
| P or Esc | Pause; the pause screen shows the objectives log |

On a phone the **WING** button (lower left, where RADIO sits in multiplayer) opens the same four orders.

### What happens, in order

1. You start about 12 km south of the border. Red and white posts mark it on the ground; it is the red line on the map.
2. **Crossing the border** shows `ENTERING HOSTILE AIRSPACE` and starts the fighter waves: wave 1 after 30 s, each later wave 60 s after the previous one is shot down (or 90 s after it appeared, whichever is first). They take off from the air base in the north-west.
3. An amber **INTEL** arrow and a dashed circle on the map give a rough fix on the train, refreshed every 25 s. Within 12 km you see it: the launcher wagon gets a red box and is designated.
4. **SAM wagons**: each needs 3 s of unbroken tracking to lock, then fires. The warning tone is slow while a radar is acquiring you and fast once it is locked. They need line of sight: fly below 60 m, or put a dune or outcrop between you and the train, and the lock breaks; a missile already in the air goes dumb about a second later.
5. **Kill the launcher**: about two laser-missile hits or two direct bomb hits. Kill it before the train is half-way for a bonus.
6. Killing the **locomotive** stops the train, but the crew then sets up where it stands (90 s). At the launch point they need 60 s. The countdown shows on the HUD; at zero the mission fails.
7. **RTB**: fly back south over the border to win.

### Play-test checklist

- [ ] The mission card shows in the list with an Expert badge and `18` missions in the menu text.
- [ ] Briefing text and the four facts read sensibly for both the Rafale and the Su-30MKI.
- [ ] The railway, the border posts, the air base and the launch pad are visible and sit on the ground (no floating or buried track).
- [ ] The train moves smoothly along the track; wagons stay on the rails through the curves.
- [ ] `ENTERING HOSTILE AIRSPACE` appears at the border; `ENEMY FIGHTERS SCRAMBLING – WAVE 1/3` about 30 s later.
- [ ] Fighters climb out from the air-base runway and then come for you; the second jet of each pair goes for the wingman.
- [ ] RWR: slow tone and `RADAR SEARCH`, then fast tone and `SAM LOCK`, then the usual missile alarm. Dropping behind a dune silences it.
- [ ] Keys 1 to 4: the wingman answers on the radio and does what he was told. Key 3 gets a laser missile fired at a SAM wagon.
- [ ] T / L on the launcher: the missile follows the moving wagon; the first hit shows a damage percentage, the second destroys it with a chain of explosions.
- [ ] Letting the train arrive starts the 60 s countdown and the canister rises; at zero the mission fails.
- [ ] Killing the locomotive stops the train and starts the 90 s countdown.
- [ ] After the kill, crossing back south wins; the end screen shows time, kills by type, SAM wagons, wingman, damage and rank.
- [ ] Easy and Hard feel different (train speed, SAM lock time, wave timing, enemy turn rate).
- [ ] Phone: the WING button and its panel do not overlap the stick or the fire buttons, in both stick and tilt mode.
- [ ] Other desert missions (SEAD, tanker strike, night dogfight) no longer show the railway or the train.

## Tweaking the config

All tunables are in one JSON block near the top of `index.html`:

```html
<script type="application/json" id="gameConfig"> { ... } </script>
```

Edit a value, save, reload the page. If the block is not valid JSON the game logs a warning in the browser console and uses its built-in defaults (the same numbers), so a typo never breaks the game. You only need to list the values you want to change.

### `difficulty` (game-wide, all 18 missions)

Three presets: `easy`, `normal`, `hard`. Each value is a multiplier; `1` is the game as it was before.

| Key | Effect |
| --- | --- |
| `enemyTurn` | How hard enemy fighters turn |
| `enemyFireDelay` | Time between their missile shots (higher = they shoot less often) |
| `enemyFlareSkill` | Chance an enemy's flares beat your missile |
| `playerDecoy` | Chance your flares and chaff beat their missile |
| `samReload` | Time between SAM launches |
| `samRange` | SAM range |
| `aaaError` | AAA aiming error (higher = they miss more) |

### `ironRail`

`ironRail.normal` holds the full mission. `ironRail.easy` and `ironRail.hard` hold only the values that differ; they are merged over `normal` (arrays item by item, so a preset can change just one wave's timing).

| Key | Meaning |
| --- | --- |
| `train.speedKmh` | Train speed. With `startDistanceKm` this sets the mission length: 16 km at 80 km/h is 12 minutes |
| `train.startDistanceKm` | Distance from the launch point at the start (the line is about 23.6 km long) |
| `train.launchCountdown` | Seconds from arrival at the launch point to launch |
| `train.stoppedCountdown` | Seconds from the train stopping (locomotive destroyed) to launch |
| `train.stopDecel` | Braking, m/s², once the locomotive is destroyed |
| `train.detectRangeKm` | How close you must get before the train is found and marked |
| `train.scale` | How many times bigger than life the train (and its track) is drawn and hit, so it reads from the air. `1` is true size (wagons 3 m wide); the default is `4` (maximum `6`) |
| `train.wagons` | The train, front to back. `kind` is `loco`, `cargo`, `sam` or `nuke`; `len` in metres; `hp` hit points. It needs one `loco` and one `nuke` |
| `sam.rangeKm`, `sam.lockTime`, `sam.missiles`, `sam.reload` | Range, seconds to lock, missiles per wagon, seconds between shots |
| `sam.minAlt`, `sam.maxAlt` | Height above ground below which, and altitude above which, the radar cannot see you |
| `sam.missileSpeed` | Launch speed of the SAM, m/s |
| `waves[]` | Each wave: `types` (`f16`, `jf17`, `j10`), then `delay` for the first wave (seconds after you cross the border) or `afterDead` / `maxWait` for later ones |
| `enemyAI.<type>` | Per-type skill: `mslRange` (how far out it shoots), `aggr` (how often), `turn`, `flare` |
| `loadouts.rafale`, `loadouts.su30` | Your `msl`, `bombs`, `agm` (laser missiles), `gun` rounds and `flares` |
| `wingman` | `name`, `msl` (air-to-air missiles) and `bombs` (the number of laser missiles he carries for the SAM wagons) |
| `damage` | `bombDirect` / `agmDirect`: damage of a direct hit; `bombSplashR` / `agmSplashR`: splash radius in metres (damage falls off to zero at the edge) |
| `score` | `base` for a win, `halfwayBonus`, `perKill`, `perSam`, `wingmanAlive`, minus `damagePenalty` per % of hull lost and `timePenaltyPerSec` for each second over `parSeconds`. `rankS` / `rankA` / `rankB` are the score thresholds; below B is C |

Examples:

- A shorter mission: `"train": { "startDistanceKm": 10 }` (7.5 minutes at 80 km/h).
- A tougher launcher: raise the `nuke` wagon's `hp` from 180 to 260 (three laser missiles).
- A third SAM wagon: add `{ "kind": "sam", "len": 16, "hp": 90 }` to `wagons`.

### Swapping the placeholder models

The train, the air base and the launch pad are simple box models built by one function each in `index.html`:

`PH_LOCO(len)`, `PH_CARGO(len, i)`, `PH_SAM_WAGON(len)`, `PH_NUKE_WAGON(len)`, `PH_WRECK(len)`, `PH_AIRBASE(origin)`, `PH_LAUNCHPAD(origin)`

To use a real model, change the function so it returns your own `THREE.Object3D`:

- Wagons are in local space: nose toward **−z**, **y = 0** on top of the rail, about `len` metres long and 3 m wide.
- `PH_SAM_WAGON` must return `{ g, yaw, pitch, radar }`: the whole wagon, the turret that turns toward you, the tubes that tilt, and the dish that spins.
- `PH_NUKE_WAGON` must set `g.userData.er` to the group that tilts up during the countdown (hinged at the back of the wagon).
- `PH_AIRBASE` and `PH_LAUNCHPAD` are placed in world space at `origin` (`{x, z}`); the ground is levelled for about 850 m around each.

## Tests

The rail geometry, the train's timing, the countdowns, the SAM state machine, the wave timing, hit damage and the config merge are covered by a headless test that runs the real code out of `index.html` with Three.js stubbed:

```
node tests/ironrail.test.js index.html
```

Run it after changing the config block. Some checks assume the default Normal values (16 km, 80 km/h, 60 s and 90 s countdowns, the default wagon list), so they will fail if you change those; the rest still tell you the block parses and merges.

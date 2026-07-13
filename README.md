# Pixel Racer

A vertical car-racing avoid-em-up. Dodge the traffic, survive as long as you can.

**[▶ Play it](https://teal-gumdrop-bf945d.netlify.app/)**

---

## What it is

One `index.html` file. No build step, no `npm install`, no external assets — every car,
lane marking and pixel is drawn with canvas paths. Download the file, open it, and it runs.

- **Delta-time game loop** built on `requestAnimationFrame`, so it plays identically on a
  60 Hz phone and a 144 Hz monitor. The delta is clamped, so a backgrounded tab can't
  teleport enemies through you on return.
- **AABB collision** with a slightly inset hitbox — a near-miss that *looks* like a miss
  is treated as one.
- **Keyboard and touch.** Arrow keys or A/D on desktop; on mobile, the screen splits down
  the middle — touch the left half to steer left, the right half to steer right. Touches are
  tracked by identifier, so dragging a finger across the midline flips direction correctly.
- **Difficulty ramps with score,** capped so it stays playable rather than becoming a wall.
- **High score** persisted in `localStorage`, guarded in `try/catch` for restricted WebViews.
- **Locked to 9:16 portrait** and scaled by CSS — ready to wrap with Capacitor or Cordova.

## Controls

| | |
|---|---|
| Desktop | `←` `→` or `A` `D` |
| Mobile | Touch the left or right half of the screen |
| Start / restart | Tap, click, or press `Space` |

## Run it locally

```bash
git clone https://github.com/Elomami1976/pixel-racer.git
cd pixel-racer
```

Then open `index.html` in a browser. That's the whole setup.

## Sound

The game ships with an `AudioController` wired to three placeholder paths. Drop your own
files in and they'll play with no code changes:

```
assets/start.mp3    → on start
assets/score.mp3    → on passing a car
assets/crash.mp3    → on collision
```

Every `play()` call is wrapped, so a missing file or a blocked autoplay policy can never
break the game loop.

---

## How it was built

Written end to end by **[Vexi](https://github.com/Elomami1976/vexi)** — an open-source,
bring-your-own-key AI coding agent that runs in your terminal.

The approach that worked, after a few that didn't:

1. **A precise spec file** rather than a chat prompt. Mechanics, collision method, control
   scheme, and UI strings all written down before a line of code was requested.
2. **Four stages, not one shot.** Asking for a complete game in a single response runs into
   the model's output limit and truncates mid-function. Splitting it — scaffold, then input
   and state, then rendering, then the loop — kept every stage well inside the ceiling.
3. **Verification after every write.** Each stage ended by checking the file actually existed
   and had grown. An agent that reports success on work it never did is worse than one that
   errors out loudly.

Nothing touched the filesystem without an explicit confirmation, and the whole session is
recorded — `vexi replay --export` turns it into a standalone HTML you can step through.

## License

MIT
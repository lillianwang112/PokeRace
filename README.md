# ⚡ PokéRace — Typing Race Adventure

> **Gotta Type 'Em All.** Race Pokémon. Study anything. Get faster.

PokéRace is a single-file, zero-install typing race game that turns your own study material into a Pokémon-themed speed challenge. Drop in lecture notes, import a textbook chapter, or paste anything — then race against bot opponents in a glowing, NitroType-style arena while your WPM climbs and your Pokémon levels up.

---

## 🎮 What It Is

A fully self-contained `index.html` — no build step, no server, no npm install. Open it in a browser and you're racing. Everything from the physics of the race track to the XP leveling system to the word-by-word typing engine lives in one file under ~3,700 lines of React + CSS.

It's also a **study tool**. You upload your own text content (lecture notes, papers, vocabulary lists), and the game generates race passages from it. Your fingers learn the material while your brain competes.

---

## ✨ Features

### The Race Arena
- **Live race track** with animated road dashes, scrolling lane markings, and a crowd strip at the top
- **Pixel-art environment** — triangle+stem trees lining both grass strips, tumbling autumn leaves (`❋ ✦ ✿`) drifting across the track
- **Canvas trail system** — persistent glow trails behind every racer rendered at 60fps
- **Type-colored everything** — each Pokémon's type (Fire, Water, Electric, etc.) tints their lane border, progress bar, glow, and particle trail
- **Camera tilt** — the arena tilts into a subtle 3D perspective at 60% race completion
- **⚠ OVERTAKEN warning** — red flash badge when a bot passes you

### Typing Engine
- **Word-by-word input** — advance on Space, blocked if the current word has errors
- **Character-level feedback** — green correct chars, red wrong chars, animated cursor
- **3-line sliding viewport** — passage scrolls smoothly as you type, never jumps
- **Shake + sound** on wrong word submission
- **Combo system** — consecutive correct chars build your combo multiplier with milestone banners at 10×, 25×, 50×, 100×

### Pokémon & Passives
20 playable Pokémon, each with a unique passive ability. You unlock them by leveling up.

| Pokémon | Unlock | Passive |
|---|---|---|
| ⚡ Pikachu | Lv 0 | Starter — balanced |
| 🔥 Charmander | Lv 0 | Starter |
| 💧 Squirtle | Lv 0 | Starter |
| 🌿 Bulbasaur | Lv 0 | Starter |
| 🦊 Eevee | Lv 3 | +5% speed |
| 🎤 Jigglypuff | Lv 5 | Your errors slow bots |
| 👻 Gengar | Lv 8 | +10% speed |
| 🥄 Alakazam | Lv 10 | Errors don't reset combo |
| 🌩️ Jolteon | Lv 12 | Nitro charges 50% faster |
| 🦕 Lapras | Lv 14 | Absorb 2 bot attacks |
| 🐲 Dragonite | Lv 16 | +25% speed in final stretch |
| 💪 Machamp | Lv 18 | Speed burst after fixing errors |
| 🔮 Mewtwo | Lv 20 | +15% speed |
| 😴 Snorlax | Lv 22 | Slow start, accelerating WPM |
| 🌸 Espeon | Lv 24 | Perfect word streaks → bonus XP |
| 🐉 Rayquaza | Lv 26 | +20% speed |
| 🦎 Charizard | Lv 28 | Combo streaks trigger Nitro |
| 🐢 Blastoise | Lv 30 | Errors don't break combo at 90%+ acc |
| 🌋 Typhlosion | Lv 33 | Once per race: 5-sec Fire Spin mega-boost |
| ✨ Arceus | Lv 40 | +25% speed + ALL passives |

### Nitro Boost
- Charges as you type correct words (longer words = bigger charge)
- Press **Tab** to fire — launches a type-specific attack animation (⚡ electric bolt, 🔥 fire ring, 💧 water ripple, 🌿 leaf burst, 🌀 psychic spiral)
- Doubles your race speed and triggers a cinematic lane flash
- Three difficulty-scaled Nitro uses per race unlock the **Nitro Master** achievement

### Progression
- **XP & leveling** — earn XP at the end of every race based on WPM × accuracy × difficulty multiplier
- **Subject mastery** — separate XP tracks per lecture topic
- **Daily challenge** — a new seeded passage every day, worth 2× XP
- **Race log** — full history with WPM sparkline chart
- **26 achievements** — from First Catch (complete one race) to Speed Demon (120+ WPM) to Gotta Type 'Em All (race with all 4 starters)
- **3-day and 7-day streak tracking**

### Study Content System
Upload your own material three ways:
- **Paste text** directly
- **Upload a .txt file**
- **Drag & drop** onto the upload zone

The game splits your content into race-length passages automatically. Organize into named topics ("Biochemistry Midterm", "Spanish Vocab") and select one or multiple topics before each race. Passages scale to difficulty (Easy caps at 120 chars, Hard has no cap).

---

## 🕹️ Controls

| Key | Action |
|---|---|
| **Type normally** | Input characters |
| **Space** | Submit current word (blocked if wrong) |
| **Tab** | Fire Nitro Boost |
| **Escape** | Restart race from countdown |

---

## 🚀 Getting Started

1. Download `index.html`
2. Open it in any modern browser
3. Pick a starter (Pikachu, Charmander, Squirtle, or Bulbasaur)
4. Add some study content under **Lectures**
5. Hit **Race** and start typing

That's it. No account. No server. No nonsense.

---

## 🏗️ Tech Stack

Everything runs in the browser with zero build tooling:

- **React 18** (loaded from CDN via UMD build)
- **Babel Standalone** for JSX transpilation in-browser
- **Tailwind CSS** (CDN, configured inline)
- **Canvas API** for the 60fps racer trail system
- **Web Audio API** for all sound effects (synthesized — no audio files)
- **PokéAPI** (`pokeapi.co`) for sprite images, with emoji fallbacks if offline
- **localStorage** for all persistence (races, XP, achievements, lectures)
- **Google Fonts** — Fredoka One, Space Mono, JetBrains Mono, Nunito, Boogaloo

---

## 💾 Data & Privacy

Everything is stored locally in your browser's `localStorage`. Nothing is ever sent to a server. The only external calls are:
- Google Fonts (CSS + font files)
- Tailwind CDN
- PokéAPI (sprite images only — read-only, no auth)

You can export a full JSON backup of all your data (races, lectures, XP, achievements) from the Settings screen and re-import it on any device.

---

## 🎨 Design System

The UI is built on a dark glass aesthetic with a custom CSS design system:

- **Colors** — `--bg-void (#0a0a14)` base, type-color accents per Pokémon
- **Fonts** — Fredoka One for display/numbers, Space Mono for HUD/badges, JetBrains Mono for the typing passage, Nunito for body copy
- **Glass cards** — `backdrop-filter: blur(20px)` with subtle inset highlights
- **Light mode** — full theme toggle with warm yellow/blue tones
- **Responsive** — mobile keyboard docks to the bottom of the screen on small viewports

---

## 📁 Structure

It's one file. Here's how it's organized internally:

```
index.html
├── <style>
│   ├── CSS design system & variables
│   ├── Race arena & lane styles
│   ├── Typing zone styles
│   ├── HUD & combo system
│   └── Keyframe animations (30+)
└── <script type="text/babel">
    ├── Constants (POKEMON, BOT_POKEMON, ACHIEVEMENTS, DIFFICULTY)
    ├── Utilities (XP calc, streak, WPM, normalizeChar)
    ├── Audio engine (Web Audio API, synthesized SFX)
    ├── App / Context / Router
    ├── Header component
    ├── MainMenu component
    ├── RaceScreen component  ← the big one
    │   ├── Countdown phase
    │   ├── Racing phase (word engine + bot AI + nitro + canvas trail)
    │   └── Finished phase
    ├── ResultsScreen component
    ├── StatsScreen component
    ├── AchievementsScreen component
    ├── LectureManager component
    └── SettingsScreen component
```

---

## 🤝 Contributing

PRs welcome. Since it's a single file, the bar for "just open it and hack" is as low as it gets. A few ideas that would be great additions:

- Multiplayer via WebRTC or a lightweight WebSocket server
- More Pokémon (Gen 2+ roster)
- Custom passage difficulty analysis (syllable count, word frequency)
- Mobile swipe gestures
- Offline mode / PWA wrapper

---

## 📄 License

MIT. Do whatever you want with it. Just maybe don't sell it as "your" Pokémon game to Nintendo.

---

*Built with ⚡ and an unreasonable number of CSS keyframes.*

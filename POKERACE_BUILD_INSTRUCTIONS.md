# 🎮 PokéRace — Claude Code Build Instructions
## "Take what exists and make it legendary"

---

## 🔍 What You're Starting From

You have a working `index.html` single-file React app (Babel + CDN) with:
- ✅ Lecture upload system (paste text → stored in localStorage as named "topics")
- ✅ Pokémon selection (10 Pokémon with emoji, type colors, speed multipliers)
- ✅ Race screen with countdown, typing input, character-by-character checking
- ✅ WPM + accuracy calculation
- ✅ Bot racers with configurable speed multipliers
- ✅ Nitro boost mechanic (charges on correct keystrokes, depletes for speed burst)
- ✅ Combo counter, XP system, leveling
- ✅ Achievement system (16 achievements)
- ✅ Stats screen, Garage screen, Settings screen
- ✅ Web Audio API sound effects
- ✅ Confetti on race finish
- ✅ Dark/light theme toggle
- ✅ LocalStorage persistence with export/import

**The architecture is solid. Do NOT rewrite it from scratch.** Enhance, extend, and visually transform it.

---

## 🎯 The Core Rule: Lecture Material = The Typing Content

The typing passages MUST come from the user's uploaded lecture notes. This is the entire educational premise of the game. Ensure:

1. **The lecture upload flow is prominent and frictionless** — it should be the very first thing a new user is guided to do
2. **Passages are intelligently chunked** from lecture text:
   - Easy: single sentences (≤120 chars)
   - Medium: 2–3 connected sentences (≤350 chars)  
   - Hard: full paragraphs (up to 600 chars)
3. **Never fall back to generic filler words** — if no lecture is loaded, show an empty state that actively prompts the user to upload material, don't silently use placeholder text
4. **Topic tagging**: when a lecture is uploaded, auto-detect likely subject from keywords and tag it (e.g., "Biology", "History", "Coding") — show the topic tag during the race so students know what they're studying
5. **Smart passage selection**: avoid repeating the same passage in back-to-back races. Shuffle the pool each session.
6. **"Study Mode" race**: a special race variant where after finishing, the passage is shown again with key terms highlighted — reinforcing retention

---

## 🚀 Phase 1: Visual Overhaul (Highest Priority)

The current UI is functional but generic. Transform it into something that looks like a **premium esports game meets a Pokémon Stadium broadcast**.

### New Design Language

**Color System** — implement as CSS variables:
```css
--bg-void: #0a0a14;          /* deepest background */
--bg-arena: #0f0f1e;          /* main surfaces */
--bg-glass: rgba(255,255,255,0.04);
--border-dim: rgba(255,255,255,0.07);
--border-glow: rgba(255,255,255,0.15);
--accent-primary: #facc15;    /* Pokémon yellow */
--accent-electric: #38bdf8;   /* electric blue */
--text-bright: #f0f4ff;
--text-muted: #64748b;
```

**Typography** — replace Orbitron with a more distinctive combo:
- Headlines: `Boogaloo` or `Fredoka One` (chunky, playful, game-feel) — load from Google Fonts
- UI labels: `Space Mono` (techy, monospaced energy)  
- Typing input text: `JetBrains Mono` (keep, it's perfect for typing games)
- Body/descriptions: `Nunito` (friendly, readable)

**Background**: Replace the flat dark background with a layered arena atmosphere:
- Deepest layer: `radial-gradient` from `#1a0a2e` (purple-void) to `#0a0a14`
- Middle layer: subtle animated `repeating-linear-gradient` grid lines (like a stadium floor) at 3% opacity
- Top layer: faint floating particle dots (10–15 CSS-animated divs, slow drift, no JS libraries needed)

**Cards/Panels**: All glass cards should have:
- `background: rgba(255,255,255,0.03)`
- `border: 1px solid rgba(255,255,255,0.08)`
- `box-shadow: 0 0 30px rgba(0,0,0,0.5), inset 0 1px 0 rgba(255,255,255,0.05)`
- Hover state: border brightens to `rgba(255,255,255,0.15)` with a subtle color-matched glow

---

## 🏟️ Phase 2: Race Track — The Main Event

This is where the most work is needed. The current track is minimal. Build a **proper animated race arena**.

### Race Track Layout (top 40% of race screen)

```
┌─────────────────────────────────────────────────────────────┐
│  🏁 LAP 1    [PLAYER NAME]  ⚡ 67 WPM   [=====>----] 58%   │
│  ════════════════════════════════════════════════════════   │
│  LANE 1: [Pikachu sprite running] ══════>                   │
│  LANE 2: [Rattata sprite]  ═══>                             │
│  LANE 3: [Pidgey sprite]     ═════>                         │
│  LANE 4: [Bot sprite]  ══>                                  │
│  ════════════════════════════════════════════════════════   │
│  🏁 FINISH                                            🏁    │
└─────────────────────────────────────────────────────────────┘
```

**Implementation details:**
- Each lane is a `div` with `overflow: hidden` and a scrolling track texture (CSS background-image with dashes)
- Pokémon use their **actual PokeAPI sprites** (fetch from `https://pokeapi.co/api/v2/pokemon/{name}` → `sprites.front_default`)
  - Cache all sprites in a `Map` at app startup
  - Fallback to emoji if fetch fails (keep current emoji as fallback)
- Sprites should `animation: runAnimation 0.25s steps(2) infinite` (pixel-art style stepping)
- Position is set via `left: ${progress}%` with `transition: left 0.15s linear`
- Add **speed lines** (`position: absolute` horizontal dashes) that appear behind the player's sprite when WPM is above their average
- When a racer **finishes**, their lane briefly flashes their type color and shows a "FINISH!" badge

### Visual attack animations (when nitro fires):
- Player's Pokémon: emit a burst of type-colored sparks toward the nearest bot
- Use CSS `@keyframes` only — no canvas, no library
- Types → animations:
  - Electric: yellow zigzag bolt
  - Fire: orange expanding ring
  - Water: blue ripple wave
  - Grass: green leaf scatter
  - Ghost/Psychic: purple spiral pulse
  - Default: white starburst

---

## ⌨️ Phase 3: Typing Area — Make It Feel Amazing

The typing engine logic is already correct. Make the **presentation** world-class.

### Redesigned Typing Zone

Replace the current character-highlight approach with a **word-by-word system** (like NitroType):

```
[completed words dimmed] [CURRENT WORD highlighted] [upcoming words normal opacity]
                                    ^cursor
```

**Word states:**
- Completed correctly: `color: #22c55e` (green), slightly faded `opacity: 0.6`
- Current word being typed: `background: rgba(250,204,21,0.15)`, `border-bottom: 2px solid #facc15`, full brightness
- Current word with error: `background: rgba(239,68,68,0.15)`, `border-bottom: 2px solid #ef4444`
- Upcoming words: `color: #94a3b8`, normal size

**The input field:**
- Should be **invisible to the user** — they just see the passage text being highlighted
- Use a hidden `<textarea>` that captures keystrokes
- Keep the textarea focused at all times during race (re-focus on any click in race area)
- On mobile: show a visible input bar at the bottom

**Passage display box:**
- Fixed height, `overflow: hidden` (never scroll — the text should auto-advance)
- When a line is completed, animate the text block sliding up to reveal the next line
- Show exactly 3 lines of text at all times
- The current word in the middle line is always in view

**Error handling:**
- If user types a wrong character: word turns red, shake animation, error sound
- User CANNOT advance past an incorrect word — they must backspace to fix it (NitroType behavior)
- Show a small red error counter below the typing area: `✗ 3 errors`

---

## 📊 Phase 4: HUD Upgrades

### Live Race HUD (between track and typing area)

```
┌──────────┬──────────┬──────────┬──────────────────────────┐
│  72 WPM  │  98.3%   │  🔥 x14  │  [NITRO ████████░░] 80% │
│  current │ accuracy │  combo   │  Press SPACE to boost    │
└──────────┴──────────┴──────────┴──────────────────────────┘
```

- WPM counter: updates every 2 seconds, large `font: Boogaloo 3rem`, color shifts: white → yellow (>60 WPM) → orange (>80) → red (>100)
- Combo badge: bounces on each increment, glows with type color
- Nitro bar: gradient fill that pulses when full, depletes visibly when used
- **New: "Topic Badge"** — small pill in the corner showing which lecture subject is being typed: `📚 Biology Ch. 3`

### Combo Milestone Banners
At combos of 10, 25, 50, 100+, briefly display a full-width banner:
- 10: "Quick Attack!" (white)
- 25: "Thunderbolt!" (yellow flash)
- 50: "HYPER BEAM!!" (orange + screen shake)
- 100: "LEGENDARY!!! ✨" (rainbow gradient + confetti burst)

---

## 🎓 Phase 5: Lecture Management — Full Overhaul

The current manage screen is bare-bones. Build a proper **Study Deck Manager**.

### New Lecture Manager UI

**Upload options (3 methods):**
1. **Paste text** — large textarea (existing)
2. **Upload .txt or .pdf file** — drag & drop zone with `FileReader` API
3. **Quick import** — paste a URL (fetch via CORS proxy like `https://api.allorigins.win/get?url=` for simple pages)

**Per-lecture options:**
- Custom name + subject tag (auto-suggested, user can edit)
- Word count + estimated race time shown
- Preview: show first 3 sentences
- "Practice Mode" button: jump straight into a solo race with just this lecture
- Delete button

**Lecture Stats** (shown per card):
- Times practiced
- Best WPM on this material
- Accuracy trend (last 5 races on this topic: sparkline using CSS bars)

**Onboarding empty state:**
When no lectures are uploaded, show a large illustrated empty state:
- Big Pokémon professor art (use an SVG illustration — a generic professor silhouette with a clipboard)
- Text: "Professor Oak wants you to study! Upload your lecture notes to begin your journey."
- Giant "Upload First Lecture" CTA button

---

## 🏆 Phase 6: Progression & Rewards

### Expanded Pokémon Roster

Upgrade from emoji-only to **real sprites + real data via PokéAPI**.

Add 10 more Pokémon (total 20) with meaningful passive abilities tied to typing:

| Pokémon | Unlock Level | Passive Ability |
|---------|-------------|-----------------|
| Pikachu | 0 | Starter — no bonus |
| Charmander | 0 | Starter — no bonus |
| Squirtle | 0 | Starter — no bonus |
| Bulbasaur | 0 | Starter — no bonus |
| Eevee | 3 | +5% WPM speed |
| Jigglypuff | 5 | Errors slow bots too |
| Gengar | 8 | +10% speed |
| Alakazam | 10 | **NEW** — Accuracy bonus: errors don't reset combo |
| Jolteon | 12 | **NEW** — Nitro charges 50% faster |
| Lapras | 14 | **NEW** — HP: takes 2 bot attacks before slowing |
| Dragonite | 16 | **NEW** — Last 20% of race: +25% speed |
| Machamp | 18 | **NEW** — Error recovery: 0.5s speed burst after fixing error |
| Mewtwo | 20 | +15% speed |
| Snorlax | 22 | **NEW** — Slow start but WPM accelerates over time |
| Espeon | 24 | **NEW** — Perfect word streaks give XP multiplier |
| Rayquaza | 26 | +20% speed |
| Charizard | 28 | **NEW** — Fire: combo streaks give speed surges |
| Blastoise | 30 | **NEW** — Water: errors don't break combo below 90% accuracy |
| Typhlosion | 33 | **NEW** — Fire Spin: once per race, 5-second speed burn mode |
| Arceus | 40 | +25% speed, all abilities |

Fetch each Pokémon's sprite from PokeAPI on garage screen load. Store in sessionStorage to avoid re-fetching.

### Leveling & XP Rework

Keep the existing XP formula but add:
- **Subject Mastery**: separate XP bar per lecture topic — level up your "Biology" or "History" skill
- **Daily Challenge**: one special passage marked ⭐ each day — completing it gives 2× XP
- **Streak Bonus**: display the current streak prominently on home screen with a flame animation

### Expanded Achievements (add 10 more):

| ID | Name | Condition |
|----|------|-----------|
| study_first | Scholar | Complete a race with lecture material |
| all_starters | Gotta Type 'Em All | Race with all 4 starters |
| no_errors | Flawless | Finish with 0 errors |
| speed_demon | Speed Demon | Hit 120+ WPM |
| lecture_10 | Bookworm | Practice 10 different lecture passages |
| daily_7 | Daily Grind | Complete daily challenge 7 days in a row |
| mastery | Subject Master | Reach level 5 mastery in any subject |
| comeback | Comeback Kid | Win a race after being in last place at 50% |
| nitro_master | Nitro Master | Use 3 nitros in a single race |
| marathon | Marathon | Type 10,000 total characters |

---

## 🖥️ Phase 7: Screen-by-Screen Polish

### Home Screen
- Add a **live "Last Race" replay strip** — animated bar showing your WPM over time from the last race
- Show today's **Daily Challenge** passage title with a glowing border
- **Trainer Card preview** in the corner: avatar (Pokémon sprite), name, level, win rate
- Animated background: slow-scrolling silhouettes of Pokémon running across the bottom (CSS keyframes, pure SVG shapes, no images needed)

### Race Results Screen
Completely rebuild this as a **post-match stats card**:

```
┌─────────────────────────────────────────────┐
│           🏆  1ST PLACE!  🏆                 │
│    [Pikachu sprite — victory pose bounce]    │
│                                             │
│    72 WPM        98%          0:47          │
│   Personal       Accuracy     Time          │
│   Best! ▲                                   │
│                                             │
│   🔥 Best Combo: 34    ⚡ Nitros Used: 2    │
│   📚 Topic: Biology Ch. 3                   │
│                                             │
│   [XP BAR ANIMATING: +148 XP ———→ ▓▓▓▓░░] │
│   Level 7 ──────────────────── Level 8      │
│                                             │
│   [Race Again]  [Change Topic]  [Garage]    │
└─────────────────────────────────────────────┘
```

- Animate each stat counting up from 0 (staggered, 200ms apart)
- If it's a personal best WPM, show a golden "NEW RECORD! 🎉" badge that pops in
- Show the passage content with difficult words highlighted in yellow (words that took >2 attempts)
- "Retry this passage" button — great for studying

### Garage Screen
Transform into a proper **Pokémon collection grid**:
- Cards with real PokeAPI sprites (loading skeleton while fetching)
- Locked Pokémon shown as silhouettes with "Unlock at Level X" badge
- Selected Pokémon has an animated glow border in their type color
- Clicking a Pokémon shows a **detail panel**:
  - Name, type, description
  - Passive ability with icon
  - Your stats with this Pokémon (races, best WPM)
  - "Select for Race" button

---

## 🔧 Phase 8: Technical Quality

### Performance
- Debounce the WPM calculation to every 500ms (not every keystroke)
- Memoize passage chunking with `useMemo` (only recompute when lecture data changes)
- Use `requestAnimationFrame` for the race position updates instead of `setInterval`
- Lazy-load PokeAPI sprite fetches (only fetch what's visible)

### Typing Engine Hardening
- Keep the existing `normalizeChar` function — it's important for smart quotes
- Add normalization for: `…` → `...`, `–` → `-`, `—` → `-`, `"` → `"`
- Handle **auto-advance on word completion**: when the current word in the passage is finished (user types the full word + space), advance automatically — don't require an exact space match if they're mid-word in the next

### Error Prevention
- If the user's browser blocks AudioContext (autoplay policy), catch silently and disable sound
- If PokeAPI is unavailable, fall back to emoji immediately (don't show broken images)
- If localStorage is full, silently prune the oldest 20 race records before saving

### Mobile Experience
- On screen width < 768px:
  - Stack the race track vertically (show only player + top bot)
  - Show a visible keyboard input bar at the bottom of screen
  - Reduce font sizes proportionally
  - Keep all core gameplay functional

---

## 📋 Implementation Order

Build in this exact sequence to always have a working game at each step:

1. **Visual overhaul** — CSS variables, new fonts, arena background, glass cards. Game still works.
2. **Race track rebuild** — new lane layout with PokeAPI sprites, progress bars, speed lines.
3. **Typing area redesign** — word-by-word highlighting, hidden input, error blocking.
4. **Lecture manager overhaul** — drag & drop upload, subject tags, onboarding empty state.
5. **HUD upgrade** — new stats bar, combo banners, topic badge.
6. **Pokémon expansion** — 10 new Pokémon with passive abilities, PokeAPI integration.
7. **Results screen rebuild** — animated stat reveal, personal best detection.
8. **Garage screen rebuild** — sprite grid, locked silhouettes, detail panel.
9. **Achievement expansion** — 10 new achievements.
10. **Polish pass** — particle background, mobile tweaks, error resilience.

---

## ⚡ Non-Negotiables

- **Lecture material is always the typing content.** No generic word lists. Ever.
- **Typing feel must be instantaneous.** Zero input lag. Test this first.
- **PokeAPI sprites must have emoji fallbacks.** Never show a broken image.
- **The game must remain a single `index.html` file.** No build system. CDN imports only.
- **Every screen transition should have at least a fade.** Nothing should snap in coldly.
- **The race should feel exciting even when alone.** Bot variety and commentary help.
- **After every race, the player should immediately know their topic coverage stats** — this is a study tool first, a game second.

---

## 🎨 Bonus: The One Thing That Makes It Unforgettable

Add a **"Professor's Commentary" system** — a small speech bubble from a Pokémon professor sprite (bottom corner) that:
- Appears at race end with a contextual comment based on performance:
  - Sub-40 WPM: "Keep practicing, trainer! Every great champion started somewhere."
  - 40–60 WPM: "Solid effort! Your typing speed is evolving!"
  - 60–80 WPM: "Impressive! You're becoming a real typing trainer!"
  - 80–100 WPM: "Outstanding! The Elite Four would be proud!"
  - 100+ WPM: "LEGENDARY! You've achieved what few trainers dare dream!"
- Occasionally references the subject being studied: "Those biology terms are tough — you handled them well!"
- Has a pixel-art speech bubble with a typing animation (characters appear one by one, 30ms each)

This single feature adds enormous personality and makes the educational angle feel intentional and warm.

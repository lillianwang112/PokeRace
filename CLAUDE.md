# PokéRace — Project Rules

## What This Is
Single-file React app (index.html, Babel + CDN). A Pokémon-themed typing race 
game where ALL typing content comes from user-uploaded lecture notes.

## Architecture Rules
- Single index.html file. No build system. CDN imports only.
- React 18 via unpkg, Tailwind via CDN, Babel standalone
- All state in localStorage (keys: pt_lectures, pt_races, pt_settings, etc.)
- PokeAPI for sprites: https://pokeapi.co/api/v2/pokemon/{name} — always have emoji fallback

## The One Unbreakable Rule
Typing passages ALWAYS come from uploaded lecture material. Never use generic 
word lists. If no lecture is loaded, show the upload prompt — don't silently 
substitute filler text.

## Current State
Working game with: lecture upload, typing engine, WPM/accuracy, bot racers, 
XP/leveling, 16 achievements, Nitro boost, dark/light theme.

## Full Build Plan
See BUILD_PLAN.md for phase-by-phase implementation spec.

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A single-file Pokemon Pokedex collection tracker built as a standalone HTML application. Users can click Pokemon cards to toggle collection status, which persists to localStorage.

## Architecture

**Single File Structure**: All HTML, CSS, and JavaScript are contained in `pokemon_pokedex_collection.html`.

**Key Components**:
- **Collection State**: Uses a `Set` stored in localStorage (`pokemonCollection` key) to track collected Pokemon IDs
- **Lazy Loading**: Pokemon grids load on-demand when generation sections are expanded (via `loadedGens` Set)
- **Sprite System**: Uses PokeAPI sprites with a three-tier fallback: HOME sprites → Official Artwork → Basic sprites
- **Form Variants**: Special forms (Mega, Alolan, Galarian, Hisuian, Paldean) use `formSpriteIds` mapping to PokeAPI form IDs

**Data Structure**: The `generations` array contains all Pokemon data organized by generation, with each Pokemon having:
- `id`: Number for base forms, string for variants (e.g., "3-mega", "26-alola")
- `name`: Display name
- `spriteId`: Optional PokeAPI sprite ID for variants

## Development

Open `pokemon_pokedex_collection.html` directly in a browser. No build step or server required.

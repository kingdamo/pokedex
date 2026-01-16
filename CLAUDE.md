# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A single-file Pokemon Pokedex collection tracker built as a standalone HTML application. Users can click Pokemon cards to toggle collection status, which persists to localStorage and can sync to the cloud.

## Architecture

**Single File Structure**: All HTML, CSS, and JavaScript are contained in `pokemon_pokedex_collection.html`.

## Features

### Collection Tracking
- Click any Pokemon card to toggle collected/uncollected status
- Collection stored in localStorage (`pokemonCollection` key) as a Set of Pokemon IDs
- Visual feedback: collected cards show green gradient background and checkmark badge
- Stats display shows total collected vs total Pokemon count

### Search & Filtering
- **Search box**: Filter Pokemon by name or ID (sticky at top when scrolling)
- **All/Missing buttons**: Toggle between showing all Pokemon or only uncollected ones
- Filters work across all generations simultaneously
- Generation sections auto-expand when matches are found

### Cloud Sync (JSONBin.io)
- **Sync button**: Save/load collection to cloud
- **Collection name**: Users enter a name (e.g., "damien") as sync key
- **Shared index**: Master index bin (`696a4116ae596e708fe08602`) maps collection names to data bins
- **Cross-device**: Same collection name on different devices syncs the same data
- **Conflict resolution**: Prompts user when cloud has newer data (load vs overwrite)

**localStorage keys for sync**:
- `pokemonCollectionName`: Current collection name
- `pokemonBinId`: Cached bin ID for current collection
- `pokemonLastSync`: Timestamp of last sync

**API Configuration**:
- `JSONBIN_API`: `https://api.jsonbin.io/v3`
- `JSONBIN_KEY`: Access key for API authentication
- `INDEX_BIN_ID`: Hardcoded shared index bin ID

### Lazy Loading
- Pokemon grids load on-demand when generation sections are expanded
- `loadedGens` Set tracks which generations have been rendered
- `loadGeneration(genId)` renders cards with 50ms delay for smooth UX
- After loading, re-applies active filters via `applyFiltersForGen(genId)`

### Print View
- **Print button**: Prepares collection for printing
- Loads all generations before printing
- Cards sized to Pokemon TCG dimensions (2.5 x 3.5 inches)
- Respects current filter (prints only visible cards)
- Shows loading overlay with progress and cancel option

### UI Components
- **Action bar**: Contains All, Missing, Sync, Print, Clear buttons
- **Toast notifications**: Success/error feedback for sync operations
- **Generation sections**: Collapsible accordion with collected counts
- **Form badges**: Color-coded labels for Mega, Alolan, Galarian, Hisuian, Paldean forms

## Data Structure

**`generations` array**: Pokemon data organized by generation, each with:
- `id`: Generation number (1-9)
- `name`: Display name (e.g., "Generation 1")
- `subtitle`: Game names (e.g., "Red/Blue/Yellow")
- `pokemon`: Array of Pokemon objects

**Pokemon object**:
- `id`: Number for base forms, string for variants (e.g., `3`, `"3-mega"`, `"26-alola"`)
- `name`: Display name
- `spriteId`: Optional PokeAPI sprite ID for variants
- `zaSprite`: Optional custom sprite path for special forms

## Sprite System

Three-tier fallback for Pokemon images:
1. **HOME sprites**: `raw.githubusercontent.com/.../home/{id}.png`
2. **Official Artwork**: `raw.githubusercontent.com/.../official-artwork/{id}.png`
3. **Basic sprites**: `raw.githubusercontent.com/.../sprites/pokemon/{id}.png`

Form variants use `spriteId` or `zaSprite` for correct image URLs.

## Key Functions

| Function | Purpose |
|----------|---------|
| `toggleCollection(id)` | Toggle Pokemon collected status |
| `saveCollection()` | Persist collection to localStorage |
| `loadCollection()` | Load collection from localStorage |
| `syncCollection()` | Sync with JSONBin.io cloud |
| `getIndex()` | Fetch master index of collection names |
| `updateIndex(index)` | Update master index with new collection |
| `setFilter(mode)` | Set filter to 'all' or 'missing' |
| `applyFilters()` | Apply search and filter to all generations |
| `applyFiltersForGen(genId)` | Apply filters to specific generation |
| `loadGeneration(genId)` | Render Pokemon cards for a generation |
| `printCollection()` | Prepare and trigger print view |
| `clearCollection()` | Clear all collected Pokemon |

## CSS Classes

| Class | Purpose |
|-------|---------|
| `.pokemon-card` | Base card styling |
| `.pokemon-card.collected` | Collected state styling |
| `.action-btn` | Action bar button styling |
| `.action-btn.active` | Active filter button |
| `.sync-toast` | Toast notification container |
| `.sync-toast.show` | Visible toast |
| `.sync-toast.success/.error` | Toast variants |
| `.generation-section` | Generation accordion container |
| `.pokemon-grid` | Grid container for cards |
| `.pokemon-grid.open` | Expanded grid |

## Development

Open `pokemon_pokedex_collection.html` directly in a browser. No build step or server required.

## Notes

- All Pokemon from Gen 1-9 included (~1200+ with forms)
- Form variants include Mega, Alolan, Galarian, Hisuian, Paldean, and special forms
- Print styles hide UI elements and format cards for TCG size

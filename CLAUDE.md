# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a 3kdb TinTin++ MUD client configuration repository for the 3Kingdoms MUD game. The codebase is built with a modular approach supporting multiple characters, guilds, professions, and gameplay automation.

## Essential Commands

### Starting the MUD Client
- `tt++` - Launch TinTin++ client (requires TinTin++ to be installed)
- Once in TinTin++, run: `#read 3k.tin` to load the character selection menu
- Press 1-4 to select and connect with a configured character

### Character Setup
- To play a character: `#read chars/charactername/charactername.tin`
- First-time setup: `setup_3kdb` (configure guild hpbar and game settings)
- `setup_3kdb_hpbar` - Configure guild-specific HP bar if available

### TinTin++ Update System
- `./tt_update` - TinTin++ beta updater script
- `./tt_update adi` - Archive current, download, and install new TinTin++ beta
- `./tt_update archive` - Archive current source and binary
- `./tt_update download` - Download latest beta source
- `./tt_update install` - Build and install downloaded source
- `./tt_update history` - Show archived binary history
- `./tt_update revert N` - Revert to archived binary number N

## Architecture Overview

### Core Structure
- **chars/**: Individual character configurations (one directory per character)
- **common/**: Shared scripts and configurations across all characters
- **modules/**: Self-contained script packages for specific functionality
- **scripts/**: Python utilities (Discord integration, 3KPI connector)
- **logs/**: Character logs organized by system username

### Character Loading Flow
1. Character .tin file sets `$user` and `$guild` variables
2. Loads `common/index.tin` which includes all shared functionality
3. `common/config.tin` handles session management and loads character-specific files
4. Guild-specific scripts loaded from `common/guilds/$guild/`
5. Character-specific files loaded from `chars/$user/`

### Key Variables
- `$user` - Character name (lowercase)
- `$guild` - Guild name for loading guild-specific scripts
- `$discordPost` - Enable/disable Discord webhook integration (0/1)

### Module System
Modules are self-contained packages in `modules/` directory:
- **mip/**: MUD Information Protocol - core data parsing
- **dmgtracker/**: Combat damage tracking and analysis
- **map/**: Mapping and navigation system
- **crafting/**: Crafting profession automation
- **colors/**: Color theme management
- **worlddrops/**: World drop item tracking

### Guild System
Guild-specific functionality in `common/guilds/`:
- Each guild has standardized files: index.tin, actions.tin, aliases.tin, etc.
- Common guild patterns: mage, bard, juggernaut, knight, priest, etc.
- Guild-specific strategies and combat automation

### Bot System
Automated gameplay scripts in `common/bot/bots/`:
- Area-specific farming bots (e.g., crystals.tin, aegis.tin)
- Generic bot framework in `common/bot/generic.tin`
- Bot selection via `botlist` command

## Development Practices

### File Organization
- Character-specific changes go in `chars/charactername/`
- Shared functionality goes in `common/`
- New modules should be self-contained in `modules/`
- Guild modifications go in `common/guilds/guildname/`

### Variable Management
- Variables persist between sessions via `_variable_import`/`_variable_export`
- Character variables stored in `chars/$user/vars.tin`
- Use descriptive variable names with appropriate prefixes

### Discord Integration
- Requires webhook URLs configured in character's `private.tin`
- Python script `scripts/discordPost.py` handles webhook posting
- Messages filtered through `sanitizePayload` function for safety

## Important Notes

- This is a TinTin++ script repository, not a traditional programming project
- No build system - files are interpreted directly by TinTin++
- The main entry point is `3k.tin` which provides character selection
- Session management handles automatic loading/saving of game state
- MIP (modules/mip/) provides the foundation for parsing MUD data streams
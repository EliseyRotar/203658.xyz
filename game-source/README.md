# 203658.xyz Game Source Code

This folder contains the **original source code** from https://203658.xyz/ (NOT the toolkit).

## Files

- **index.html** - The complete game (from xqr.htm)
- **xqr_files/** - Supporting files for the game
  - **a.htm** - Cloudflare challenge page
  - **a_data/main.js** - Cloudflare challenge script

## About the Game

This is the actual game that runs at https://203658.xyz/

### Features:
- Infinite procedurally-generated ASCII world
- Rare cartridge terminals (0.005% spawn rate)
- AES-GCM encrypted cartridge system
- WASD/Arrow key movement
- Touch/mouse drag support

### Technical Stack:
- Pure vanilla JavaScript
- HTML5 Canvas rendering
- Web Crypto API for encryption
- Compression API for GZIP

### Encryption Details:
- **Algorithm**: AES-GCM 256-bit
- **Key Derivation**: PBKDF2 (100,000 iterations)
- **Password**: `uninteresting205436705384`
- **Salt**: `enc-salt`
- **Compression**: GZIP

### Cartridge Format:
- Must start with node `203658`
- 6-digit node IDs
- Format: `NODEID Text content`
- Max 1582 chars per line

## Creating Cartridges

Use the toolkit at the repository root (index.html) to create encrypted cartridges for this game.

## License

Original game from https://203658.xyz/

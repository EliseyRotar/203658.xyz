# 203658.xyz Game Source

This folder contains the source code for the actual game at https://203658.xyz/

## Files

- `index.html` - The complete game source code
- All game logic, rendering, and cartridge system

## About the Game

The game is an infinite procedurally-generated world where players:
- Navigate using arrow keys or WASD
- Find rare cartridge terminals (0.005% spawn rate)
- Insert encrypted cartridges to play interactive text adventures
- Explore an endless ASCII art landscape

## Technical Details

- **Encryption**: AES-GCM 256-bit with PBKDF2 key derivation
- **Compression**: GZIP
- **Rendering**: ASCII art on HTML5 canvas
- **Frame Rate**: 20 FPS
- **World Generation**: Chunk-based (16x16 tiles)

## Cartridge Format

Cartridges must:
- Start with node `203658`
- Use 6-digit node IDs
- Follow the format: `NODEID Text content`
- Max 1582 characters per line

See the main toolkit at the repository root for creating cartridges.

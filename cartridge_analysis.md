# Complete Cartridge System Analysis

## Overview
This is an encrypted puzzle/adventure game where you insert "cartridges" (encrypted data strings) to load interactive question-and-answer scenarios.

## Encryption Details

### Password
```
"uninteresting205436705384"
```
Found in line 31 of the HTML: `_S[8]` which decodes from base64 array.

### Encryption Method
- **Algorithm**: AES-GCM (256-bit)
- **Key Derivation**: PBKDF2
  - Salt: "enc-salt"
  - Iterations: 100,000
  - Hash: SHA-256
- **Compression**: GZIP (data is compressed before encryption)

### Cartridge Format (After Decryption)

The decrypted data must be in this format:
```
203658 This is the first question text
123456 First answer option
789012 Second answer option

123456 This is where answer 1 leads
345678 Next choice A
901234 Next choice B

789012 This is where answer 2 leads
203658 Go back to start
000000 End node (ignored)
```

**Rules:**
1. Each node has a 6-digit ID
2. First node MUST be `203658` (hardcoded requirement)
3. Each question line: `NODEID Question text here`
4. Each answer line: `TARGET_NODEID Answer text here`
5. Lines starting with `000000` are ignored
6. Each question can have 0-2 answer options
7. Maximum text length: 1582 characters per line
8. Format: Groups of 3 lines (1 question + up to 2 answers)

## Cartridge Input Format

The cartridge string you paste into the game must be:
```
BASE64_IV:BASE64_ENCRYPTED_DATA
```

Example structure:
```
dGVzdGl2MTIzNDU2Nzg=:YWJjZGVmZ2hpamtsbW5vcHFyc3R1dnd4eXo=
```

## No Existing Cartridges Found

After analyzing all files:
- ❌ No pre-made cartridges found in the code
- ❌ No network requests to load cartridges
- ❌ No example cartridges in comments or documentation
- ✅ The game expects YOU to create or obtain cartridges

## The Coordinate System

The "random coordinates" you see (like 841037) are NOT related to cartridges:
- They are decorative elements in the procedurally-generated world
- The world is infinite and generates as you move
- Movement uses arrow keys or WASD or touch/mouse drag
- The coordinates at the center of screen show your position

## How to Use

1. Get or create an encrypted cartridge string
2. Navigate the world until you find a cartridge terminal (the box with "insert cartridge")
3. Click the input field
4. Paste your cartridge string
5. Click "run" button
6. If valid, it will load the question tree starting at node 203658

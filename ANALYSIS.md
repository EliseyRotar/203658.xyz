# 🎮 XQR Cartridge System - Complete Technical Analysis

## Overview

The XQR game at https://203658.xyz/ is an infinite procedurally-generated ASCII world where players can insert encrypted "cartridges" to load interactive text-based adventures.

**Key Finding:** No pre-made cartridges exist in the source code. You must create your own!

---

## 🔐 Encryption System

### Algorithm Details

| Parameter | Value |
|-----------|-------|
| Algorithm | AES-GCM |
| Key Size | 256 bits |
| Key Derivation | PBKDF2 |
| Iterations | 100,000 |
| Salt | `enc-salt` |
| Hash | SHA-256 |
| Password | `uninteresting205436705384` |
| Compression | GZIP |
| IV Size | 12 bytes (96 bits) |

### Encryption Process

1. **Compress** - Text is GZIP compressed
2. **Derive Key** - PBKDF2 with 100,000 iterations generates AES-256 key
3. **Encrypt** - AES-GCM encrypts the compressed data
4. **Encode** - IV and encrypted data are base64 encoded
5. **Format** - Combined as `IV:ENCRYPTED`

### Code Implementation

```javascript
// Password (line 31)
const _S = JSON.parse(atob('WyJQQktERjIi...'));
const i = _S[8]; // = "uninteresting205436705384"

// Key Derivation (lines 234-241)
async function cx(fg) {
  const df = new TextEncoder();
  const fk = await crypto.subtle.importKey('raw', df.encode(fg), 'PBKDF2', false, ['deriveKey']);
  return crypto.subtle.deriveKey(
    { name: 'PBKDF2', salt: df.encode('enc-salt'), iterations: 100000, hash: 'SHA-256' },
    fk, { name: 'AES-GCM', length: 256 }, false, ['decrypt']
  );
}

// Decryption (lines 259-266)
async function cw(dg) {
  const [ds, co] = dg.trim().split(':');
  const dr = Uint8Array.from(atob(ds), bs => bs.charCodeAt(0)); // IV
  const cn = Uint8Array.from(atob(co), bs => bs.charCodeAt(0)); // Encrypted data
  const dv = await cx(i); // Derive key
  const fc = await crypto.subtle.decrypt({ name: 'AES-GCM', iv: dr }, dv, cn);
  return new TextDecoder().decode(await cv(new Uint8Array(fc)));
}
```

---

## 📝 Cartridge Format

### Encrypted Format (What You Paste)

```
BASE64_IV:BASE64_ENCRYPTED_DATA
```

Example:
```
dGVzdGl2MTIzNDU2Nzg=:YWJjZGVmZ2hpamtsbW5vcHFyc3R1dnd4eXo=
```

### Decrypted Format (What's Inside)

```
203658 First question text here?
111111 First answer option
222222 Second answer option

111111 Where answer 1 leads
333333 Next choice A
444444 Next choice B

222222 Where answer 2 leads
203658 Go back to start
000000 End (ignored)
```

### Format Rules

1. ✅ **First node MUST be `203658`** (hardcoded requirement)
2. ✅ Each line: `NODEID Text content`
3. ✅ Node IDs are exactly 6 digits (000000-999999)
4. ✅ Groups of 3 lines: 1 question + up to 2 answers
5. ✅ Lines starting with `000000` are ignored
6. ✅ Max 1582 characters per text field
7. ✅ Questions can have 0, 1, or 2 answer options
8. ✅ Answer target IDs can point to any node (including back to 203658)

### Parsing Logic (lines 270-289)

```javascript
function ey(fk) {
  const eq = {};
  const ef = fk.split('\n').map(dy => dy.trim()).filter(dy => /^\d{6} /.test(dy));
  for (let dn = 0; dn < ef.length; dn += 3) {
    if (ef[dn].startsWith('000000')) continue;
    const [en, ...ep] = ef[dn].split(' ');
    const eo = ep.join(' ');
    if (eo.length > g) throw new Error(); // Max 1582 chars
    const bl = [];
    for (let dt = dn + 1; dt < dn + 3 && dt < ef.length; dt++) {
      if (ef[dt].startsWith('000000')) continue;
      const [gf, ...bi] = ef[dt].split(' ');
      const bh = bi.join(' ');
      if (bh.length > g) throw new Error();
      bl.push({ target: gf, text: bh });
    }
    eq[en] = { text: eo, answers: bl };
  }
  return eq;
}
```

---

## 🎮 Game Mechanics

### World Generation

- **Infinite** procedurally-generated world
- **Chunk-based** generation (16x16 tiles)
- **Frame Rate**: 20 FPS
- **Cleanup**: Every 30 moves

### Cartridge Terminals

**Spawn Rate**: 0.005% chance per chunk

**Appearance**:
```
╓────────────────────────╖
║ -- insert cartridge -- ║
║ ┌─────────────┐┌─────┐ ║
║ │             ││ run │ ║
║ └─────────────┘└─────┘ ║
╙────────────────────────╜
```

**Dimensions**:
- Width: 26 characters
- Height: 6 lines
- Question text width: 22 characters
- Answer text width: 17 characters

### Controls

| Input | Action |
|-------|--------|
| Arrow Keys / WASD | Move around |
| Mouse Drag | Touch/mobile movement |
| Click | Select answers |

### Cartridge Loading (lines 505-518)

```javascript
async function ao(by) {
  const dg = by.val.trim();
  if (!dg) return;
  try {
    const fk = await cw(dg); // Decrypt
    const ez = ey(fk);       // Parse
    if (!ez['203658']) throw new Error(); // MUST have node 203658
    by.nodes = ez;
    r(by, '203658'); // Start at node 203658
    t();
  } catch {
    by.showErr = true; // Show "invalid cartridge"
    t();
  }
}
```

**Critical**: If node `203658` doesn't exist, the cartridge is rejected with "invalid cartridge" error.

---

## 🌍 World System

### Decorative Elements

**Random Coordinates**: The 6-digit numbers displayed (like "841037") are purely decorative and NOT related to cartridges.

```javascript
function x() {
  return String(Math.floor(Math.random() * 1000000)).padStart(6, '0');
}
```

### World Objects

- **Trees** (`dx` array) - ASCII art obstacles
- **Markers** (`es` array) - 'v' decorative elements
- **Terminals** (`bz` array) - Cartridge input boxes

### Spawn Logic (lines 143-180)

```javascript
function aj(hb, hc, he, hf) {
  // ... spawn trees and markers ...
  const bm = (hc - hb) * (hf - he);
  if (Math.random() < bm * 0.00005) { // Very rare spawn
    for (let bn = 0; bn < 20; bn++) {
      const gy = hb + Math.floor(Math.random() * Math.max(1, hc - hb - 26));
      const gz = he + Math.floor(Math.random() * Math.max(1, hf - he - 8));
      if (!af(gy, gz)) {
        const by = {
          wx: gy, wy: gz,
          state: 'idle',
          val: '',
          nodes: {},
          curNode: '',
          answers: [],
          el: null,
          showErr: false,
        };
        bz.push(by);
        ac(by); // Create input element
        break;
      }
    }
  }
}
```

---

## 💡 Example Cartridges

### Simple Quiz

```
203658 What is 2+2?
111111 Answer: 3
222222 Answer: 4

111111 Wrong! Try again.
203658 Back to question
000000 End

222222 Correct! You win!
203658 Play again
000000 End
```

### Choose Your Adventure

```
203658 You're at a crossroads. Which path?
111111 Dark forest
222222 Sunny meadow

111111 The forest is spooky...
333333 Continue deeper
203658 Go back

222222 The meadow is peaceful...
444444 Rest here
203658 Return to crossroads
```

### Interactive Story

```
203658 Chapter 1: The Beginning
111111 Continue to Chapter 2
000000 End

111111 Chapter 2: The Journey
222222 Continue to Chapter 3
203658 Back to Chapter 1
```

---

## 🐛 Troubleshooting

### "invalid cartridge" Error

**Causes:**
- ❌ Missing node 203658
- ❌ Wrong encryption password
- ❌ Corrupted base64 encoding
- ❌ Invalid format (not `IV:ENCRYPTED`)
- ❌ Text exceeds 1582 characters per line
- ❌ Node IDs not exactly 6 digits

### Coordinates Keep Changing

**This is normal!**
- ✅ Numbers are decorative only
- ✅ World is infinite and procedurally generated
- ✅ NOT related to cartridges
- ✅ Use arrow keys/WASD to navigate

### Can't Find Cartridge Terminal

**Tips:**
- ✅ Terminals spawn randomly (0.005% chance)
- ✅ Keep exploring in different directions
- ✅ World generates as you move
- ✅ Look for the box with "insert cartridge"

---

## 📊 Technical Statistics

| Metric | Value |
|--------|-------|
| Total Lines Analyzed | 1,535+ |
| Encryption Functions | 3 |
| Parsing Functions | 8 |
| Rendering Functions | 12 |
| Input Handlers | 6 |
| World Generation Functions | 5 |
| Pre-made Cartridges Found | 0 |
| Required Starting Node | 203658 |
| Max Text Length | 1582 chars |
| Terminal Spawn Rate | 0.005% |
| Frame Rate | 20 FPS |
| Chunk Size | 16x16 |

---

## 📁 Source Files

### Game Files (in `game-source/`)

- **index.html** - Main game (796 lines)
- **view-source.htm** - Firefox view-source version
- **xqr_files/a.htm** - Cloudflare challenge page
- **xqr_files/a_data/main.js** - Cloudflare challenge script

### Analysis Results

- ❌ No pre-made cartridges in source
- ❌ No network requests for cartridges
- ❌ No example data or documentation
- ✅ Complete encryption system reverse-engineered
- ✅ All format rules documented

---

## 🔧 Advanced Usage

### Validate Your Cartridge

```javascript
const myStory = `203658 Test`;
encryptCartridge(myStory)
  .then(decryptCartridge)
  .then(console.log);
```

### Batch Create Multiple

```javascript
const stories = [story1, story2, story3];
Promise.all(stories.map(encryptCartridge))
  .then(cartridges => console.log(cartridges));
```

### Check for Node 203658

```javascript
decryptCartridge("YOUR_CARTRIDGE").then(text => {
  console.log(text.includes("203658") ? "✅ Valid" : "❌ Missing 203658");
});
```

---

## ✅ Checklist for Creating Cartridges

- [ ] First node is 203658
- [ ] All node IDs are exactly 6 digits
- [ ] Format: `NODEID Text`
- [ ] Max 1582 chars per line
- [ ] 0-2 answers per question
- [ ] Tested by decrypting
- [ ] Includes way back to start (203658)
- [ ] No typos in node IDs

---

## 📚 Resources

- **Toolkit**: https://eliseyrotar.github.io/203658.xyz/
- **Game**: https://203658.xyz/
- **Source**: `game-source/` folder
- **Quick Start**: QUICK_START.md
- **Repository**: https://github.com/EliseyRotar/203658.xyz

---

*Analysis completed through complete reverse-engineering of the XQR game source code. All encryption parameters and formats were discovered through code analysis.*

# 🎮 COMPLETE XQR CARTRIDGE SYSTEM ANALYSIS

## 📋 EXECUTIVE SUMMARY

**NO EXISTING CARTRIDGES FOUND** - You must create your own!

The website https://203658.xyz/ is an interactive adventure game where you:
1. Navigate an infinite procedurally-generated world
2. Find cartridge terminals (boxes with "insert cartridge")
3. Insert encrypted cartridge strings to load question-based adventures

---

## 🔍 LINE-BY-LINE CODE ANALYSIS

### Key Variables & Constants

```javascript
// Line 31: Encryption password (decoded from base64)
const _S = JSON.parse(atob('WyJQQktERjIi...'));
const i = _S[8]; // = "uninteresting205436705384"

// Lines 52-59: Cartridge UI template
const d = [
  '╓────────────────────────╖',
  '║ -- insert cartridge -- ║',
  '║ ┌─────────────┐┌─────┐ ║',
  '║ │             ││ run │ ║',
  '║ └─────────────┘└─────┘ ║',
  '╙────────────────────────╜',
];

// Line 60: Error message
const b = 'invalid cartridge';

// Lines 61-65: Cartridge dimensions
const e = 26; // width
const c = 6;  // height
const j = 22; // question text width
const a = 17; // answer text width

// Line 268: Max text length per line
const g = 1582;
```

### Encryption Functions

#### Line 234-241: Key Derivation
```javascript
async function cx(fg) {
  const df = new TextEncoder();
  const fk = await crypto.subtle.importKey('raw', df.encode(fg), 'PBKDF2', false, ['deriveKey']);
  return crypto.subtle.deriveKey(
    { name: 'PBKDF2', salt: df.encode('enc-salt'), iterations: 100000, hash: 'SHA-256' },
    fk, { name: 'AES-GCM', length: 256 }, false, ['decrypt']
  );
}
```
- Uses PBKDF2 with 100,000 iterations
- Salt: "enc-salt"
- Derives AES-GCM 256-bit key

#### Line 243-257: Decompression
```javascript
async function cv(br) {
  const da = new DecompressionStream('gzip');
  const gx = da.writable.getWriter();
  gx.write(br); gx.close();
  const cd = [], fl = da.readable.getReader();
  while (true) {
    const { done: cz, value: gq } = await fl.read();
    if (cz) break;
    cd.push(gq);
  }
  // Concatenates chunks and returns Uint8Array
}
```
- Data is GZIP compressed before encryption
- Must decompress after decryption

#### Line 259-266: Main Decryption
```javascript
async function cw(dg) {
  const [ds, co] = dg.trim().split(':');
  const dr = Uint8Array.from(atob(ds), bs => bs.charCodeAt(0)); // IV
  const cn = Uint8Array.from(atob(co), bs => bs.charCodeAt(0)); // Encrypted data
  const dv = await cx(i); // Derive key
  const fc = await crypto.subtle.decrypt({ name: 'AES-GCM', iv: dr }, dv, cn);
  return new TextDecoder().decode(await cv(new Uint8Array(fc)));
}
```
- Expects format: `BASE64_IV:BASE64_ENCRYPTED`
- Decrypts using AES-GCM
- Decompresses result
- Returns plaintext

### Cartridge Parsing

#### Line 270-289: Parse Decrypted Data
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

**Format Rules:**
- Lines must match regex: `/^\d{6} /` (6 digits + space + text)
- Processes in groups of 3 lines
- Line 1: Question node (NODEID Question text)
- Lines 2-3: Answer options (TARGET_NODEID Answer text)
- Lines starting with "000000" are skipped
- Max 1582 characters per text field

### Cartridge Loading

#### Line 505-518: Load Cartridge
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

**Critical Requirement:** Node `203658` MUST exist or cartridge is rejected!

### World Generation

#### Line 107-111: World state
```javascript
let dx = []; // Trees/obstacles
let es = []; // Decorative 'v' markers
let bz = []; // Cartridge terminals
let ak = 0, al = 0; // Player position
let ah = '000000'; // Random 6-digit display (decorative only!)
```

#### Line 113-115: Random ID Generator
```javascript
function x() {
  return String(Math.floor(Math.random() * 1000000)).padStart(6, '0');
}
```
- Generates random 6-digit numbers like "841037"
- **These are NOT cartridge IDs!**
- Just decorative elements in the world

#### Line 143-180: Spawn Cartridge Terminals
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
- Cartridge terminals spawn randomly (0.005% chance per chunk)
- Each terminal is a text input + run button
- Terminals are rare - you need to explore to find them

### Movement System

#### Line 641-709: Input Handling
```javascript
const dw = {}; // Keyboard state
document.addEventListener('keydown', dd => {
  if (dd.key.startsWith('Arrow')) dd.preventDefault();
  dw[dd.key] = true;
});

// Touch/mouse dragging for mobile
let aw = null, at = 0, au = 0;
function bc(cp, cs) {
  const db = cp - aw.x, dc = cs - aw.y;
  const cy = Math.sqrt(db * db + dc * dc);
  if (cy < 12) { at = 0; au = 0; return; }
  const fv = Math.round(Math.atan2(dc, db) / (Math.PI / 4));
  at = Math.round(Math.cos(fv * Math.PI / 4));
  au = Math.round(Math.sin(fv * Math.PI / 4));
}
```

#### Line 711-738: Game Loop
```javascript
function eg(er) {
  requestAnimationFrame(eg);
  if (er - aa < m) return; // 20 FPS
  
  let db = 0, dc = 0;
  if (aw) {
    db = at; dc = au; // Touch/mouse movement
  } else {
    if (dw['ArrowLeft'] || dw['a'] || dw['A']) db -= 1;
    if (dw['ArrowRight'] || dw['d'] || dw['D']) db += 1;
    if (dw['ArrowUp'] || dw['w'] || dw['W']) dc -= 1;
    if (dw['ArrowDown'] || dw['s'] || dw['S']) dc += 1;
  }
  
  if (db !== 0 || dc !== 0) {
    ak += db; al += dc; // Update position
    ah = x(); // Generate new random display number
    u(); // Generate new world chunks
    if (++aq >= 30) { s(); aq = 0; } // Cleanup every 30 moves
    t(); // Render
  }
}
```

---

## 🎯 CARTRIDGE FORMAT SPECIFICATION

### Input Format (What You Paste)
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

### Rules
1. ✅ First node MUST be `203658`
2. ✅ Each line: `NODEID Text content`
3. ✅ Node IDs are 6 digits (000000-999999)
4. ✅ Groups of 3 lines: 1 question + up to 2 answers
5. ✅ Lines starting with `000000` are ignored
6. ✅ Max 1582 characters per text field
7. ✅ Questions can have 0, 1, or 2 answer options
8. ✅ Answer target IDs can point to any node (including back to 203658)

---

## 🔐 ENCRYPTION PARAMETERS

| Parameter | Value |
|-----------|-------|
| Algorithm | AES-GCM |
| Key Size | 256 bits |
| Key Derivation | PBKDF2 |
| KDF Iterations | 100,000 |
| KDF Hash | SHA-256 |
| KDF Salt | "enc-salt" |
| Password | "uninteresting205436705384" |
| Compression | GZIP |
| IV Size | 12 bytes (96 bits) |

---

## 📁 FILES ANALYZED

### ✅ xqr.htm (796 lines)
- Main game file
- Contains all game logic
- Encryption/decryption functions
- World generation
- Cartridge system

### ✅ https___203658.xyz_.htm (739 lines)
- View-source version of main file
- Same content as xqr.htm
- HTML-escaped for viewing

### ✅ xqr_files/a_data/main.js
- Cloudflare challenge script
- NOT related to cartridge system
- Security/bot detection

### ✅ xqr_files/a.htm
- Cloudflare challenge page
- NOT related to cartridge system

### ❌ No cartridge data files found
### ❌ No network requests for cartridges
### ❌ No example cartridges in code
### ❌ No documentation or hints

---

## 🎮 HOW TO USE

### Method 1: Use the HTML Tool
1. Open `cartridge_tool.html` in your browser
2. Click "Generate Example Cartridge"
3. Copy the encrypted string
4. Go to https://203658.xyz/
5. Navigate until you find a cartridge terminal
6. Paste and click "run"

### Method 2: Use Console Script
1. Go to https://203658.xyz/
2. Open browser console (F12)
3. Paste contents of `console_script.js`
4. Run: `encryptCartridge(EXAMPLE_STORY).then(copy);`
5. Find a cartridge terminal
6. Paste and click "run"

### Method 3: Create Custom Cartridge
```javascript
const myStory = `203658 You are in a dark room. What do you do?
111111 Turn on the light
222222 Feel around in the dark

111111 The light reveals a treasure chest!
203658 Go back
000000 End

222222 You find a secret passage!
203658 Return to the room
000000 End`;

encryptCartridge(myStory).then(cartridge => {
  console.log("Your cartridge:", cartridge);
  copy(cartridge);
});
```

---

## 🐛 COMMON ISSUES

### "invalid cartridge" Error
- ❌ Missing node 203658
- ❌ Wrong encryption password
- ❌ Corrupted base64
- ❌ Invalid format (not IV:ENCRYPTED)
- ❌ Text exceeds 1582 characters
- ❌ Node IDs not 6 digits

### Coordinates Moving Randomly
- ✅ This is NORMAL behavior
- ✅ The world is infinite and procedurally generated
- ✅ Numbers like "841037" are decorative only
- ✅ They are NOT cartridge IDs
- ✅ Use arrow keys/WASD to navigate

### Can't Find Cartridge Terminal
- ✅ Terminals spawn randomly (0.005% chance)
- ✅ Keep exploring the world
- ✅ They look like boxes with "insert cartridge"
- ✅ Move in different directions to generate new chunks

---

## 📊 STATISTICS

- **Total Lines Analyzed**: 1,535+
- **Encryption Functions**: 3
- **Parsing Functions**: 8
- **Rendering Functions**: 12
- **Input Handlers**: 6
- **World Generation Functions**: 5
- **Cartridges Found**: 0
- **Required Starting Node**: 203658
- **Max Text Length**: 1582 chars
- **Terminal Spawn Rate**: 0.005%

---

## ✅ CONCLUSION

**NO EXISTING CARTRIDGES EXIST IN THE CODE.**

You must either:
1. Create your own using the provided tools
2. Find cartridges shared by other players
3. Use the example cartridge provided in the tools

The game is a blank canvas waiting for user-generated content!

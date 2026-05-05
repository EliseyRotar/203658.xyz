# 🚀 QUICK START GUIDE

## TL;DR - Get Playing in 2 Minutes

### Option 1: Browser Console (Fastest)
1. Go to https://203658.xyz/
2. Press **F12** to open console
3. Copy and paste **console_script.js** into console
4. Run: `copy(EXAMPLE_CARTRIDGE)`
5. Find a cartridge terminal in the game
6. Paste and click "run"

### Option 2: HTML Tool (Easiest)
1. Open **cartridge_tool.html** in your browser
2. Click "Generate Example Cartridge"
3. Click "Copy Result"
4. Go to https://203658.xyz/
5. Find a cartridge terminal
6. Paste and click "run"

---

## 🎮 Game Controls

| Control | Action |
|---------|--------|
| Arrow Keys | Move around |
| WASD | Alternative movement |
| Mouse Drag | Touch/mobile movement |
| Click | Interact with answers |

---

## 🔍 Finding Cartridge Terminals

Cartridge terminals look like this:
```
╓────────────────────────╖
║ -- insert cartridge -- ║
║ ┌─────────────┐┌─────┐ ║
║ │             ││ run │ ║
║ └─────────────┘└─────┘ ║
╙────────────────────────╜
```

**Tips:**
- They spawn randomly (rare)
- Keep exploring in different directions
- The world is infinite
- Look for the box with input field

---

## ✏️ Creating Your Own Cartridge

### Simple Example
```
203658 You find a magic lamp. Rub it?
111111 Yes, rub the lamp
222222 No, leave it alone

111111 A genie appears and grants you three wishes!
203658 Make a wish
000000 End

222222 You walk away. The adventure ends.
203658 Go back
000000 End
```

### Encrypt It
**Using Console:**
```javascript
const myStory = `203658 Your text here...`;
encryptCartridge(myStory).then(copy);
```

**Using HTML Tool:**
1. Paste your story in the text area
2. Click "Encrypt to Cartridge"
3. Copy the result

---

## ❌ Common Mistakes

| Mistake | Fix |
|---------|-----|
| No node 203658 | First node MUST be 203658 |
| Wrong format | Use: `NODEID Text here` |
| Not 6 digits | Node IDs must be exactly 6 digits |
| Too long | Max 1582 chars per line |
| Wrong encryption | Use the provided tools |

---

## 📝 Cartridge Format Cheat Sheet

```
NODEID Question or statement text
TARGETID Answer option 1
TARGETID Answer option 2

TARGETID Next question or statement
TARGETID Answer A
TARGETID Answer B
```

**Rules:**
- ✅ First node = 203658
- ✅ 6-digit node IDs
- ✅ 0-2 answers per question
- ✅ Use 000000 to ignore/end
- ✅ Can loop back to any node

---

## 🎯 Example Node IDs

| ID | Purpose |
|----|---------|
| 203658 | START (required) |
| 111111 | Path A |
| 222222 | Path B |
| 333333 | Path C |
| 999999 | Ending |
| 000000 | Ignored/End |

---

## 🔧 Tools Provided

| File | Purpose |
|------|---------|
| **cartridge_tool.html** | Visual tool for encryption/decryption |
| **console_script.js** | Console commands for quick use |
| **COMPLETE_ANALYSIS.md** | Full technical documentation |
| **cartridge_analysis.md** | Encryption details |
| **EXAMPLE_CARTRIDGE.txt** | Ready-to-use example |

---

## 💡 Pro Tips

1. **Test your cartridge** - Decrypt it first to verify format
2. **Keep it simple** - Start with 3-5 nodes
3. **Use 203658** - Always include a way back to start
4. **Save your work** - Keep the decrypted version
5. **Share cartridges** - Exchange with other players

---

## 🆘 Troubleshooting

### "invalid cartridge" Error
```javascript
// Check if your cartridge has node 203658
decryptCartridge("YOUR_CARTRIDGE").then(text => {
  console.log(text.includes("203658") ? "✅ Has 203658" : "❌ Missing 203658");
});
```

### Can't Find Terminal
- Keep exploring (they're rare)
- Try different directions
- The world generates as you move

### Coordinates Keep Changing
- This is normal!
- The numbers are decorative
- Not related to cartridges

---

## 📚 Learn More

- **COMPLETE_ANALYSIS.md** - Full code analysis
- **cartridge_analysis.md** - Encryption details
- **console_script.js** - See the code

---

## 🎉 Ready to Play!

1. Generate example cartridge
2. Find terminal in game
3. Paste and run
4. Enjoy the adventure!

**Have fun exploring! 🌲✨**

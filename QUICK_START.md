# 🚀 Quick Start Guide

Get started creating cartridges in under 2 minutes!

## Step 1: Open the Toolkit

Visit **https://eliseyrotar.github.io/203658.xyz/** or open `index.html` locally.

## Step 2: Generate an Example

Click the **"Generate Example"** button to create a random story. Each click generates a different adventure!

## Step 3: Copy the Cartridge

After encryption completes, click **"Copy Result"** to copy the encrypted cartridge string.

## Step 4: Play the Game

1. Go to **https://203658.xyz/**
2. Use **Arrow Keys** or **WASD** to explore
3. Find a cartridge terminal (rare spawn - keep exploring!)
4. Paste your cartridge and click **"run"**

## Cartridge Terminal

Look for this in the game world:

```
╓────────────────────────╖
║ -- insert cartridge -- ║
║ ┌─────────────┐┌─────┐ ║
║ │             ││ run │ ║
║ └─────────────┘└─────┘ ║
╙────────────────────────╜
```

## Create Your Own Story

### Format Rules:
- First node **must be** `203658`
- Each node: `NODEID Text content`
- Node IDs are 6 digits (000000-999999)
- Max 1582 characters per line
- 0-2 answer options per question

### Example:

```
203658 You find a mysterious door. Open it?
111111 Yes, open the door
222222 No, walk away

111111 Inside is a treasure chest!
203658 Go back
000000 End

222222 You leave. The adventure ends.
203658 Return to the door
000000 End
```

## Tips

- **Test first**: Use the "Decrypt Cartridge" section to verify your format
- **Start simple**: Begin with 3-5 nodes
- **Always include 203658**: Players need a way back to start
- **Save your work**: Keep the unencrypted version

## Troubleshooting

**"invalid cartridge" error?**
- Check that node 203658 exists
- Verify all node IDs are exactly 6 digits
- Ensure text doesn't exceed 1582 characters per line

**Can't find a terminal?**
- They spawn randomly (0.005% chance)
- Keep exploring in different directions
- The world is infinite - keep moving!

## Learn More

See **ANALYSIS.md** for complete technical documentation including:
- Full encryption details
- Game mechanics
- Cartridge format specification
- Advanced usage examples

---

**Ready to create? Visit https://eliseyrotar.github.io/203658.xyz/**

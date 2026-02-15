# Linux Migration — Step by Step
# Getting ChetGPT back on native Linux (Dell XPS)
# Written for Aviel — copy-paste every command, nothing to figure out

---

## BEFORE YOU START (Samsung laptop)

### Step A: Download Ubuntu
1. On your Samsung laptop, go to: https://ubuntu.com/download/desktop
2. Click "Download Ubuntu 24.04 LTS" (it's a free .iso file, ~5GB)
3. Save it somewhere you can find it

### Step B: Make a bootable USB
1. Plug in a USB stick (8GB minimum — it will be erased)
2. Download Rufus: https://rufus.ie (click "Rufus Portable")
3. Open Rufus
4. Under "Device" — select your USB stick
5. Under "Boot selection" — click SELECT, find the Ubuntu .iso you downloaded
6. Click START
7. Wait until it says "READY" (takes ~5 minutes)
8. Done — unplug the USB

---

## ON THE DELL XPS

### Step 1: Boot from USB
1. Plug the USB stick into the Dell
2. Turn on (or restart) the Dell
3. Immediately press F12 repeatedly until you see a boot menu
4. Select the USB stick from the list
5. Ubuntu will load — pick "Install Ubuntu"

### Step 2: Install Ubuntu
1. Choose your language (English)
2. Choose "Erase disk and install Ubuntu" — this wipes Windows, that's what we want
3. Set your name: `inkredible`
4. Set computer name: `inkredible-server`
5. Pick a password you'll remember
6. Click Install and wait (~15-20 minutes)
7. When done, click "Restart Now"
8. Remove the USB when prompted

### Step 3: First boot into Ubuntu
1. Log in with the password you just set
2. It may ask to install updates — say Yes, let it finish
3. Open Terminal (press Ctrl+Alt+T or search "Terminal" in apps)

### Step 4: Install basic tools
Copy-paste these one at a time into Terminal:

```bash
sudo apt update && sudo apt upgrade -y
```

```bash
sudo apt install -y curl git build-essential
```

### Step 5: Install Node.js
```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install -y nodejs
```

Verify it worked:
```bash
node --version
```
Should say v22.something.

### Step 6: Install OpenClaw
```bash
sudo npm install -g openclaw
```

```bash
openclaw init
```
Follow the prompts. When it asks for Anthropic API key, you can skip for now.

### Step 7: Download the backup
```bash
cd ~/.openclaw/workspace
curl -L -o chetgpt-backup.tar.gz https://github.com/Shadowwall44/kanban/raw/main/chetgpt-backup-2026-02-14.tar.gz
```

### Step 8: Unpack the backup
```bash
tar -xzf chetgpt-backup.tar.gz
```

### Step 9: Restore everything
```bash
# Copy skills
cp -r backup-package/skills/* ~/.openclaw/skills/

# Copy workspace files (memory, reports, templates, etc.)
cp -r backup-package/workspace/* ~/.openclaw/workspace/

# Copy agent config (auth profiles, model settings)
cp -r backup-package/agent-config/* ~/.openclaw/agents/main/agent/

# Copy main config
cp backup-package/openclaw.json ~/.openclaw/openclaw.json
```

### Step 10: Clone the GitHub repos
```bash
cd ~/.openclaw/workspace
git config --global user.email "avielsamucha92@gmail.com"
git config --global user.name "Shadowwall44"

git clone https://github.com/Shadowwall44/kanban.git
git clone https://github.com/Shadowwall44/inkredible-tools.git
git clone https://github.com/Shadowwall44/inkredible-voice.git
```

### Step 11: Start OpenClaw
```bash
openclaw gateway start
```

### Step 12: Connect Telegram
Open Telegram on your phone and message @AvielAI_Bot.
If the bot doesn't respond, you may need to re-pair:
```bash
openclaw pair telegram
```
Follow the on-screen instructions.

---

## AFTER I'M BACK ONLINE

Once ChetGPT is running on the new Linux install, I will automatically:

1. ✅ Detect the fresh install
2. ✅ Read MEMORY.md and pick up where we left off
3. ✅ Recreate all 14 cron jobs from the backup
4. ✅ Verify all skills are loaded
5. ✅ Test Telegram connection
6. ✅ Send you a "I'm back" message

You don't need to do anything after Step 12. Just message me on Telegram and I'll handle the rest.

---

## TROUBLESHOOTING

### "openclaw: command not found"
Run: `sudo npm install -g openclaw`

### "permission denied"
Add `sudo` before the command

### Dell won't boot from USB
- Make sure Secure Boot is disabled in BIOS (press F2 at startup → Security → Secure Boot → Disabled)
- Try a different USB port

### "node: command not found"
Run Step 5 again

### Telegram bot doesn't respond
Run: `openclaw gateway restart`
Then: `openclaw pair telegram`

### WiFi not working on Ubuntu
- Ubuntu usually handles WiFi automatically
- If not: click the network icon top-right, select your WiFi network

---

## QUICK REFERENCE

| What | Command |
|------|---------|
| Start OpenClaw | `openclaw gateway start` |
| Stop OpenClaw | `openclaw gateway stop` |
| Check status | `openclaw status` |
| View logs | `openclaw gateway logs` |
| Restart | `openclaw gateway restart` |

---

## TOTAL TIME: ~45 minutes
- USB creation: 10 min
- Ubuntu install: 20 min
- OpenClaw + restore: 15 min

No coding knowledge needed. Just copy-paste every command.

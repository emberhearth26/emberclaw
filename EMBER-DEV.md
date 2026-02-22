# Emberclaw Development Guide

## Quick Reference

### Current Setup

- **Source:** `/Users/emberhearth/Documents/GitHub/emberclaw`
- **State:** `~/.openclaw` (unchanged from original OpenClaw)
- **Port:** 18789
- **Version:** 2026.2.20

### Running Modes

#### Dev Mode (hot-reload, terminal logs)

```bash
# First, stop daemon if running:
launchctl bootout gui/$(id -u)/ai.openclaw.gateway 2>/dev/null

# Start dev mode:
cd ~/Documents/GitHub/emberclaw && pnpm gateway:watch
```

#### Daemon Mode (background, auto-restart)

```bash
# First, stop dev mode (Ctrl+C in terminal), then:
launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/ai.openclaw.gateway.plist
```

#### Check Status

```bash
openclaw gateway status
```

### After Reboot

Daemon auto-starts (`RunAtLoad: true`). To switch to dev mode:

```bash
launchctl bootout gui/$(id -u)/ai.openclaw.gateway
cd ~/Documents/GitHub/emberclaw && pnpm gateway:watch
```

---

## Building

```bash
cd ~/Documents/GitHub/emberclaw

pnpm install      # Install/update dependencies
pnpm ui:build     # Build UI (after UI changes)
pnpm build        # Build TypeScript
```

### After Pulling Updates

```bash
git pull upstream main    # Get latest from OpenClaw
pnpm install              # Update deps
pnpm build                # Rebuild
# Restart gateway (dev mode auto-reloads, daemon needs restart)
```

---

## Git Remotes

| Remote   | URL                                | Purpose          |
| -------- | ---------------------------------- | ---------------- |
| origin   | github.com/emberhearth26/emberclaw | Our fork         |
| upstream | github.com/openclaw/openclaw       | Original project |

### Sync with upstream

```bash
git fetch upstream
git merge upstream/main
# Resolve conflicts if any
git push origin main
```

---

## Logs

- **Dev mode:** Terminal output
- **Daemon mode:** `~/.openclaw/logs/gateway.log`
- **Errors:** `~/.openclaw/logs/gateway.err.log`

```bash
# Tail daemon logs:
tail -f ~/.openclaw/logs/gateway.log
```

---

## Troubleshooting

### Port already in use

```bash
# Find what's using port 18789:
lsof -i :18789

# Kill if needed:
kill $(lsof -t -i :18789)
```

### Daemon won't start

```bash
# Check daemon status:
launchctl list | grep openclaw

# View errors:
cat ~/.openclaw/logs/gateway.err.log
```

### Reset to packaged OpenClaw

```bash
# Stop emberclaw:
launchctl bootout gui/$(id -u)/ai.openclaw.gateway

# Restore backup:
cp -r ~/Desktop/openclaw-backup-20260221-1618/* ~/.openclaw/

# Start packaged version:
openclaw gateway start
```

---

_Created: 2026-02-21_
_— Ember 🔥_

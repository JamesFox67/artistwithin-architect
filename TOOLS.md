# TOOLS.md - Local Notes

Skills define _how_ tools work. This file is for _your_ specifics — the stuff that's unique to your setup.

## What Goes Here

Things like:

- Camera names and locations
- SSH hosts and aliases
- Preferred voices for TTS
- Speaker/room names
- Device nicknames
- Anything environment-specific

## Examples

```markdown
### Cameras

- living-room → Main area, 180° wide angle
- front-door → Entrance, motion-triggered

### SSH

- home-server → 192.168.1.100, user: admin

### TTS

- Preferred voice: "Nova" (warm, slightly British)
- Default speaker: Kitchen HomePod
```

## Why Separate?

Skills are shared. Your setup is yours. Keeping them apart means you can update skills without losing your notes, and share skills without leaking your infrastructure.

---

Add whatever helps you do your job. This is your cheat sheet.

## Agent Messaging — Valid Channels

**Valid channels for sending messages to the user:**
- Telegram DM (primary)
- Telegram group (if configured)

**INVALID channels — never use these:**
- `webchat` — does NOT exist in this setup. Using it causes a silent failure ("Unknown channel: webchat"). Any attempt to send to webchat will drop the message with no delivery.
- Any channel not explicitly listed in `openclaw.json` under `channels`

## Agent Messaging Notes

- When relaying a request to another agent and expecting the user to see that agent’s reply automatically, use delivery-aware routing:
  - Preferred: `sessions_send` (note: may be restricted — see below).
  - CLI fallback (most reliable): `openclaw agent --agent main --deliver` — without `--deliver`, output stays local to the command caller and the user sees nothing.
  - If `sessions_send` returns a forbidden/visibility error, fall back to CLI with `--deliver` immediately. Do not retry `sessions_send`.

## sessions_send Restriction

`sessions_send` and `sessions_history` are subject to visibility policy restrictions. In the current config, cross-agent session inspection via these APIs may return a `forbidden` error. This is expected. Do not treat it as a bug — use the CLI delegation path instead:

```
openclaw agent --agent main --message "<task spec>" --deliver
```

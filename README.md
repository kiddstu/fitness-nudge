# fitness-nudge

[![smoke](https://github.com/kiddstu/fitness-nudge/actions/workflows/smoke.yml/badge.svg)](https://github.com/kiddstu/fitness-nudge/actions/workflows/smoke.yml)

A Claude Code plugin that keeps you moving while Claude works.

Every time you hand Claude a task it sets you a small exercise — out loud — and
beeps when it needs you back. The idea is that the dead time while an agent
works is the most reliably wasted time in your day.

```
you:     "refactor the auth module"
         🔔 "ten push-ups"
claude:  Ten push-ups — go, I'll keep working.
         ...
         🔔  (it needs your input)
```

## Install

```
/plugin marketplace add kiddstu/fitness-nudge
/plugin install fitness-nudge@fitness-nudge
```

Then start a new session.

## Platforms

Works on macOS, Linux and Windows. The exercise, the log and Claude's spoken
announcement are platform-independent; only the audio differs.

| | Beep | Spoken exercise |
|---|---|---|
| **macOS** | `afplay` — built in | `say` — built in |
| **Windows** | PowerShell `SystemSounds` — built in | `System.Speech` — built in |
| **Linux** | `paplay`, `canberra-gtk-play` or `pw-play` | `espeak-ng`, `espeak` or `spd-say` — **install one** |

Linux is the only platform where voice needs anything installed:

```bash
sudo apt install espeak-ng
```

Windows needs `bash` on PATH for the hook to run at all, which Git for Windows
provides. WSL is detected and routed through PowerShell, since Windows audio is
more reliable there than Linux audio.

Missing audio is never fatal — with no player and no voice you still get the
exercise announced in text, logged and tallied. Check what was detected with:

```bash
bash scripts/claude-fitness platform
```

## How it works

Three hooks, no magic:

| Event | What happens |
|---|---|
| `UserPromptSubmit` | If the cooldown has elapsed, picks an exercise, logs it, speaks it, and tells Claude to announce it |
| `Notification` | Glass beep — Claude wants your input |
| `Stop` | Tink beep, *only* if a nudge is still outstanding |

The `Stop` beep is gated deliberately. Without that gate you get beeped after
every trivial exchange and stop hearing it within a day.

## Tuning

Ask Claude: `/fit report`, `/fit off`, `/fit every 15m`, `/fit desk only`,
`/fit harder`. Or do it yourself:

| Thing | Where |
|---|---|
| Exercise list | `~/.claude/fitness/exercises.txt`, one per line |
| Your log | `~/.claude/fitness/log.tsv` |
| Instant kill switch | `touch ~/.claude/fitness/off` |
| Gap between nudges | `CLAUDE_FITNESS_COOLDOWN` seconds, default 480 |
| Mute the voice, keep beeps | `CLAUDE_FITNESS_VOICE=0` |

Set env vars in the `env` block of `~/.claude/settings.json`.

## Design notes

- **Hooks, not a skill.** A skill is instructions to the model, so it depends on
  the model remembering. It drifts within a few turns. Hooks are executed by the
  harness and fire whether the model is paying attention or not. The bundled
  `/fit` skill only handles the parts needing judgment — tallies, tuning, picking
  a gentler list.
- **The nudge path always exits 0.** Exit code 2 on `UserPromptSubmit` blocks the
  prompt, so a bug here could stop you talking to Claude at all.
- **Audio is backgrounded.** Hooks are synchronous; an unbackgrounded `say`
  stalls your turn until it finishes speaking.
- **A cooldown is essential.** `UserPromptSubmit` fires on "yes" and "ok" too.
  Without a floor between nudges you'd be doing push-ups to approve a file read.

## Caveat

The hooks fire in every project, not just one. To scope them, guard on
`$CLAUDE_PROJECT_DIR` inside the `nudge` case in `scripts/claude-fitness`.

## Tests

`.github/workflows/smoke.yml` runs on every push across `macos-latest`,
`windows-latest` and `ubuntu-latest`.

**Covered:** all three hook paths exit 0; the nudge announces, logs and
respects the cooldown; the `pending` marker is set and cleared; the kill switch
suppresses; the 7-day report window is correct, which catches BSD vs GNU `date`
differences; an unknown subcommand still exits 0, since exit 2 on
`UserPromptSubmit` would block the user's prompt. On Linux, `paplay`,
`espeak-ng` and the freedesktop sound assets are installed and asserted
present, and `espeak-ng` is made to synthesize a real `.wav`.

**Not covered:** audible playback. CI runners are headless and have no audio
sink on any of the three platforms, so what is verified is that the right
player and speech engine are detected and invoked — not that a sound came out
of a speaker.

## License

MIT

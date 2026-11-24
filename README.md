# fitness-nudge

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

Then start a new session. macOS only — it uses `say` and `afplay`.

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

## License

MIT

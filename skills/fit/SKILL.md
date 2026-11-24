---
name: fit
description: "Report, tune or silence the fitness nudges — the movement break set when Claude starts work and the beep when it needs input. Use when the user asks how many sets they have done today, wants their fitness tally or streak, wants nudges off or on, wants them more or less frequent, wants the exercise list changed, or wants a different kind of exercise (desk-friendly, quiet, legs only, harder, easier). Not for general fitness advice unrelated to these nudges."
user-invocable: true
argument-hint: "[report | on | off | every <n>m | easier|harder|quiet|desk | add <exercise> | list]"
---

# Fitness nudges

The nudging is done by hooks, not by you. Three events drive it:

| Event | Behaviour |
|---|---|
| `UserPromptSubmit` | After the cooldown, picks an exercise, logs it, speaks it, injects it into context |
| `Notification` | Glass beep — Claude needs the user |
| `Stop` | Tink beep, but only if a nudge is still outstanding |

Your job is the judgment the hooks can't do. Never reimplement the nudging, and
never issue an exercise yourself — if no hook fired, no exercise is due.

## State

All state lives in `~/.claude/fitness/`, independent of where this plugin is
installed. Work on these files directly; you do not need to locate the script.

- `log.tsv` — one line per set: `timestamp<TAB>exercise`
- `exercises.txt` — one exercise per line; edit freely
- `off` — kill switch; nudges stop while this file exists
- `last-nudge`, `last-exercise`, `pending` — internal, leave alone

## Responding

**report / tally / streak / "how am I doing"**

```bash
A=~/.claude/fitness/log.tsv; T=$(date +%F)
awk -F'\t' -v d="$T" 'index($1,d)==1{c[$2]++} END{for(k in c) printf "%d\t%s\n",c[k],k}' "$A" | sort -rn
awk -F'\t' '{c[substr($1,1,10)]++} END{for(k in c) printf "%s %d\n",k,c[k]}' "$A" | sort | tail -7
```

Read the numbers back as prose, not raw output. Mention the day's total and
anything notable — a gap, a strong run. Don't congratulate mechanically.

**off / on** — `touch ~/.claude/fitness/off` or `rm -f ~/.claude/fitness/off`.
Takes effect immediately, no restart. Confirm in one line.

**every \<n\> minutes / more often / less often** — the cooldown is the env var
`CLAUDE_FITNESS_COOLDOWN`, in seconds, default 480. Set it in the `env` block of
`~/.claude/settings.json`. Say that it needs a new session to take effect.

**quiet** — `CLAUDE_FITNESS_VOICE=0` the same way. Beeps stay, speech stops.

**easier / harder / desk only / legs only / no floor work** — rewrite
`exercises.txt` to suit. Each line must be a short imperative phrase that reads
naturally when spoken aloud by `say`: "10 push-ups", not "Pushups x10". Keep
8–15 entries so repeats feel rare. Show the user the new list.

**add \<exercise\>** — append a line. **list** — print the file.

## Gotchas worth preserving

- The `nudge` path must always exit 0. Exit code 2 on `UserPromptSubmit` blocks
  the user's prompt entirely.
- Keep hooks fast and all audio backgrounded (`( ... & )`). Hooks are
  synchronous; an unbackgrounded `say` stalls the turn until it stops talking.
- `shuf` is not on macOS. The script uses a `$RANDOM` index instead.
- Hooks here fire in every project. To scope them, guard on
  `$CLAUDE_PROJECT_DIR` inside the `nudge` case.

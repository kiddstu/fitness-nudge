---
name: fit
description: "Record, report, tune or silence the fitness nudges — the movement break set when Claude starts work and the beep when it needs input. Use when the user says they did or skipped the exercise they were set (\"did those\", \"done\", \"skipped that one\"), asks how many sets they have done today, wants their fitness tally or streak, wants nudges off or on, wants them more or less frequent, wants the exercise list changed, or wants a different kind of exercise (desk-friendly, quiet, legs only, harder, easier). Not for general fitness advice unrelated to these nudges."
user-invocable: true
argument-hint: "[report | did | skip | on | off | every <n>m | easier|harder|quiet|desk | add <exercise> | list]"
---

# Fitness nudges

The nudging is done by hooks, not by you. Three events drive it:

| Event | Behaviour |
|---|---|
| `UserPromptSubmit` | After the cooldown, picks an exercise, logs it, speaks it, injects it into context. In between, names any set still unanswered |
| `Notification` | Glass beep — Claude needs the user |
| `Stop` | Tink beep, but only if a nudge is still outstanding |

Your job is the judgment the hooks can't do. Never reimplement the nudging, and
never issue an exercise yourself — if no hook fired, no exercise is due.

## State

All state lives in `~/.claude/fitness/`, independent of where this plugin is
installed. Work on these files directly; you do not need to locate the script.

- `log.tsv` — append-only, tab separated:
  `timestamp<TAB>exercise<TAB>set|did|skip<TAB>id`. A `set` row is what the user
  was *told* to do; a `did` or `skip` row carrying the same id is what they
  actually did. Never rewrite a row in place to mark it done — these hooks fire
  in every project at once, and two sessions editing one file will eat it.
  Two-column rows predate the status column and count as unconfirmed.
- `exercises.txt` — one exercise per line; edit freely
- `off` — kill switch; nudges stop while this file exists
- `last-nudge`, `last-exercise`, `pending`, `outstanding` — internal, leave alone

## Responding

**report / tally / streak / "how am I doing"**

```bash
A=~/.claude/fitness/log.tsv; T=$(date +%F)
awk -F'\t' -v d="$T" 'index($1,d)!=1{next} NF<=2||$3=="set"{s[$2]++;next} $3=="did"{k[$2]++}
  END{for(e in s) printf "%d\t%d\t%s\n",s[e],k[e]+0,e}' "$A" | sort -rn
awk -F'\t' '{d=substr($1,1,10);seen[d]=1} NF<=2||$3=="set"{n[d]++;next} $3=="did"{c[d]++}
  END{for(d in seen) printf "%s %d nudged %d confirmed\n",d,n[d]+0,c[d]+0}' "$A" | sort | tail -7
```

Read the numbers back as prose, not raw output. Mention the day's total and
anything notable — a gap, a strong run. Don't congratulate mechanically.

Keep *nudged* and *confirmed* clearly apart; they are not the same claim and the
first one is not an achievement. Do not turn them into a completion percentage:
a set the user did and never mentioned looks identical to one they skipped, so
the denominator means nothing. "Four confirmed today" is honest. "31% adherence"
is not.

**off / on** — `touch ~/.claude/fitness/off` or `rm -f ~/.claude/fitness/off`.
Takes effect immediately, no restart. Confirm in one line.

**every \<n\> minutes / more often / less often** — the cooldown is the env var
`CLAUDE_FITNESS_COOLDOWN`, in seconds, default 480. Set it in the `env` block of
`~/.claude/settings.json`. Say that it needs a new session to take effect.

**quiet** — `CLAUDE_FITNESS_VOICE=0` the same way. Beeps stay, speech stops.

**stop tracking what I did** — `CLAUDE_FITNESS_CONFIRM=0` the same way. Nudges
carry on; the open set is no longer mentioned and nothing is confirmed.

**easier / harder / desk only / legs only / no floor work** — rewrite
`exercises.txt` to suit. Each line must be a short imperative phrase that reads
naturally when spoken aloud by `say`: "10 push-ups", not "Pushups x10". Keep
8–15 entries so repeats feel rare. Show the user the new list.

**add \<exercise\>** — append a line. **list** — print the file.

**did / skip / "done" / "I did those"** — close the open set:

```bash
claude-fitness did    # or: claude-fitness skip
```

Both are no-ops when nothing is open, so they are safe to call blind.

## Logging what was actually done

While a set is open, the `UserPromptSubmit` hook puts a line in your context
naming it. That line is your only cue, and it exists so the user can confirm by
just saying so in passing — not by running a command.

Two rules here matter more than anything else in this skill:

- **Only log what the user actually claimed.** Never infer that a set was done
  because time passed, because they sent another message, or because they sound
  energetic. A fabricated `did` is worse than no record at all: the single
  reason the column is worth having is that the number can be trusted.
- **Never ask.** Don't open a turn with "did you do your squats?". Either they
  mention it or they don't. A question on every turn gets tuned out within a
  day, for exactly the reason the `Stop` beep is gated. Telling is not asking:
  the hook adds "and that they can just say when it is done" to the first few
  announcements so the user learns it exists, and drops it after three. If that
  clause is not in the instruction you were given, don't add it yourself.

## Gotchas worth preserving

- The `nudge` path must always exit 0. Exit code 2 on `UserPromptSubmit` blocks
  the user's prompt entirely.
- Keep hooks fast and all audio backgrounded (`( ... & )`). Hooks are
  synchronous; an unbackgrounded `say` stalls the turn until it stops talking.
- `shuf` is not on macOS. The script uses a `$RANDOM` index instead.
- Audio differs per platform. If the user reports no sound, run
  `bash scripts/claude-fitness platform` — it prints the detected OS, which
  player and which speech engine were found. On Linux, voice needs
  `espeak-ng` installed; missing audio is never fatal.
- Hooks here fire in every project. To scope them, guard on
  `$CLAUDE_PROJECT_DIR` inside the `nudge` case.

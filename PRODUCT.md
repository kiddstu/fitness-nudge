# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Stack

delegated: plain static HTML + CSS, one file, no build step. The repository is a
Claude Code plugin made of bash and JSON; it has no Node toolchain of its own and
the 118MB `node_modules` currently present belongs to a one-off video build, not
to the product. GitHub Pages serves the page from `docs/` on the default branch,
so a framework would add a build to a repo that otherwise has none. Offered to
the user as a recommendation rather than asked; revisit if the page grows past
one surface.

## Users

Developers who use Claude Code (or a comparable coding agent) and who sit still
for long stretches while the agent works. On this surface specifically: a
Product Hunt or Hacker News visitor who arrived from a link, has roughly five
seconds of patience, and is deciding whether this is a real tool or a joke
repository. They are technical, allergic to marketing language, and likely
scrolling on a phone with sound off.

## Product Purpose

Converts the dead time while a coding agent works into movement. On every task
hand-off it picks a small exercise, announces it out loud, logs it, and beeps
when the agent needs the user back. Success is the user doing sets they would
not otherwise have done, and not disabling it within a week.

## Positioning

The mechanism is hooks, not a skill. A skill is instructions to the model, so it
depends on the model's attention and drifts within a few turns; these are
`UserPromptSubmit`, `Notification` and `Stop` hooks executed by the harness, so
they fire whether the model is paying attention or not. A competitor built as a
prompt or skill could not truthfully claim this. The second non-copyable claim is
the gated `Stop` beep: it fires only when a nudge is actually outstanding, which
is what stops the user tuning the sound out within a day.

## Operating Context

Used inside a terminal session, eyes on a running agent. The core experience is
**audio** — a spoken exercise plus two distinct beeps — and there is no UI to
screenshot. State lives in `~/.claude/fitness/` (`exercises.txt`, `log.tsv`, an
`off` kill-switch file). Tuning happens either by asking Claude via the bundled
`/fit` skill or by editing those files directly. Hooks fire in every project, not
just one.

## Capabilities and Constraints

- Three hooks: nudge on `UserPromptSubmit` (cooldown-gated, default 480s), glass
  beep on `Notification`, tink beep on `Stop` (only when a nudge is outstanding).
- macOS, Linux and Windows. Audio is the only platform-varying part: `afplay`/`say`
  on macOS, PowerShell `SystemSounds`/`System.Speech` on Windows,
  `paplay`/`espeak-ng` on Linux. Linux is the only platform needing an install
  (`espeak-ng`). WSL is detected and routed through PowerShell.
- Missing audio is never fatal: the exercise is still announced in text, logged
  and tallied.
- The nudge path always exits 0 by design, because exit 2 on `UserPromptSubmit`
  blocks the user's prompt.
- Windows needs `bash` on PATH, which Git for Windows provides.
- MIT. Version 0.1.0.
- **Undecided:** the author name in the plugin manifests still reads `kidd` with
  no email. Not a design decision; left for the user.

## Brand Commitments

- Name is lowercase `fitness-nudge` throughout, including in prose.
- Voice is deadpan: dry, specific, no exclamation, no emoji in body copy, and no
  claim the product cannot make. The humour comes from the premise being
  presented completely straight. This was set in the launch video and holds here.
- Binding visual constraint carried from the launch video so page and film read as
  one system: ground `#0b0b0c`, ink `#e6e4e1`, dim `#6b6a68`, amber `#d9a05b`,
  green `#86b06b`, JetBrains Mono.

## Evidence on Hand

- `brag-output/brag.mp4` — 23.2s 1080p launch film, real product audio, poster
  baked as frame 0. Currently gitignored.
- `brag-output/brag.jpg` — poster frame.
- `brag-output/composition/assets/audio/` — the genuine sounds the plugin plays:
  `ping.wav`, `glass.wav`, `tink.wav`, and `nudge.wav` (macOS `say` speaking
  "ten push ups").
- `.github/workflows/smoke.yml` — green CI across macOS, Windows and Ubuntu; the
  README states plainly what it does not cover (audible playback).
- Real terminal transcript shape in the README.
- **Absent, must not be fabricated:** any user count, install count, star count,
  testimonial, press mention, benchmark, or named adopter. The project is new and
  has none. No Product Hunt placement or HN rank exists yet.

## Product Principles

1. **Determinism over attention.** The thing fires because the harness runs it,
   not because a model remembered. Every claim on every surface must respect that
   distinction.
2. **The product is audio.** Any surface that cannot be heard has not shown the
   product. Visuals are a frame around a sound.
3. **State the limits.** The README names what CI does not cover and that hooks
   fire in every project. Hiding a limitation would cost more credibility with
   this audience than the limitation does.
4. **Deadpan, never zany.** The premise is funny on its own; decorating it kills
   it.
5. **Nothing invented.** No numbers, no social proof, no borrowed logos.

## Accessibility & Inclusion

The product itself degrades to text when no audio exists, which is the stance the
page must match: an audio-first surface may never make sound the *only* carrier of
meaning. Everything audible must also be readable, controls must be real focusable
elements, and nothing may autoplay sound. Target WCAG AA contrast on the dark
ground.

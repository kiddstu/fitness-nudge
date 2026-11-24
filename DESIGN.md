---
name: fitness-nudge
description: A near-black terminal room where amber is the only colour that means sound.
colors:
  ground: "#0b0b0c"
  ink: "#e6e4e1"
  dim: "#807e7a"
  amber: "#d9a05b"
  amber-hover: "#e8b473"
  green: "#86b06b"
  rule: "rgba(230,228,225,.09)"
  rule-strong: "rgba(230,228,225,.17)"
  raise: "rgba(230,228,225,.028)"
  rail-veil: "rgba(11,11,12,.86)"
  scroll-thumb: "#2a2927"
  scroll-thumb-hover: "#3d3b39"
typography:
  display:
    fontFamily: "JetBrains Mono, ui-monospace, SFMono-Regular, Menlo, Consolas, monospace"
    fontSize: "clamp(29px, 5.2vw, 50px)"
    fontWeight: 700
    lineHeight: 1.16
    letterSpacing: "-0.025em"
  headline:
    fontFamily: "{typography.display.fontFamily}"
    fontSize: "clamp(23px, 3.2vw, 32px)"
    fontWeight: 700
    lineHeight: 1.25
    letterSpacing: "-0.022em"
  quote:
    fontFamily: "{typography.display.fontFamily}"
    fontSize: "clamp(19px, 2.6vw, 26px)"
    fontWeight: 700
    lineHeight: 1.3
    letterSpacing: "-0.01em"
  title:
    fontFamily: "{typography.display.fontFamily}"
    fontSize: "16px"
    fontWeight: 700
    lineHeight: 1.4
    letterSpacing: "-0.012em"
  body:
    fontFamily: "{typography.display.fontFamily}"
    fontSize: "15px"
    fontWeight: 400
    lineHeight: 1.75
    letterSpacing: "normal"
  data:
    fontFamily: "{typography.display.fontFamily}"
    fontSize: "14px"
    fontWeight: 400
    lineHeight: 2
    letterSpacing: "normal"
  meta:
    fontFamily: "{typography.display.fontFamily}"
    fontSize: "12.5px"
    fontWeight: 400
    lineHeight: 1.6
    letterSpacing: "normal"
  label:
    fontFamily: "{typography.display.fontFamily}"
    fontSize: "12px"
    fontWeight: 700
    lineHeight: 1
    letterSpacing: "0.09em"
rounded:
  none: "0"
  hairline: "1px"
  pill: "50%"
spacing:
  s1: "8px"
  s2: "16px"
  s3: "24px"
  s4: "40px"
  s5: "64px"
  s6: "96px"
  s7: "136px"
  gut: "clamp(20px, 4vw, 48px)"
  wide: "1120px"
components:
  button-copy:
    backgroundColor: "{colors.amber}"
    textColor: "{colors.ground}"
    typography: "{typography.label}"
    rounded: "{rounded.none}"
    padding: "0 18px"
  button-copy-hover:
    backgroundColor: "{colors.amber-hover}"
    textColor: "{colors.ground}"
  button-copy-done:
    backgroundColor: "{colors.green}"
    textColor: "{colors.ground}"
  button-sound:
    backgroundColor: "transparent"
    textColor: "{colors.amber}"
    rounded: "{rounded.pill}"
    size: "38px"
  button-sound-pressed:
    backgroundColor: "{colors.amber}"
    textColor: "{colors.ground}"
    rounded: "{rounded.pill}"
    size: "38px"
  press-target:
    backgroundColor: "transparent"
    textColor: "{colors.ground}"
    rounded: "{rounded.pill}"
    size: "min(300px, 34vh)"
  command-strip:
    backgroundColor: "{colors.raise}"
    textColor: "{colors.ink}"
    typography: "{typography.data}"
    rounded: "{rounded.none}"
    padding: "13px 18px"
  rail:
    backgroundColor: "{colors.rail-veil}"
    textColor: "{colors.dim}"
    rounded: "{rounded.none}"
    height: "64px"
---

# Design System: fitness-nudge

## Overview

**Creative North Star: "The Listening Room"**

A near-black room with one thing in it to press. The product has no interface — it
is a spoken sentence and three beeps — so the visual system's whole job is to be a
quiet, credible frame around a sound and then get out of the way. Everything is set
in one monospaced face because every string on these surfaces genuinely is one: a
command, a filename, an event name, or a line of transcript. Nothing is decorated;
structure is drawn with hairlines rather than boxes, and the single bright element
on any screen is the thing that makes noise.

This world was not invented for the landing page. It was established by the launch
film (`brag-output/composition/`) and the page inherits it so the two read as one
system. Record it as shared: the next asset made for this project — a second page,
a social card, a follow-up film — takes its ground, ink, amber, green and face from
here, not from a fresh decision.

Density is low and deliberate. Sections are separated by a 136px rhythm and a single
hairline, never by a container. The page is comfortable being mostly empty; the
emptiness is what makes one amber disc read as the only object in the room.

**Key Characteristics:**
- Near-black ground (#0b0b0c), one accent, one reserved state colour.
- One typeface, two weights (400/700), self-hosted.
- Hairlines at 9% and 17% ink instead of cards, panels or boxes.
- Exactly one shadow in the entire system.
- Browser surfaces (selection, caret, focus ring, scrollbar) are themed, not defaulted.
- Icons are authored inline SVG at one stroke weight, referenced by `<use>`.

## Colors

A near-black monochrome with two colours that carry meaning and nothing that carries
mood.

### Primary
- **Nudge Amber** (frontmatter `amber`): the only colour that ever means *sound*. It is
  the concentric rings of the press target, the spoken-quote type, the sound icon in
  transcripts, the event names in the sound list, the first column of every table, the
  emphasis word inside headings, the copy button fill, the caret, the text-selection
  ground and the focus ring. 8.55:1 on the ground as text; 8.55:1 for ground-on-amber
  when it is a fill.
- **Amber Lift** (`amber-hover`): hover fill for the solid amber button only. Never a
  rest state.

### Secondary
- **Detected Green** (`green`): reserved for *detected / working / confirmed* and for
  nothing else — Claude's own transcript lines, the `/plugin` keyword inside commands,
  built-in platform players in the support tables, and the copy button's post-copy
  confirm. 7.9:1 on the ground.

### Neutral
- **Room Black** (`ground`): the page ground, and the text colour that sits *on* amber
  or green fills.
- **Terminal Ink** (`ink`): all primary text, headings, and the user's own transcript
  lines. 15.5:1.
- **Readable Dim** (`dim`): secondary text — lede, body paragraphs, captions, nav at
  rest, table headers, metadata, footer. 4.86:1.
- **Hairline** (`rule`) and **Hairline Strong** (`rule-strong`): 9% and 17% ink. Rule
  is every section divider, row divider and the rail's underline; rule-strong is the
  border of a thing you can interact with (the command strip, the sound buttons, the
  table head).
- **Raise** (`raise`): a 2.8% ink wash. The only "surface" in the system — the command
  strip's fill and a row's hover state. It is a breath above the ground, not a panel.
- **Rail Veil** (`rail-veil`), **Scroll Thumb** / **Scroll Thumb Hover**: the themed
  browser-and-chrome layer.

### Named Rules

**The Sound Is Amber Rule.** Amber means sound and only sound. If an element does not
make, represent, or trigger audio, it is not amber. This is why the primary action
(copy) can be a solid amber fill and a decorative heading word cannot be amber unless
it names what is heard.

**The Green Is Earned Rule.** Green means something was detected, is working, or
succeeded. It is never a second accent, never a palette-balancing choice, and never
used for emphasis.

**The Readable Dim Rule.** The film's dim is `#6b6a68`; the page ships `#807e7a` and
`#807e7a` is canonical for text. `#6b6a68` measures 3.64:1 on `#0b0b0c` and fails
WCAG AA at the 12.5–15px sizes this role is actually used at; `#807e7a` is the same
hue and role at 4.86:1. Do not "correct" page dim back to the film value. If a future
asset renders dim as a large non-text graphic element, the film value is still fine
there — the override is about body copy.

## Typography

**Display Font:** JetBrains Mono 700, self-hosted woff2 (`docs/fonts/`)
**Body Font:** JetBrains Mono 400, same source
**Fallback stack:** `ui-monospace, SFMono-Regular, Menlo, Consolas, monospace`

**Character:** One face, two weights, ligatures explicitly off. Headings get negative
tracking (-0.012em to -0.025em) so monospace reads as a voice rather than as code at
display sizes; body is left at normal tracking and a generous 1.65–1.85 leading so
long explanatory paragraphs stay readable in a fixed-pitch face.

### Hierarchy
- **Display** (700, `clamp(29px, 5.2vw, 50px)`, 1.16, -0.025em, max 30ch): the hero
  headline, once per page. Balanced wrap below 620px.
- **Headline** (700, `clamp(23px, 3.2vw, 32px)`, 1.25, -0.022em, max 22ch, balanced):
  every section opener. Section headings are full sentences, not labels.
- **Quote** (700, `clamp(19px, 2.6vw, 26px)`, 1.3, amber): the spoken line, rendered as
  type. The only place display-weight amber type appears.
- **Title** (700, 16px, -0.012em): the name of a sound or a listed item.
- **Body** (400, 15px, 1.75, max 65ch; 1.7 at 56ch for the hero lede): all prose, in
  dim. Inline `<code>` and emphasis step *up* to ink rather than changing size.
- **Data** (400, 14px, line-height 2 for transcripts and 1.85 inside command blocks):
  transcripts, commands, tables. 13px at ≤620px.
- **Meta** (400, 12.5–13.5px, dim): captions, row descriptions, event labels, footer.
- **Label** (700, 12–13px, uppercase, 0.09em–0.16em tracking): only on controls — the
  copy button (12px/0.09em) and the press target's face (13px/0.16em). Table headers
  use the same uppercase treatment at 400 weight, 12px, 0.1em, in dim.

### Named Rules

**The Everything Is A String Rule.** One typeface for the whole system. Monospace is
not a costume here: every string on the surface is a command, a path, a hook name, or
a transcript line. A second face would be a second voice, and there is only one.

**The Caps Means Control Rule.** Uppercase with open tracking is reserved for controls
and column headers. Never set a heading, a sentence, or a product name in caps — the
name is lowercase `fitness-nudge` everywhere, including in the rail.

## Layout

One centred column: `max-width: 1120px` with a fluid gutter of
`clamp(20px, 4vw, 48px)`, applied by a single `.wrap` that every band reuses. No grid
framework, no nested containers.

The vertical rhythm is a seven-step scale (8 / 16 / 24 / 40 / 64 / 96 / 136px). Section
padding is the top of the scale (136px, dropping to 96px at ≤720px); within a section,
heading→prose is 16px, prose→artifact (transcript, table, sound list) is 40px, and the
close band's metadata row sits 64px below the action. Tight inside a group, generous
between groups.

The hero is a `100svh` flex column under a fixed 64px rail, with the content block
optically centred and the first two transcript lines cropped at the bottom by a
52–64px masked band — scrolling is offered by a half-visible object rather than
announced by an arrow. That band hides entirely below 700px of viewport height rather
than stealing room from the hero.

Breakpoints are three, each with a job: **720px** collapses the rail's section links to
the GitHub link only, reduces section padding, and turns tables into stacked
label/value blocks via `data-th` pseudo-content; **620px** stacks the command strip so
the install lines wrap instead of scrolling out of sight, lets transcripts wrap with a
hanging indent, and compresses hero spacing so the headline, target and action still
land in one phone viewport; **560px** shrinks the press target. Body measure never
exceeds 65ch at any width.

### Named Rules

**The Hairline Rule.** Structure is drawn with 1px rules at 9% ink (dividers) and 17%
ink (interactive edges). There are no cards, no panels, no filled containers and no
rounded surfaces. If something needs to be set apart, give it a rule above it and
space around it.

**The Action Never Scrolls Rule.** The install command is the page's action; where the
column is too narrow it wraps and the button stacks under it. It is never put in a
horizontally scrolling region.

## Elevation & Depth

The system is flat. Depth comes from three things: the 2.8%-ink `raise` wash, hairline
borders, and a translucent blurred rail that lets the page pass beneath it
(`rgba(11,11,12,.86)` + `blur(10px)`). Nothing else lifts.

### Shadow Vocabulary
- **Press lift** (`box-shadow: 0 16px 42px -18px rgba(0,0,0,.95)`): under the solid
  amber disc of the hero press target. Offset down, wide blur, negative spread, so it
  reads as the one physical object in the room sitting slightly off the ground.

### Named Rules

**The One Shadow Rule.** There is exactly one shadow in this system and it belongs to
the hero press target. Any new surface that wants a shadow is asking for a hairline
instead.

## Shapes

Two shapes only: the rectangle with square corners (radius 0) and the perfect circle
(radius 50%). Rectangles are dividers, command strips and tables; circles are
everything that makes sound — the three concentric rings of the press target (50% /
72% / 94% of a `min(300px, 34vh)` box, the inner one solid amber, the outer two at 30%
and 12% opacity) and the 38px play/stop buttons in the sound list. The favicon is the
same figure: a filled amber dot inside two fading amber rings.

The only other radii in the build are functional, not expressive: 1px on the focus
ring and 6px on the custom scrollbar thumb.

### Named Rules

**The Circle Means Sound Rule.** Round geometry is reserved for audio controls. A
circle in this system is always something you can press to hear.

## Components

### Buttons
- **Copy (primary action):** solid amber fill, ground-coloured uppercase label at 12px
  / 0.09em with a 13px inline SVG icon, square corners, `0 18px` padding, flush against
  the command strip's right edge and separated from it by a 17%-ink hairline. Hover
  lifts the fill to Amber Lift (0.18s). On success the fill becomes green, the label
  becomes "copied" and the icon swaps to a tick for 1.9s. If the clipboard API refuses,
  it selects the command text and says so — the control always names its real state.
- **Sound play/stop:** 38px circle, transparent, 17%-ink border, amber icon. Row hover
  brings the border to amber and washes the circle in 10% amber. The pressed state is
  driven by `[aria-pressed="true"]` — solid amber fill, ground icon — so the exposed
  state and the visual state cannot drift apart.
- **Rail links:** dim at rest, ink on hover (0.18s). The GitHub link is underlined; the
  section links are not.
- **Text links:** ink with a 1px dim underline at 4px offset; hover turns the underline
  amber. The colour never changes, only the rule under it.

### Command strip
- **Shape:** square corners, 17%-ink border, `raise` fill.
- **Content:** the literal install lines at 14px / 1.85, with `/plugin` in green.
- **Internal padding:** 13px 18px.
- **Responsive:** below 620px it becomes a column and the lines wrap.

### Tables
- **Head:** uppercase 12px dim labels at 0.1em over a 17%-ink rule.
- **Rows:** 9%-ink rule under each; first cell amber and non-wrapping; `.ok` green for
  a built-in capability, `.need` ink for one you must install.
- **Below 720px:** rows become stacked blocks and each cell prints its own column name
  from `data-th` as an 11px uppercase dim label.

### Transcript
A flex-row list at 14px / line-height 2: a 62px dim speaker gutter, then the line. You
is ink, Claude is green, a spoken nudge is amber 700 inside typographic quotes preceded
by the 15px amber sound glyph, and system asides are dim. Below 620px lines wrap with
the gutter narrowed to 46px so the speaker column still reads as a hanging indent.

### Sound list
Three-column rows (52px control / text / right-aligned event), 24px gap, 20px vertical
padding, 9%-ink rule between. Hover washes the row in `raise`. The event column is dim
with its hook name on a second line in amber. Below 620px the event drops under the
text at left-aligned.

### Icons
An authored inline `<svg><defs>` block at the top of the body, visually hidden, holding
five symbols (play, stop, copy, tick, sound) referenced by `<use href="#i-…">`. Stroke
icons share 1.4–1.8px weight with round caps on a 14px box; filled icons are solid
`currentColor`. State changes swap the `href`, not the element. No icon font, no icon
library, no emoji, no Unicode glyph.

### Press target (signature component)
The page's one authored object. A `min(300px, 34vh)` circular button holding three
concentric amber rings — solid inner disc at 50% with the press lift beneath it, a 30%
ring at 72%, a 12% ring at 94% — with an uppercase ground-coloured face reading
"hear it" / "playing" / "again". Hover scales the rings 1.025 / 1.03 / 1.04 over 0.4s.
Press runs the one authored motion in the system and plays the product's real audio.
Its caption line and the amber spoken quote occupy the same 46px slot and cross-fade.

### Degradation
With `.no-js` on the root element the press target and sound buttons drop to 40%
opacity and stop accepting pointer events, the copy button is removed, and the
cross-faded slot becomes static so the spoken quote and its caption both show at once.
Nothing audible is ever the only carrier of meaning: the quote is on the page as type,
and pressing announces it through an `aria-live` status region as well.

### Named Rules

**The Aria Is The Styling Rule.** Toggle state is selected on the ARIA attribute
(`.btn[aria-pressed="true"]`), never on a parallel class. If you add a toggle, style it
from the attribute that screen readers read.

**The One Authored Moment Rule.** There is one piece of real motion: a three-pulse
ripple on press, each pulse a 1.4s `cubic-bezier(.16,1,.3,1)` scale from 0.5 to 1.5 with
opacity 0.6→0, staggered 0ms / 240ms / 540ms. Everything else is a 0.18–0.4s ease on
hover or focus. No scroll-triggered entrances, no section reveals.
`prefers-reduced-motion: reduce` collapses all animation and transition to 0.01ms and
turns off smooth scrolling.

**The Themed Chrome Rule.** The parts the browser draws still belong to the system:
selection is amber with ground-coloured text, the caret is amber, the focus ring is a
2px amber outline at 3px offset with a 1px radius, and the scrollbar is an 11px track
in ground with a `#2a2927` thumb (`#3d3b39` on hover) inset 3px. A new surface themes
all four or it is not finished.

## Do's and Don'ts

### Do:
- **Do** use amber for anything that makes, represents or triggers sound, and nothing
  else.
- **Do** keep green for detected / working / succeeded states only.
- **Do** use `#807e7a` for dim text on the near-black ground, including on future
  assets, and keep every text pair at WCAG AA or better.
- **Do** separate things with a 1px rule at 9% ink and space from the seven-step scale.
- **Do** set every size in JetBrains Mono at 400 or 700, self-hosted from
  `docs/fonts/`, with ligatures off.
- **Do** give headings negative tracking (-0.012em to -0.025em) and a `ch`-capped
  measure (30ch display, 22ch headline, 65ch body).
- **Do** draw new icons as inline SVG symbols in the existing `<defs>` block at the
  same stroke weight, and swap `href` to change state.
- **Do** style toggle state from `aria-pressed`, and give every audible thing a readable
  equivalent on the page.
- **Do** theme selection, caret, focus ring and scrollbar on any new surface.

### Don't:
- **Don't** add a card, panel, filled container or rounded surface. Hairline plus space
  is the container.
- **Don't** add a second shadow. The press lift is the only one.
- **Don't** introduce a third colour, a second accent, or a gradient — including
  gradient text.
- **Don't** set the product name, a heading or a sentence in uppercase; caps belong to
  controls and column headers.
- **Don't** use an emoji, an icon font, or a Unicode glyph where an icon belongs.
- **Don't** put a kicker, eyebrow or section number above a heading; the headline is a
  full sentence and carries itself.
- **Don't** add scroll-triggered animation or a second authored motion. One moment,
  one ripple.
- **Don't** autoplay audio, or let sound be the only carrier of any meaning.
- **Don't** revert dim to the film's `#6b6a68` for text, and don't let a new value into
  `:root` without checking it against the ground at the size it will actually be read.

---
name: slop-mop
description: Mops the AI slop out of app UI. Use when building, reviewing or polishing screens in iOS (SwiftUI), Android (Compose) or web apps, or when a user says the UI looks generic, cluttered, "AI-made" or "sloppy". Gives a delete-first process, a list of slop tells, and a 20-point review checklist backed by Apple HIG, Material 3, NN/g, Refactoring UI and Rams.
---

# slop-mop

You are cleaning up product UI so it looks deliberate, native and calm instead of generic and
AI-generated. The full research, with reasons and sources, is in
[design-guide.md](design-guide.md) next to this file. Read the sections you need; the rules
below are the short version.

## Ground rules

1. **The project's own design rules win.** Look for a design system, tokens, a style guide,
   a CLAUDE.md or anything the user has said about taste. This skill fills the gaps.
2. **Delete before you decorate.** The default verdict for any element that isn't needed is
   delete. Restructure (grouping, order, alignment) before restyling. Recolouring a cluttered
   layout is lipstick.
3. **Native beats clever.** Use stock platform components and copy the first-party apps
   (Apple Maps, Settings, Fitness; Google Maps, system Settings) before inventing anything.
4. **One screen at a time.** No sweeping global restyles.
5. **Product UI only.** Marketing pages and games are allowed a point of view (custom type,
   playful motion). Don't flatten a landing page into a settings screen. See guide §13.11.

## The process

For each screen:

1. **Render before.** Look at the real screen: screenshot it in light, dark and the largest
   text size if you can (simulator, emulator, headless browser). Never redesign from code
   alone. If you truly can't render, say so and read the view code closely.
2. **Write a 3-line brief.**
   - Job: what the person comes here to do (one sentence, a verb).
   - Primary action: the single thing that gets the accent, or "none".
   - Reference app: a first-party app that already solves this layout (guide §2).
3. **Inventory every element** and mark it **keep / merge / demote / delete**. Default is
   delete. Show this list to the user before making big structural changes.
4. **Restructure, then restyle.** Grouping and alignment first, then type, then colour.
5. **Re-render** light, dark and largest text. Check nothing clips or overlaps.
6. **Score** with the checklist below. Report what you deleted, with before/after if possible.

## Hard defaults (unless the project says otherwise)

- **Lists, not cards**, for homogeneous items. Row = optional glyph → title → one secondary
  line → trailing value or chevron. Hairline separators.
- **One container level.** Never a card in a card.
- **Leading-align** text, stats and empty states. Centre only a lone short element.
- **Spacing scale 4 / 8 / 12 / 16 / 24 / 32 / 48**, and more space between groups than inside.
- **System fonts, platform text styles, ≤3 sizes and ≤2 weights per screen.** No light/thin
  weights. Tabular figures for numbers that change.
- **Neutrals first, one accent** with a written job list (e.g. primary action, on-state
  controls, completed items). Never an accent wash behind content.
- **One prominent button per screen.** Verb labels ("Save", "Start"). Destructive actions are
  never primary.
- **Bare glyphs** in secondary grey. No tinted icon discs, no icons on section headings.
- **The state change is the confirmation.** No success toasts. Errors inline, in plain words.
- **Empty state = one plain sentence**, optionally one text action. Never "No results" while
  loading.
- **Motion needs a purpose**, stays under ~300ms, is interruptible, respects Reduce Motion.
  Frequent actions barely animate. Pick a technique per moment (layout transition, spring,
  gesture...) instead of "add animations" (guide §8).
- **Copy:** short, concrete, one case style, no "we", no exclamation marks, no marketing
  adjectives, numbers over adjectives.

## Slop tells (delete on sight)

- Bordered/shadowed card around every row; cards in cards; everything centred
- Hero block or big greeting at the top of an in-app screen; three equal feature cards
- Uniform padding everywhere; vertical rules between stats
- Gradients on cards, buttons, avatars, text; more than one accent; accent on decoration
- Tinted icon discs; the same icon on every row; sparkle/zap/arrow glyphs; emoji bullets
- "NEW" / "HOT" / "PRO" pills; badges nobody needs
- Success toasts, "Thanks!", confetti after ordinary actions; errors as toasts
- Fade-in-up on every element; press wobble, pulse, glow, shimmer; blinking dots
- "Amazing", "seamless", "elevate"; subtitles that repeat the heading; "Oops!", "Let's…"
- Five font sizes and three weights on one screen; proportional digits in live numbers

The full list with sources is guide §10.

## Review checklist

Score 0/1 each. Ship at 18+ of 20, and no fails on 1–4.

1. The project's own design rules hold.
2. The 3-line brief exists and the screen matches it.
3. Zero or one prominent accent action; the accent only does its listed jobs.
4. Renders correctly in light, dark and largest text on every platform.
5. Content is leading-aligned; nothing is centred without a reason.
6. Spacing on the 4/8 scale; between-group > within-group.
7. At most one container level; no per-row cards.
8. ≤3 text sizes, ≤2 weights, all via text styles.
9. Hierarchy reads in greyscale (squint test).
10. Text contrast ≥4.5:1 in light and dark.
11. No state shown by colour alone.
12. Icons are bare glyphs, labelled where meaning isn't universal.
13. No badges/pills unless critical.
14. No success toasts; errors inline.
15. Motion is purposeful, fast, interruptible, Reduce Motion respected.
16. Copy: verb buttons, one case style, no filler, no "we", no "!".
17. Empty, loading and error states all designed.
18. Touch targets ≥44pt (iOS) / 48dp (Android).
19. Stock platform components; nothing looks like a web page in a frame.
20. Elements deleted this pass outnumber elements added.

## Where to look in the guide

| Need | Section |
|---|---|
| Reference apps per screen type | §2 |
| Spacing, grouping, alignment, safe areas | §3 |
| Type scale, Dynamic Type, numbers | §4 |
| Colour, accent budget, dark mode | §5 |
| Lists, sheets, bars, tabs, buttons, empty states, icons, stats, maps | §6 |
| SwiftUI / Compose / web specifics, what not to cross-port | §7 |
| Motion, techniques, Rive/Lottie | §8 |
| Copy | §9 |
| Worked layouts (feed, map, profile, store, leaderboard, editor) | §11 |
| When Apple, Material and the essays disagree | §13 |

## Reporting

When you finish a screen, tell the user in a few lines: the brief, what you deleted, merged
or demoted, the checklist score, and anything you left alone because it needs their call.

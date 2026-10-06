# 🧹 slop-mop

**A Claude skill that mops the AI slop out of your app's UI.**

You know the look. A bordered card around every row. A tinted circle behind every icon.
Gradients on the buttons, a "Welcome back! 👋" hero on a settings screen, three font weights
fighting for attention, and a little "Saved! ✨" toast every time you breathe.

slop-mop teaches Claude to see that and clean it up: delete first, restructure second,
restyle last. It's a short skill backed by a long, sourced design guide built from Apple's
Human Interface Guidelines, Material 3, Nielsen Norman Group, Refactoring UI, Butterick's
Practical Typography, Dieter Rams, Emil Kowalski, Rauno Freiberg and Linear's redesign notes.

Works for **iOS (SwiftUI)**, **Android (Jetpack Compose / Material 3)** and **web apps**.

---

## What's in the bucket

| File | What it is |
|---|---|
| [`skills/slop-mop/SKILL.md`](skills/slop-mop/SKILL.md) | The skill Claude loads: ground rules, a 6-step process, hard defaults, slop tells and a 20-point checklist. |
| [`skills/slop-mop/design-guide.md`](skills/slop-mop/design-guide.md) | The full research guide (44 cited sources). Readable on its own, no Claude required. |

Just want the guide? [Download design-guide.md](https://raw.githubusercontent.com/Mysz0/slop-mop/main/skills/slop-mop/design-guide.md)
and read it like a normal person.

## What it does

When Claude works on UI with this skill loaded, it:

1. **Looks at the real screen first** (light, dark, largest text) instead of redesigning from code.
2. **Writes a 3-line brief:** the screen's job, its one primary action, and a first-party app
   that already solves the layout.
3. **Inventories every element** as keep / merge / demote / delete, with **delete as the default**.
4. **Restructures before restyling:** grouping, alignment and order before colour.
5. **Re-checks** light, dark and accessibility text sizes.
6. **Scores the screen** on a 20-point checklist and tells you what it removed.

It also knows things like:

- Lists beat cards for anything homogeneous ([NN/g](https://www.nngroup.com/articles/cards-component/)).
- One prominent button per screen, one accent colour with a written job list.
- Three text sizes and two weights per screen is plenty.
- The state change *is* the confirmation. No success toasts. Errors inline.
- Motion needs a purpose, stays under ~300ms, and frequent actions barely animate.
- What not to cross-port between iOS and Android.
- Where Apple, Google and the design essays disagree, and a sane default for each.

And it knows when to back off: **marketing pages and games are allowed to have fun.** The
skill is about product screens people use every day, not about flattening your landing page.

## Install

### Option 1: copy the folder (any Claude Code setup)

```bash
git clone https://github.com/Mysz0/slop-mop.git
mkdir -p ~/.claude/skills
cp -r slop-mop/skills/slop-mop ~/.claude/skills/
```

That's it. Claude Code picks up skills from `~/.claude/skills/<name>/SKILL.md`.
For a single project instead of everywhere, copy it to `.claude/skills/slop-mop/` inside that repo.

### Option 2: as a Claude Code plugin

```
/plugin marketplace add Mysz0/slop-mop
/plugin install slop-mop@slop-mop
```

### Option 3: Claude.ai / Claude desktop

Zip the `skills/slop-mop` folder and upload it under **Settings → Capabilities → Skills**.

## Use it

Claude loads the skill on its own when you're working on UI. You can also just ask:

```
> use slop-mop on the settings screen

> this profile page looks like AI made it. mop it.

> review the onboarding flow with the slop-mop checklist, don't change anything yet
```

### Example

You:

> The home screen looks cluttered, clean it up.

Claude (with slop-mop):

> **Brief.** Job: see what's new nearby. Primary action: none. Reference: Apple Maps search results.
>
> **Inventory.**
> - Delete: "Good morning, Alex 👋" hero, tinted icon discs on every row, the "NEW" pills,
>   the sparkle icon on the section heading, the success toast after favouriting.
> - Merge: the three metadata chips per row → one secondary line ("Café · 240 m · Open").
> - Demote: the "See all" button → a plain text button.
> - Keep: the list itself, the search field.
>
> Cards became a hairline list, everything is leading-aligned, the screen went from five font
> sizes to three. Checked in light, dark and the largest text size.
>
> **Score: 19/20.** The one miss: the empty state still says "No results" while loading. Want
> me to fix that too?

## Credits

The guide stands on the shoulders of: Apple Human Interface Guidelines, Material Design 3 and
the Android developer docs, Nielsen Norman Group, Refactoring UI (Adam Wathan & Steve
Schoger), Matthew Butterick, Dieter Rams / Vitsœ, Jon Yablonski's Laws of UX, Jakob Nielsen,
Rauno Freiberg, Emil Kowalski, Linear, Matt Ström-Awn, 925 Studios, the
[avoid-ai-design](https://github.com/funboy322/avoid-ai-design) project and the Rive docs.
Full links are at the bottom of the guide. Quotes are short and attributed; go read the originals.

## Contributing

Found a slop tell that isn't on the list? Open an issue or PR. Bonus points for a source.

## License

[MIT](LICENSE). Mop freely.

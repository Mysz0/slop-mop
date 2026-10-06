# The slop-mop design guide

How to make app and product UI look clean, native and deliberate instead of generic and
AI-generated. Written for iOS (SwiftUI), Android (Jetpack Compose / Material 3) and web apps.

Rules first, short reasons after. Citations like [3] point to the numbered sources at the end;
every source listed was actually read. Lines without a citation are working conventions that
held up in practice.

This is about **product UI**: screens people use every day. Marketing pages and games can be
louder on purpose; section 13 says where that changes the advice.

---

## 0. How to use this guide

- **The project's own rules win.** If the codebase has a design system, a style guide or an
  owner with stated taste, follow it. This guide fills the gaps and explains the reasons.
- Section 13 lists the places where well-known sources disagree with each other. Read it before
  "correcting" something toward Apple or Material.
- The deletions that most often fix a slop-looking screen:
  - tinted icon discs → bare glyphs
  - "Thanks!" toasts and tick flashes → the state change is the confirmation
  - a bordered card around every row → a hairline list
  - centred illustration empty states → nothing, or one plain line
  - vertical rules between stats → leading-aligned numbers
  - icons on section headings → none
  - gradient avatars → a neutral disc with an initial

---

## 1. Core principles

1. **Less, but better.** Rams: good design "concentrates on the essential aspects, and the
   products are not burdened with non-essentials" [26]. Every element must earn its place; the
   default answer for a decorative element is delete.
2. **Unobtrusive.** Rams: design should be "both neutral and restrained, to leave room for the
   user's self-expression" [26]. The user's content is the content; the UI is the frame.
3. **One primary action per screen.** HIG: keep prominent buttons to "one or two per view";
   more increases cognitive load [7]. Toolbars: "Only specify one primary action" [5]. One is
   a better default than two.
4. **Hierarchy through size, weight and grey value, not boxes.** Refactoring UI: use colour and
   font weight, not only size, to create hierarchy; use fewer borders and separate with
   spacing or background instead [23]. NN/g lists colour/contrast, scale and grouping as the
   hierarchy tools and says to limit big elements to two [27].
5. **Content over chrome.** HIG: differentiate controls from content; Liquid Glass belongs to
   the control layer only, never the content layer [1][12]. Linear's redesign was mostly
   *removing* noise from sidebar, tabs and headers [38].
6. **Familiar beats clever.** Jakob's Law: "users prefer your site to work the same way as all
   the other sites they already know" [32]. Nielsen: "It's the true test of design skills to do
   great work within the constraints of user expectations" [33]. Copy the first-party apps on
   each platform before inventing anything.
7. **Honest.** Rams: design "does not make a product more innovative, powerful or valuable than
   it really is" [26]. No hype copy, no fake urgency, no inflated celebrations.
8. **Thorough.** "Nothing must be arbitrary or left to chance" [26]. Alignment work is felt
   even when it isn't seen [38].

---

## 2. Process (do this every time)

1. **Render before.** Screenshot the current screen in light, dark and largest text, on every
   platform you ship. Never redesign from code alone.
2. **Write the 3-line brief.**
   - Job: what the person comes here to do (one sentence, a verb).
   - Primary action: the single thing that gets the accent treatment, or "none".
   - Reference app: a first-party app that already solves this layout (see table below).
3. **Inventory every element** and mark it keep / merge / demote / delete. **Default is delete.**
   An element survives only if the brief needs it. Hick's Law: decision time grows with the
   number and complexity of choices [32].
4. **Restructure before restyling.** Fix grouping, order and alignment first. Colour changes
   on a cluttered layout are lipstick.
5. **One screen at a time.** Finish, render, check, then move on. No sweeping global restyles.
6. **Re-render** light, dark, largest Dynamic Type / 200% font scale [2][14]. HIG: test "the
   largest and the smallest layouts" first [1].
7. **Score it** with the checklist in section 12.

Reference apps by screen type:

| Screen type | iOS reference | Android reference |
|---|---|---|
| Feed / nearby list | Apple Maps search results, Settings lists | Google Maps "Explore nearby" list |
| Map with a sheet | Apple Maps (sheet over map) | Google Maps (bottom sheet over map) |
| Profile + stats | Fitness summary, Health | Google Fit, Settings > About |
| Store / purchases | App Store in-app purchases, Settings > Apple ID | Play Store purchase sheet |
| Saved collections | Apple Maps Guides, Fitness workouts list | Google Maps saved lists |
| Leaderboard | Game Center leaderboard | Play Games leaderboard |
| Editors / forms | Contacts edit, Reminders detail | Contacts edit |
| Settings | Settings | Settings |

---

## 3. Layout and spacing

**Spacing scale**
- Use one scale everywhere: **4, 8, 12, 16, 24, 32, 48**. Android's own grid is 8dp for
  layout, 4dp for icons, type and small internal gaps: "stick to measurements of 4 and 8" [13].
- Start with too much space, then remove until it feels right; cramped is the default failure
  mode [24]. Dense is fine where it is a deliberate choice (stats tables), never by accident [24].
- **Avoid ambiguous spacing** [24]: space *between* groups must be clearly larger than space
  *inside* a group (e.g. 8 inside, 24 between). Proximity is what groups things [32][27].

**Grouping**
- Group by proximity first, then a hairline separator, and only as a last resort a container
  (background or card). HIG lists negative space, container shapes and separators as the
  grouping tools [1]. Common region (a box) is the heaviest grouping signal [32]; spend it
  rarely.
- One level of container at most. A card inside a card inside a section is always wrong.

**Alignment**
- **Leading-align everything by default.** People read top-to-bottom, leading-to-trailing, so
  important items go top and leading [1]. Centring is for single short elements in an
  otherwise empty region (a sheet's lone button, a map pin label), never for lists, stats or
  empty states. Butterick: "Use centered text sparingly" [25].
- Align text to a single leading edge per column. Glyphs in list rows sit in a fixed-width
  leading column so text edges line up row to row [38].
- Indentation means subordination; don't indent for decoration [1].
- Numbers in stat rows: leading-aligned label above or beside the value, no vertical rules.
  In tables of numbers, use tabular figures and trailing-align the numeric column.

**Margins and safe areas**
- iOS: use system layout margins, safe areas and readable-width guides; don't hard-code device
  sizes. Lay out by size class, not by device or orientation [1].
- Android: 16dp side margins on phones (Compose/M3 convention), content on the 8dp grid [13];
  draw edge-to-edge and pad for system bars with insets.
- Web: max content width ~680px for reading, ~1120px for app layouts; 16px gutters on phones.
- Touch targets: iOS 44×44pt minimum [7]; Android 48dp (the M3 drag handle itself is 48dp
  for this reason [19]). A small glyph can have a larger invisible hit area.

**Adaptivity**
- At large text sizes, stack horizontally-arranged pieces vertically; let rows grow; keep
  functionality identical across sizes [1][2].
- Progressive disclosure over showing everything: detail views, menus, disclosure [1].

---

## 4. Typography

**Fonts**
- **In native apps, use system fonts**: SF Pro on iOS, Roboto / system default on Android, the
  system stack on web apps (`-apple-system, system-ui, ...`). HIG: minimise typefaces; mixing
  obscures hierarchy and looks inconsistent [2]. (Some anti-slop writing calls system fonts a
  tell [41]; that advice targets marketing pages, see section 13.)
- Never embed SF; use the platform APIs [2].

**Scale**
- Use the platform text styles, not raw sizes. iOS: Large Title, Title 1–3, Headline, Body,
  Callout, Subheadline, Footnote, Caption [2]. Android M3: display / headline / title / body /
  label, e.g. body large 16sp, title large 22sp, label small 11sp [15][17].
- iOS default body 17pt, minimum 11pt [2].
- **Three sizes per screen is usually enough** (NN/g: no more than 3 sizes [27]); two weights
  (regular and semibold/bold) [23]. A screen with five sizes and three weights is a slop tell.
- Avoid light/thin weights in UI; prefer Regular, Medium, Semibold, Bold [2]. Refactoring UI:
  nothing under 400 for UI work [23].
- Hierarchy order of tools: grey value first (primary / secondary / tertiary label colour),
  then weight, then size [23][2].

**Dynamic Type / font scaling**
- iOS: every text uses a text style so Dynamic Type works; support at least 200% enlargement
  [10]. Icons that carry meaning must scale with text (SF Symbols do) [2].
- Keep truncation minimal at large sizes; let labels wrap (`lineLimit(nil)`) and switch rows to
  a stacked layout at accessibility sizes [2].
- Android: text sizes always in sp; line height in sp; never sp for padding (non-linear scaling
  means 4sp + 20sp ≠ 24sp); test at 200% [14].
- Prioritise: not every word must grow equally; content grows, tab labels needn't [2].

**Text details**
- Line length 45–90 characters for any paragraph; line spacing 120–145% [25]. On a phone this
  is automatic; on web cap the measure.
- Bold or italic "as little as possible, and not together" [25].
- ALL CAPS only for under one line, with 5–12% extra tracking [25]. Prefer sentence case
  headings instead.
- No underlines except web links [25].
- Numbers: tabular (monospaced) figures for anything that updates or stacks (scores,
  distances, ranks, timers, prices in a column) — `.monospacedDigit()` in SwiftUI,
  `fontFeatureSettings = "tnum"` in Compose, `font-variant-numeric: tabular-nums` on web.
  Format with locale-aware formatters.
- Use tight leading only for one or two lines; three or more lines need normal leading [2].

---

## 5. Colour

**Neutrals first**
- Build every screen in greyscale using semantic system colours: label / secondaryLabel /
  tertiaryLabel, systemBackground / secondarySystemBackground, separator on iOS [3];
  onSurface / onSurfaceVariant, surface, outlineVariant on Android [15][16].
- Don't redefine semantic colours (no separator colour as text, no secondaryLabel as a
  background) [3]. Don't hard-code system colour values [3].
- Linear's lesson: reduce colour variables to base, accent, contrast and keep chrome neutral;
  limit how much accent leaks into neutrals [38].
- Neutral scales should be numbered (e.g. 0–1000), not named "light/dark"; a gap of ~500 steps
  gives AA 4.5:1 in a well-built scale [39].

**Accent budget**
- Pick **one** accent colour and give it a short list of jobs, for example: the one primary
  action, controls in the on state, and completed items. Write the list down and keep
  everything else (links, icons, headings, decoration) neutral unless the list says otherwise.
- HIG agrees in spirit: apply colour sparingly; emphasise primary actions by colouring the
  background, not the text; "Refrain from adding color to the background of multiple
  controls" [3].
- **One meaning per colour.** HIG: "Avoid using the same color to mean different things" [3].
  If the accent means "primary / on / done", it can't also decorate a stat.
- Never use the accent as a tinted background wash behind content.
- Destructive actions: system red, text only, inside a confirmation; never a red filled button
  in a list. Refactoring UI: destructive needn't be big and red if it isn't the primary
  action on the page [23].
- Categories that need colour for data (tiers, map layers, chart series): make glyph shape and
  text the primary signal; colour, if any, is secondary and muted [3][27].

**Dark mode and contrast**
- Every colour needs light, dark and increased-contrast variants [3]. Use system dynamic
  colours to get these for free [3][10].
- Contrast: 4.5:1 minimum for text (WCAG AA), checked in both light and dark [10].
- Never rely on colour alone for state; add a glyph, weight or text [3][27].
- No grey text on coloured backgrounds; on a tinted surface use a tint of the surface hue [23].
- Dark mode is not inverted light mode: elevated surfaces get lighter greys, not shadows.
- Android: M3 tints surfaces and elevation with the primary colour by default [15][16]. If you
  want a neutral look, build the scheme so surfaces/containers are true neutrals.

---

## 6. Components

### Lists vs cards
- **Default to lists.** NN/g: a vertical list "is more scannable than cards", cards take more
  space, deemphasise ranking and hurt comparison; use a list for homogeneous items [28].
  HIG: prefer lists for text-heavy, scannable content [8].
- Cards only for heterogeneous items browsed rather than searched [28]. Feeds of places,
  products, collections and rankings are almost always homogeneous: use hairline lists.
- iOS: `List` with `.insetGrouped` or `.plain`; system separators; disclosure chevron when a row
  navigates [8]. Android: `ListItem` with `HorizontalDivider` (outlineVariant) or no dividers
  with 8/16dp rhythm.
- Row anatomy: leading glyph or thumbnail (optional) → title → one secondary line →
  trailing value or chevron. One secondary line, not three metadata chips.
- Keep row text succinct; move detail to the detail view [8].
- Selection feedback: navigation rows highlight briefly; option rows show a checkmark [8].

### Sheets
- Pick one sheet header pattern and use it on every sheet. A good default: title leading, one
  small grey close glyph trailing; editors use Cancel and Save. (HIG's own layout differs, see
  section 13; consistency matters more than which one you pick.)
- Use the standard close symbol (xmark), not a text label "Close" [5].
- One sheet at a time; close the first before opening another [4].
- Support swipe-down to dismiss; if there are unsaved edits, confirm on swipe [4].
- iOS detents: medium + large for map sheets, large only for editors and compose-like
  flows [4].
- Android: `ModalBottomSheet`; scrim for modal; tapping outside dismisses [19].
- Sheets are for scoped tasks; long multi-step flows get a full-screen modal [4].

### Navigation bars / top app bars
- Title states location. Under ~15 characters; never the app name as a title [5].
- iOS: large title on root tabs, collapsing to inline on scroll [5]; inline title on pushed
  screens. Standard back button; never a custom "Back" text label [5].
- At most one trailing action group; aim for three groups max even on iPad [5]. Prefer
  symbols for common actions, text for ones symbols don't express ("Edit") [5].
- Android: small top app bar, title start-aligned (the default) [22]; lift on scroll with a
  tonal/neutral change, not a coloured bar.

### Tab bars / navigation bars
- Tabs are for navigation, never actions [6]. Keep the bar visible in all sections [6].
- Fewer tabs is easier; five or fewer [6]; the M3 navigation bar holds three to five [20].
- Don't disable or hide tabs when empty; show why it's empty inside [6].
- Labels under icons: HIG says include them, single words [6]; NN/g: icons need visible text
  labels [31]. Keep labels unless you have a strong reason.
- Selected state: HIG and M3 tint it with accent / secondaryContainer [6][20]. A quieter option
  that works well: selected = primary label colour + filled symbol, unselected = secondary grey.
- Badges on tabs only for genuinely critical, actionable info [6]. No "New" badges.

### Buttons
- One prominent (filled accent) button per screen [7][5]. Everything else: plain/borderless
  text or a grey bordered style. Refactoring UI hierarchy: primary solid, secondary outline or
  low contrast, tertiary link-style [23].
- Use style, not size, to mark the preferred choice; buttons in a set share a height [7].
- Primary actions on phones may span the width [7]; place them at the bottom or in the
  trailing toolbar slot, not floating mid-content.
- Labels start with a verb, a few words: "Start", "Buy", "Save" [7][11]. "Send" beats
  "Let's do it!" [11]. No arrow glyphs welded to labels [41].
- Never make a destructive action the primary/default button [7].
- Async actions: show a spinner inside the button, optionally with "Buying…" [7], then just
  change state. No success screen.
- Don't make custom white-fill/black-text buttons on iOS; that style means "toggled on" [7].

### Empty, loading and error states
- Empty: one plain sentence stating the status, leading-aligned, secondary grey; optionally
  one text button that fixes it. "No saved places yet." NN/g's core requirements — say the
  status, give a path [29] — fit in one line; illustrations and centred hero copy don't.
- Never show "No results" while still loading; that's actively harmful [29].
- Loading: system spinner or skeleton placeholders shaped like the real rows; no shimmer
  storms, no branded loaders.
- Errors: inline where the problem is, in plain words: "Couldn't load results. Retry."
  Not a toast [30]. Avoid "we" ("Unable to load content" beats "We're having trouble…") [11].

### Icons
- Bare glyphs (SF Symbols / Material Symbols) in secondary grey. No tinted discs, no
  icon-in-rounded-square [41].
- Pair icons with text in navigation and anywhere meaning isn't universal; only search, home
  and a few others are universal [31].
- 5-second rule: if you can't think of an icon in five seconds, use a word [31].
- Don't scale small-optimised glyphs up to decorative sizes [23]; use the symbol weight/scale
  that matches adjacent text [2].
- No icons on section headings. No icon-per-row when every row would have the same icon.
- Filled symbols in tab bars on iOS [6].

### Badges, chips, pills
- Default delete. A badge is justified only for critical actionable info [6]. Status that
  matters goes in the row's secondary text ("Completed · 3.2 km").
- Never stack multiple pills on a row ("NEW", "HOT", "2×").

### Feedback, toasts, snackbars
- **The state change is the confirmation.** Toggle flips, row moves to completed, number
  updates, button becomes "Owned". No "Thanks!" toast, no tick flash.
- Toasts are a bad way to show errors [30]; errors go inline.
- The one strong case for a snackbar: Undo after a destructive action (Material's intended
  use: undo/retry, one at a time, never critical [21]).
- Haptics may supplement a meaningful state change (HIG suggests haptics as an alternative to
  motion [9]); one light haptic, not a pattern.

### Stats
- Leading-aligned value in a large numeric style with a small secondary label; no vertical
  rules, no boxes around each stat, no icon above each number.
- Two to four stats max in a row; more goes into a list.
- Use tabular figures; units in secondary grey and smaller.

### Maps
- The map is the content; controls float in the system control layer (Liquid Glass on iOS [12],
  standard FAB/small buttons on Android). Few controls: location, layers, maybe filter.
- Pins: one neutral style; selected = larger/darker, or the accent if "selected" is on your
  accent's job list.
- A bottom sheet with a medium detent shows the nearby list; full detent shows the full list [4].

---

## 7. Platform-native

### iOS (SwiftUI)
- Use stock components: `NavigationStack`, `List`, `Form`, `TabView`, `.sheet` with
  `presentationDetents`, `toolbar` placements, `Button` styles `.borderedProminent` for the one
  primary, `.bordered`/`.plain` elsewhere.
- Large titles on top-level screens [5]. Swipe actions on list rows instead of edit buttons.
- Liquid Glass: system bars and tab bars get it automatically; don't put glass in the
  content layer; use sparingly on custom controls [12]. Colour on glass only for the primary
  action, and on the background, not the symbol [3].
- Context menus (long-press) for secondary row actions rather than a visible "…" per row.
- SF Symbols at the text style's scale; hierarchical rendering in grey, not multicolour.

### Android (Compose, Material 3)
- `Scaffold`, `TopAppBar` (small, start-aligned), `NavigationBar`, `ModalBottomSheet`,
  `ListItem`, `Button` (filled = primary only), `TextButton`/`OutlinedButton` otherwise.
- Colour scheme from tokens, with neutral surfaces; decide deliberately whether to allow
  dynamic colour (it injects wallpaper hues into accents [15][16]). If the brand depends on a
  fixed accent, turn it off.
- Text in sp, layout in dp, 8dp grid [13][14]. Support predictive back and edge-to-edge.
- Material motion tokens and springs for system-like transitions [18].

### What not to cross-port
- No iOS-style chevrons-and-inset-grouped look on Android settings; use M3 `ListItem`s.
- No Android FABs on iOS. No iOS segmented "pill" sheets on Android.
- No iOS swipe-from-edge assumptions on Android; Android has system back.
- Don't centre Android top app bar titles because iOS centres inline titles.
- Shared structure, platform-native skin: same information architecture, same copy, same
  accent rules; each platform's own controls and metrics.

### Web apps
- Must feel like a product, not a landing page: no hero + three feature cards + CTA
  template, no gradient headline text, no `rounded-2xl shadow-lg` on everything [40][41].
- System font stack, the same neutral tokens and accent budget as the native apps,
  `prefers-color-scheme` and `prefers-reduced-motion` respected [36].

---

## 8. Motion

- **Purpose test first.** HIG: "Don't add motion for the sake of adding motion" [9]. Emil
  Kowalski: ask "what's the purpose of this animation?" [35]. Valid purposes: show where
  something came from or went (spatial continuity), confirm a state change, follow a finger.
- **Frequency kills.** Things used many times a day should not animate (or barely): tab
  switches, row taps, toggles beyond the system's own [9][34][35]. Never animate
  keyboard-initiated actions [35][36].
- **Fast.** UI animations under 300ms [35][36]; small components 50–200ms, larger
  traversals 250–400ms [18]. Doherty threshold: keep interactions under 400ms [32].
- **Easing:** ease-out for things entering/exiting [36][37]; standard curves for on-screen
  movement [18]. Prefer system springs (SwiftUI `.smooth`/`.snappy`, M3 spring tokens:
  fast spatial for small components, default spatial for partial-screen, slow for
  full-screen [18]). No bouncy overshoot on UI chrome.
- **Interruptible.** Never block input while animating; let people cancel [9][36]. Destructive
  gestures complete on release, not mid-gesture [34].
- **Direct manipulation on gestures.** Dragging a sheet, panning a map, swiping a row: content
  tracks the finger exactly and keeps momentum on release [34]. Beyond that, be very careful
  with reactive wobble, scale pulses, tilt, parallax or shake on touch; in product UI they
  mostly read as noise.
- **No blinking, no looping attention-seekers** [10].
- Animate only transform and opacity on web [36]; never scale from 0 (start ≥0.9 if scaling
  at all) [37]; scale from the trigger's origin, not centre [37].
- Transition choice (Android): container transform for list→detail, shared axis for steps,
  fade-through between unrelated tabs, plain fade for dialogs/menus (150ms in, 75ms out) [18].
  iOS: default push and sheet presentation; zoom transition only where a thumbnail opens.
- **Reduce Motion:** replace movement with a short cross-fade or nothing; reduce zooming,
  scaling, peripheral and repeating motion [10][36]. Never use motion as the only signal [9].
- Never uniformly "fade-in-up" every element on appear; that's a slop tell [41].

### Name the technique before writing code

Asking a model to "add animations" gets the average of every animation it has seen [44].
Pick one technique per moment first:

- **Layout transitions** — the cheapest big win, natively supported on every platform.
- **Springs** — UI response.
- **Gestures** — follow the finger. Gesture and spring are one idea: the object sits under the
  finger 1:1, and the spring takes over only at release, carrying the throw's velocity.
- **Keyframes** — choreographed A to B.
- **Physics** — world-like objects.
- **Path** — draw or move along a line.
- **Shaders, particles, sprites** — celebration and spectacle; rare in product UI.
- **Rive / Lottie** — drawn characters and illustrations.

Then:
- **Every animation has a rest state it returns to** (think state machine).
- **Placed vs appeared.** Things the user placed may land with a tiny settle. Things that
  merely appear don't.
- **Watch it on a device, especially the last frame.** AI-written motion usually breaks at the
  end.

### Rive, Lottie and other animation files

- **Default: none.** Native springs and system transitions cover almost all UI motion. Rive
  belongs only where a drawn, stateful illustration *is* the content, never on chrome
  (buttons, rows, tabs, sheets).
- **Cost** [42]: RiveRuntime adds about 1.7 MB download and 4.7 MB install on iOS, and about
  2.4 MB download and 7 MB install per ABI on Android. It also adds a second animation
  system to keep in sync across platforms, plus a designer-only file format.
- **What it does well** [43]: one `.riv` file drives iOS (SwiftUI via
  `RiveUIViewRepresentable`, iOS 14+) and Android. State machines take inputs from code,
  so an illustration can react to app state. Editor semantics can be exposed to VoiceOver.
  Reduce Motion is not automatic: check it yourself and show a static frame.
- **Good uses:** a genuine milestone moment, a mascot or illustrated empty state someone
  actually asked for, onboarding art.
- **Not for:** loading spinners (use skeletons), success ticks (the state change is the
  confirmation), press feedback, or looping idle decoration.
- The same rules apply to Rive as to any motion: purpose test, play once, respect Reduce
  Motion, never block input.

---

## 9. Copy

- **Short, concrete, plain.** "Check each word to be sure it needs to be there" [11].
- Buttons: verb + object [7][11]. "Start route", "Buy pack", "Save". Not "Let's go!".
- No filler or marketing tone in the product: no "Discover amazing things near you!",
  "Elevate your workflow", "Seamless", "Powerful" [40][41]. Rams: honest, no overselling [26].
- No "we"; no possessives unless needed ("Favourites", not "Your Favourites") [11].
- One case style per element type and use it everywhere [11]. Sentence case for titles,
  headings, buttons and labels is the easiest to keep consistent.
- Exclamation marks: essentially none [25]. No emoji in UI copy or as bullets [41].
- Empty states state a fact ("No saved places yet."), not a pep talk.
- Errors: what happened + what to do, without blame or apology theatre.
- Numbers over adjectives: "1.2 km away · Open until 22:00", not "Close by and open late".
- Section headers are nouns ("Nearby", "Saved", "Settings"), never full sentences.
- Stores and paywalls: state price, effect and duration plainly ("2× storage for 1 month ·
  $0.99"). No countdown urgency, no "Best value!" ribbons.

---

## 10. Slop tells — delete on sight

Structure
- [ ] Bordered or shadowed card around every list row → hairline list [28][23]
- [ ] Card inside a card; section inside a rounded container inside a screen-wide card
- [ ] Everything centred: headings, stats, empty states, paragraphs [1][25]
- [ ] Hero block at the top of an in-app screen (big greeting, illustration, tagline)
- [ ] Three equal feature cards in a row [40][41]
- [ ] Uniform padding everywhere, no difference between inside- and between-group spacing [41][24]
- [ ] Vertical rules between stats → leading-aligned numbers
- [ ] Decorative dividers, double borders, accent stripes on cards

Colour and surface
- [ ] Gradients on cards, buttons, avatars, headers, text [40][41]
- [ ] More than one accent colour; accent on non-primary things
- [ ] Tinted icon discs / icon-in-rounded-square [41]
- [ ] Coloured section headings or coloured stat numbers
- [ ] Soft large shadows on flat content [40]; glass in the content layer [12]
- [ ] Gradient or generated avatars → neutral disc with initial [41]
- [ ] Accent colour as a tinted background wash

Icons and badges
- [ ] Icon on every section heading
- [ ] Same icon repeated on every row
- [ ] Generic sparkle / zap / arrow-right glyphs [41]
- [ ] "NEW", "HOT", "PRO" pills; counts nobody needs
- [ ] Emoji as bullets or decoration [41]

Feedback and motion
- [ ] Success toasts, "Thanks!", tick/confetti flashes after ordinary actions
- [ ] Error shown as a toast [30]
- [ ] Fade-in-up on every element on appear [41]
- [ ] Press-scale, wobble, shake, pulse, glow, shimmer on touch
- [ ] Blinking dots or looping attention animations [10]

Copy
- [ ] Marketing adjectives (amazing, seamless, powerful, elevate) [40][41]
- [ ] Explanatory subtitle under every heading that repeats the heading
- [ ] Exclamation marks, "Let's…", "Oops!", "We…" [11][25]
- [ ] Title Case Everywhere mixed with sentence case [11]

Typography
- [ ] More than three sizes or two weights on a screen [27][23]
- [ ] Light/thin weights for UI text [2]
- [ ] All-caps labels longer than a few words; tracking-free caps [25]
- [ ] Proportional digits in changing numbers

---

## 11. Worked defaults by screen type

- **Feed / nearby list:** large title ("Nearby"); plain list; row = category glyph (grey, fixed
  column) → item name (body, primary) → "Café · 240 m · Open" (subheadline, secondary) →
  trailing chevron. No cards, no thumbnails unless a real photo exists, no rating stars
  unless the ratings are real.
- **Map with a sheet:** full-bleed map; a small set of floating system controls; sheet with a
  medium detent listing results; a selected item shows in the same sheet, not a new one [4].
- **Profile:** neutral avatar disc + name; one row of 2–4 leading-aligned stats; then a plain
  grouped list (History, Saved, Settings). No progress ring with gradients; a thin neutral
  progress bar if progress matters at all.
- **Store / paywall:** a list of items; each row: name, effect + duration, trailing price as a
  bordered grey button; an owned/active item shows "Active · 42 min left" in secondary text.
  The one prominent (accent) button lives inside the purchase confirmation.
- **Saved collections:** list grouped "In progress" / "Completed"; completed rows get a check
  glyph. Detail: content + items list + one prominent action.
- **Leaderboard:** a plain ranked list, rank number in tabular figures in a fixed leading
  column, the current user's row highlighted with a neutral fill. Lists, not cards, because
  ranking matters [28].
- **Editors (profile edit, settings forms):** `Form` / M3 list; Cancel leading, Save
  trailing (Save is the one prominent action); swipe-dismiss confirms if dirty [4].

---

## 12. Review checklist (score a screen)

Score 0/1 each; ship at 18+ of 20, and no fails on items 1–4.

1. [ ] The project's own design rules (if any) all hold.
2. [ ] The 3-line brief exists and the screen matches it.
3. [ ] Exactly zero or one prominent accent action; the accent only does the jobs on its list.
4. [ ] Renders correctly in light, dark and largest text on every platform (no clipping,
       no overlap, rows grow) [1][2][14].
5. [ ] Content is leading-aligned; nothing is centred without a reason.
6. [ ] Spacing uses the 4/8 scale; between-group > within-group spacing [13][24].
7. [ ] No more than one container level; no per-row cards.
8. [ ] ≤3 text sizes, ≤2 weights; all via text styles [27][2].
9. [ ] Hierarchy is readable in greyscale (squint test) [23][27].
10. [ ] Text contrast ≥4.5:1 in light and dark [10].
11. [ ] No state communicated by colour alone [3].
12. [ ] Icons are bare glyphs; labelled where meaning isn't universal [31].
13. [ ] No badges/pills unless critical [6].
14. [ ] No success toasts; errors inline [30].
15. [ ] Motion: purposeful, <300ms, interruptible, Reduce Motion respected [9][35][10].
16. [ ] Copy: verbs on buttons, one case style, no filler, no "we", no exclamation marks [11].
17. [ ] Empty/loading/error states each designed; none says "empty" while loading [29].
18. [ ] Touch targets ≥44pt / 48dp [7][19].
19. [ ] Uses stock platform components; nothing looks like a web page in a frame.
20. [ ] Elements deleted this pass outnumber elements added.

---

## 13. Where the sources disagree (judgment calls)

Pick a side per project, write it down, and be consistent. The "default" here is the
restrained choice; the project's own rules override it.

1. **Sheet buttons.** HIG puts Cancel/Close on the *leading* edge and Done on the trailing
   edge in iOS sheets [4]. Many polished apps instead use title leading + one close glyph
   trailing. → Either works; mixing them doesn't. Keep swipe-to-dismiss and the dirty-state
   confirmation either way [4].
2. **Grabbers / drag handles.** HIG says include a grabber in resizable sheets (it also helps
   VoiceOver) [4]; Material's drag handle is a 48dp accessibility affordance for TalkBack [19].
   → Keep one on resizable sheets. If you drop it, keep accessibility actions for resize.
3. **Accent tint everywhere.** HIG uses the app accent for interactive tint, and M3 uses
   primary for "prominent buttons, active states, and the tint of elevated surfaces" [3][15].
   → Default to the short accent job list in section 5; links, icons and surfaces neutral.
4. **Tab bar selected colour.** HIG tints selected tabs; M3 uses a secondaryContainer active
   indicator pill [6][20]. → Platform default is fine; the quieter option is primary label
   colour + filled glyph, with a neutral indicator on Android.
5. **Tab labels.** HIG: include labels [6]; NN/g: icons need labels [31]; M3 auto mode hides
   unselected labels at 4+ items [20]. → Keep labels.
6. **Success feedback / snackbars.** Material calls snackbars "the preferred mechanism for
   displaying feedback messages" [21]; NN/g treats passive notifications as legitimate [30].
   → The state change is the confirmation. Undo-after-delete is the snackbar worth keeping.
7. **Material tonal surfaces and dynamic colour.** M3 represents elevation with primary-tinted
   tonal overlays, and dynamic colour derives accents from wallpaper [15][16]. → Neutral
   surface containers; dynamic colour off if the brand needs a fixed accent.
8. **Refactoring UI decoration tips.** It suggests enlarging icons by putting them in
   coloured shapes and adding accent borders for colour [23]. → In product UI these are now
   the most recognisable slop tells. Bare glyphs; no accent borders.
9. **Press feedback motion.** Emil Kowalski recommends a 0.97 scale on button press [37]; Rauno
   praises momentum and physicality [34]. → Subtle press feedback can be lovely; in dense
   product UI, system highlight states are usually enough. Physics for true direct
   manipulation (sheet drag, map pan).
10. **Empty states.** NN/g recommends learning cues and direct pathways, often with buttons and
    "Learn more" links [29]. → For a repeat-use app, one plain line and at most one text
    action. Onboarding-heavy products may need more.
11. **Anti-slop advice that pushes the other way.** Some anti-AI-design writing calls
    Inter/Roboto/system fonts, neutral zinc/slate palettes and absent motion "tells", and
    urges display faces, asymmetry and more motion [41]. That advice targets **marketing web
    pages**, where it is often right: a landing page should have a point of view, custom type
    and playful, smooth interaction. For native product UI: system fonts, neutral palette,
    minimal motion.
12. **Badges.** HIG allows red tab badges for critical info [6]; M3 supports "New" text badges
    [20]. → Critical-only, and never "New".
13. **Centre-aligned app bars.** M3 offers centred titles as an option [22]. → Start-aligned
    on Android, per platform default.
14. **Buttons with a background shape.** HIG prefers buttons with a visible fill [7]. → One
    accent-filled button; secondary actions use the grey bordered or plain style, never a
    second coloured fill.

---

## 14. Sources

Apple Human Interface Guidelines
1. HIG — Layout. https://developer.apple.com/design/human-interface-guidelines/layout
2. HIG — Typography. https://developer.apple.com/design/human-interface-guidelines/typography
3. HIG — Color. https://developer.apple.com/design/human-interface-guidelines/color
4. HIG — Sheets. https://developer.apple.com/design/human-interface-guidelines/sheets
5. HIG — Toolbars. https://developer.apple.com/design/human-interface-guidelines/toolbars
6. HIG — Tab bars. https://developer.apple.com/design/human-interface-guidelines/tab-bars
7. HIG — Buttons. https://developer.apple.com/design/human-interface-guidelines/buttons
8. HIG — Lists and tables. https://developer.apple.com/design/human-interface-guidelines/lists-and-tables
9. HIG — Motion. https://developer.apple.com/design/human-interface-guidelines/motion
10. HIG — Accessibility. https://developer.apple.com/design/human-interface-guidelines/accessibility
11. HIG — Writing. https://developer.apple.com/design/human-interface-guidelines/writing
12. HIG — Materials. https://developer.apple.com/design/human-interface-guidelines/materials

Android / Material 3
13. Android Developers — Grids and units. https://developer.android.com/design/ui/mobile/guides/layout-and-content/grids-and-units
14. Android Developers — Android 14 features (non-linear font scaling to 200%). https://developer.android.com/about/versions/14/features
15. Android Developers — Material Design 3 in Compose. https://developer.android.com/develop/ui/compose/designsystems/material3
16. Material Components Android — Color theming. https://github.com/material-components/material-components-android/blob/master/docs/theming/Color.md
17. Material Components Android — Typography. https://github.com/material-components/material-components-android/blob/master/docs/theming/Typography.md
18. Material Components Android — Motion. https://github.com/material-components/material-components-android/blob/master/docs/theming/Motion.md
19. Material Components Android — Bottom sheets. https://github.com/material-components/material-components-android/blob/master/docs/components/BottomSheet.md
20. Material Components Android — Navigation bar. https://github.com/material-components/material-components-android/blob/master/docs/components/BottomNavigation.md
21. Material Components Android — Snackbar. https://github.com/material-components/material-components-android/blob/master/docs/components/Snackbar.md
22. Material Components Android — Top app bars. https://github.com/material-components/material-components-android/blob/master/docs/components/TopAppBar.md

Essays and research
23. Wathan & Schoger, "7 Practical Tips for Cheating at Design" (Refactoring UI). https://medium.com/refactoring-ui/7-practical-tips-for-cheating-at-design-40c736799886
24. Refactoring UI, "Start with too much white space" (book excerpt).
25. Butterick, Practical Typography — Summary of key rules. https://practicaltypography.com/summary-of-key-rules.html
26. Vitsœ — Dieter Rams: ten principles for good design. https://www.vitsoe.com/us/about/good-design
27. NN/g — Visual Hierarchy in UX: Definition. https://www.nngroup.com/articles/visual-hierarchy-ux-definition/
28. NN/g — Cards: UI-Component Definition. https://www.nngroup.com/articles/cards-component/
29. NN/g — Designing Empty States in Complex Applications: 3 Guidelines. https://www.nngroup.com/articles/empty-state-interface-design/
30. NN/g — Indicators, Validations, and Notifications. https://www.nngroup.com/articles/indicators-validations-notifications/
31. NN/g — Icon Usability. https://www.nngroup.com/articles/icon-usability/
32. Yablonski, Laws of UX. https://lawsofux.com/
33. Nielsen, "Jakob's Law of the Internet User Experience". https://www.uxtigers.com/post/jakobs-law
34. Freiberg, "Invisible Details of Interaction Design". https://rauno.me/craft/interaction-design
35. Kowalski, "You Don't Need Animations". https://emilkowal.ski/ui/you-dont-need-animations
36. Kowalski, "Great Animations". https://emilkowal.ski/ui/great-animations
37. Kowalski, "7 Practical Animation Tips". https://emilkowal.ski/ui/7-practical-animation-tips
38. Linear, "How we redesigned the Linear UI (part II)". https://linear.app/now/how-we-redesigned-the-linear-ui
39. Ström-Awn, "How to generate color palettes for design systems". https://mattstromawn.com/writing/generating-color-palettes/
40. 925 Studios, "AI Slop Fonts and Gradients: The Tells That Give Away AI Design". https://www.925studios.co/blog/ai-slop-design-tells
41. avoid-ai-design (GitHub README, AI-generated frontend audit). https://github.com/funboy322/avoid-ai-design
42. Rive docs, "Runtime Sizes". https://rive.app/docs/runtimes/runtime-sizes
43. Rive docs, "Apple" runtime. https://rive.app/docs/runtimes/apple/apple
44. "AI App Animations is Hard Until You Learn This" (YouTube). https://www.youtube.com/watch?v=f-Ar8mwm3kQ

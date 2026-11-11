# DESIGN.md - ER-Refill

## Overview

ER-Refill is used by a pharmacist once a day, in the morning, to produce one
paper: the ER drug requisition form. The interface therefore borrows two
things from the Mintlify language - **cinematic atmosphere on the rare empty
states** (gradient hero bands) and **documentation-grade density everywhere
work actually happens** - but re-anchors the palette on the nurse-bear mark:
pastel rose for the working system, warm bear gold for the mascot.

**The signature element is the sheet** - the A4 requisition itself, staged on
a soft desk surface as a live preview: the bear mark at the top-left, hospital
header, the window sentence in full Thai ("วันอาทิตย์ที่ 18 ตุลาคม 2569 เวลา
16:00 น. ถึง วันจันทร์ที่ 19 ตุลาคม 2569 เวลา 08:30 น."), the grouped drug
table with click-to-edit จำนวนเบิก numbers, the reason table, and the
ผู้เบิก/ผู้จ่าย signature blocks. The screen and the printer share one DOM and
one stylesheet - what the pharmacist verifies is exactly what prints; there is
no separate preview mode. The command bar above the desk is the only control
surface: today's date is fixed and read-only, and the operator picks one of
four clearly labelled overnight ranges before fetching.

**Key characteristics:**

- **Atmospheric gradient hero bands, used once**: the login gate gets the
  blush-gradient brand panel with the nurse-bear mascot; that is the only
  marketing-grade surface in the app. Working surfaces are flat and dense.
- **Signature rose pink** ({colors.brand-pink}) reserved for accent CTAs,
  the selected range option, active states, focus rings, and feature
  confirmations - never body text or large surfaces.
- **Black-pill primary buttons** ({colors.primary} + `{rounded.full}`) for the
  dominant action (พิมพ์ใบเบิก); one rose accent CTA per viewport at most.
- **Usage-only rows, plus deliberate additions**: the sheet shows only drugs
  with usage in the window. A configured drug that had no usage can be added
  manually (usage 0) when the pharmacy knows it is needed.
- **Every manual change has a reason**: adding a drug or changing จำนวนเบิก
  opens a reason dialog, and the reasons print as a final
  "ยาที่ขอเพิ่มหรือขอแก้ไขจำนวนเบิก" table (drug name + reason only).
- **Confirmed print gate**: the print button stays disabled until the operator
  confirms the sheet. Confirmation snapshots rows, reasons, window, and header;
  any later edit invalidates the snapshot and disables printing again.
- **Bear gold is illustration, not UI**: the amber mascot gradient
  (`#FDE047 → #F59E0B → #B45309`) belongs to `icon-master.svg`, the login
  illustration, and the printed sheet's mark; it is never used for buttons,
  status, or meaning.
- **Inter for UI prose, IBM Plex Sans Thai for Thai text, IBM Plex Mono for
  codes and quantities** - numbers must align in columns.
- **Command bar + paper stage, not a sidebar**: the fixed date, four range
  options, and the document actions (fetch, add, confirm, print) live in one
  toolbar; the stage below shows the paper. The job is producing a document,
  not looking something up.
- **Tightly-controlled radius scale**: the sheet is paper (no radius); cards
  `{rounded.lg}` (12px), compact UI `{rounded.md}` (8px), all buttons and
  pills `{rounded.full}`.
- **One warm alert tone**: amber ({colors.warn-*}) for "this needs attention"
  (degraded connection, unmapped drug) - deliberately distinct from the error
  red used for required-field validation.

### Adaptation Notes - what changed from the Mintlify reference

| Reference token | ER-Refill token | Why |
|---|---|---|
| Mintlify mint `#00D4A4`-family | `{colors.brand-pink}` `#DB2777` ("กุหลาบตราประทับ" seal rose) | Same role; deepened so white button text holds WCAG AA, while the pastel pinks stay decorative. |
| Inter + Geist Mono only | Inter + **IBM Plex Sans Thai** + IBM Plex Mono | The app is Thai-first; Inter has no Thai glyphs. Plex Thai pairs with Plex Mono and matches the sibling AllerX system. |
| Marketing scale (16px body, 72px hero) | Tool scale (15px body, 28px display) | This is a once-a-day work tool, not a landing page; density wins. The display scale exists **only** on the login brand headline (34px at desktop). |
| Testimonial orange | **Dropped** | No testimonial/marketing surfaces exist. The warm slot is filled by amber, which carries clinical meaning (attention), not emotion. |
| `rgba(0,212,164,.08) 0 8px 24px` featured glow | Kept, retinted | The rose glow marks exactly one thing: the single accent CTA on an empty state. |
| Photography/clouds/rockets | None | No photography or illustration sets. The login brand panel carries one custom requisition illustration (`app/assets/login-illustration.svg`) built around the same nurse-bear mascot from `icon-master.svg`. |

## Colors

### Brand & Accent

- **Seal Rose** ({colors.brand-pink}): `#DB2777` - brand seal and accent CTAs,
  selected range-option border, focus rings, active tab underline, sidebar
  active indicator. Holds WCAG AA with white button text; never for body text.
- **Deep Rose** ({colors.brand-pink-deep}): `#9D174D` - pressed/active variant;
  text on soft rose backgrounds.
- **Soft Rose** ({colors.brand-pink-soft}): `#FCE7F3` - selected range-option
  background, the sheet window sentence tint, playful section tint.
- **Seal Glow** ({colors.brand-glow}): `rgba(219, 39, 119, 0.10)` - the single
  featured shadow, reserved for one brand accent; not used in the current UI.
- **Bear Gold** (`#F59E0B` / `#B45309`): the mascot gradient only - artwork in
  `icon-master.svg` and the login illustration; never a UI token and never
  load-bearing for meaning.

### Semantic

- **Success Green** ({colors.success}): `#059669` on {colors.success-soft}
  `#ECFDF5` - the connected status dot and saved confirmations. This is the
  only green in the system; it carries state, not brand.
- **Alert Red** ({colors.error}): `#D92D20` - required-field labels, validation
  messages, destructive confirmations. Never decorative.
- **Alert Soft** ({colors.error-soft}): `#FEECEB` - error field background tint.
- **Warn Amber** ({colors.warn-text} `#8A6420` on {colors.warn-bg} `#FFF8E1`,
  border {colors.warn-border} `#FFE082`) - system attention states: degraded
  HOSxP connection banner, drug not yet mapped. This
  amber is a *system* warning; it never means "answer found/not found" (there
  is no such verdict in this app).
- **Info Blue** ({colors.tag-blue}): `#3772CF` on {colors.tag-bg}
  `rgba(55, 114, 207, 0.12)` - informational chips (icode, unit, "OPD" tag).

### Surface

- **Canvas** ({colors.canvas}): `#FFFFFF` - the printable sheet's color and
  the stage.
- **Surface** ({colors.surface}): `#FDF7FA` - section backgrounds, search
  pill rest, sheet table header rows, the command-bar stat strip.
- **Surface Soft** ({colors.surface-soft}): `#FEFBFD` - quieter section
  bands, zebra striping off (we keep rows flat; this is for framing).
- **Hairline** ({colors.hairline}): `#F2E2EA` - 1px borders and dividers.
- **Hairline Soft** ({colors.hairline-soft}): `#F8EDF2` - table row
  dividers, secondary breaks.
- **Canvas Dark** ({colors.canvas-dark}): `#17181C` - reserved (no dark
  surfaces in v1; kept named so a future variant doesn't rename tokens).

### Hero Atmospheric

- **Hero Blush From / To** ({colors.hero-from} `#FDF2F8` → {colors.hero-to}
  `#FFF7ED`): the login brand-panel gradient, 165° top-to-bottom, warmed by a
  rose radial glow at the top and a bear-gold glow at the bottom edge.
- **Hero Dark From / To** ({colors.hero-dark-from} `#3B0A22` → {colors.hero-dark-to}
  `#DB2777`): reserved for the About/upgrade band if one ever ships; not used
  in v1 (documented so the token set is complete).

### Text

- **Ink** ({colors.ink}): `#241016` - headlines, primary text, table data.
- **Charcoal** ({colors.charcoal}): `#432C37` - body text.
- **Slate** ({colors.slate}): `#6F5863` - secondary text, metadata.
- **Steel** ({colors.steel}): `#967F89` - tertiary text, table headers,
  sidebar inactive items.
- **Stone** ({colors.stone}): `#B9A5AE` - captions, placeholders.
- **Muted** ({colors.muted}): `#DACBD2` - disabled text.
- **On Dark** ({colors.on-dark}) `#FFFFFF` / **On Dark Muted**
  ({colors.on-dark-muted} `rgba(255,255,255,0.72)`) - text on dark surfaces
  (reserved; see Canvas Dark).
- **On Primary** ({colors.on-primary}): `#FFFFFF` - text on black pills and
  on the rose accent pill.

## Typography

### Font Family

- **Inter** (primary UI): body, headings, buttons, navigation. Fallbacks:
  `-apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif`.
- **IBM Plex Sans Thai** (Thai text): every Thai glyph, paired after Inter so
  Latin characters render in Inter and Thai in Plex Thai within the same
  string. Fallback: `"Leelawadee UI", Tahoma, sans-serif`.
- **IBM Plex Mono** (data): quantities, icodes, times, dates in tables -
  anything that benefits from column alignment (`font-variant-numeric:
  tabular-nums` is applied on top). Fallbacks: `"SF Mono", Menlo, Consolas,
  monospace`.

### Hierarchy

| Token | Size | Weight | Line Height | Letter Spacing | Use |
|---|---|---|---|---|---|
| `{typography.display}` | 28px | 600 | 1.20 | -0.5px | Login brand headline only |
| `{typography.heading-1}` | 22px | 600 | 1.25 | 0 | Screen titles ("ใบเบิกยาประจำห้องฉุกเฉินรายวัน") |
| `{typography.heading-2}` | 18px | 600 | 1.30 | 0 | Category section headers |
| `{typography.heading-3}` | 16px | 600 | 1.35 | 0 | Card/section titles, modal titles |
| `{typography.sheet-window}` | 13px | 400 | 1.40 | 0 | The sheet window sentence (dates in mono) |
| `{typography.body-md}` | 15px | 400 | 1.50 | 0 | Primary body, table data |
| `{typography.body-md-medium}` | 15px | 500 | 1.50 | 0 | Emphasized body (drug name) |
| `{typography.body-sm}` | 14px | 400 | 1.50 | 0 | Secondary body, helper text |
| `{typography.label}` | 13px | 600 | 1.40 | 0 | Field labels |
| `{typography.caption}` | 12px | 400 | 1.40 | 0 | Metadata, table footnotes |
| `{typography.micro-uppercase}` | 11px | 600 | 1.40 | 0.5px | Command-bar chip labels, table column headers |
| `{typography.button}` | 14px | 500 | 1.30 | 0 | Button labels |
| `{typography.code-md}` | 14px | 400 | 1.50 | 0 | icodes, units (mono) |
| `{typography.code-sm}` | 13px | 400 | 1.40 | 0 | Table numbers, timestamps (mono) |

### Principles

- **Thai is first-class**: never render Thai in Inter-only stacks; never
  render Latin codes in Plex Thai (assign fonts by *content role*, not by
  luck of the fallback chain).
- **Weight carries emphasis**, not size: 400 reading, 500 emphasis, 600
  headings/buttons/labels. Only the login brand display goes above 22px.
- **Line height 1.40-1.50 on everything** - the person reading this is under
  time pressure in imperfect hospital lighting.
- **Numbers in tables are mono** with `tabular-nums` so columns of quantities
  align as columns, not as ragged text.

## Layout

### Spacing System

- Base unit 4px, primary increment 8px.
- Tokens: `{spacing.xxs}` (4px) · `{spacing.xs}` (8px) · `{spacing.sm}` (12px)
  · `{spacing.md}` (16px) · `{spacing.lg}` (20px) · `{spacing.xl}` (24px) ·
  `{spacing.xxl}` (32px) · `{spacing.xxxl}` (40px) · `{spacing.section-sm}`
  (48px) · `{spacing.section}` (64px).
- Working surfaces use `{spacing.xxl}` between sections; table rows pack to
  `{spacing.xs}`-`{spacing.sm}` vertical rhythm. `{spacing.section}` exists
  only for the login brand panel.

### Structure - Command Bar + Paper Stage (document-first)

Before the document exists, the full-height `login-gate` replaces the shell
until an `officer` signs in. Connection setup is not part of this surface:
the settings dialog opens from the gate and owns the HOSxP credentials. After
sign-in the shell below is revealed; the top bar shows the signed-in user
and a prominent logout pill on the right, with the connection dot tucked
beside the subtitle and the settings action hidden behind the brand mark.

```
┌──────────────────────────────────────────────────────────────────────┐
│ Top bar (48px: [mark=ตั้งค่า] ER-Refill · subtitle · ● │ user · ออกจากระบบ) │
├──────────────────────────────────────────────────────────────────────┤
│ COMMAND BAR (the only control surface; stacks under 1500px)          │
│  วันที่เบิก 24 ตุลาคม 2569                                            │
│  ช่วงเวลาเบิก  [20:30→08:00] [20:30→08:30] [16:00→08:00] [16:00→08:30] [ดึงข้อมูล] │
│  ช่วงข้อมูล: 23 ต.ค. 2569 20:30 → 24 ต.ค. 2569 08:00                  │
│  [เพิ่มรายการยา] [ยืนยันข้อมูลเบิก] [พิมพ์ใบเบิก]                       │
├──────────────────────────────────────────────────────────────────────┤
│ STAGE (desk surface, scrollable)                                     │
│                                                                      │
│        ┌─────────────────────────────┐  ← THE SHEET (A4 preview)    │
│        │ [bear] โรงพยาบาลสระโบสถ์  [20:30–08:30] │    = the printed output: │
│        │  ใบเบิกยาประจำห้องฉุกเฉินรายวัน │      one DOM, one CSS        │
│        │  กลุ่มงานอุบัติเหตุฉุกเฉิน      │                               │
│        │  ช่วงเวลาเบิก: ...            │                               │
│        │  [table: ลำดับ ชื่อยา+ความแรง │                               │
│        │   จำนวน/Stock จำนวนใช้ จำนวนเบิก]│                             │
│        │  ยาที่ขอเพิ่มหรือขอแก้ไขจำนวนเบิก │                             │
│        │  [ชื่อยา │ เหตุผล]            │                               │
│        │  ลงชื่อ ผู้เบิก │ ลงชื่อ ผู้จ่าย │                               │
│        └─────────────────────────────┘                              │
└──────────────────────────────────────────────────────────────────────┘
```

**Why document-first:** ER-Refill's job is not "look a value up" (the sidebar
pattern of the sibling AllerX tool); it is "verify a document, then print
it". Established document tools teach the same lesson: the print preview
must reuse the same DOM and CSS as the print output, or the preview lies.
So the sheet is rendered once, on screen, exactly as it prints - the
operator corrects จำนวนเบิก on the paper they will sign, explains each change
in a dialog, then confirms the print snapshot before printing. No second
layout exists.

### Grid & Container

- Desktop window 1280×900 default, clamped to the monitor work area, min
  900×620 (a document tool; it does not need to survive phone widths).
- The sheet is a real A4: `min(210mm, 100%)` wide, `297mm` min-height,
  12mm internal padding, `{elevation.3}` on the `{colors.surface}` desk.
  On print, `@page` owns the 12mm margins and the sheet drops its padding.
- The table is the *only* multi-column surface; column widths are frozen:
  `46px` ลำดับ / `1fr` ชื่อยา (+ความแรง) / `108px` จำนวน/Stock /
  `64px` จำนวนใช้ / `76px` จำนวนเบิก.
- จำนวนเบิก starts as whole packs: `ceil(usage / units-per-pack)` from the
  จำนวน/Stock setting, capped at the stock pack count; the operator can
  adjust it on the sheet with a required reason.
- The reason table uses the same table chrome with two columns:
  `34%` ชื่อยา / `66%` เหตุผล.

## Elevation & Depth

Predominantly flat with strategic atmosphere (same table as the reference,
re-scoped):

| Level | Treatment | Use |
|---|---|---|
| 0 (flat) | No shadow; `{colors.hairline}` border | Command bar, top bar, in-modal surfaces |
| 1 (subtle) | `rgba(0,0,0,0.04) 0 1px 2px` | Drug-picker result rows, small popovers |
| 2 (card) | `rgba(0,0,0,0.08) 0 4px 12px` | Toasts, elevated menu surfaces |
| 3 (sheet) | `rgba(0,0,0,0.12) 0 24px 48px -8px` | The A4 sheet on the desk - the paper look - and the settings modal |
| 4 (brand-tinted) | `{colors.brand-glow} 0 8px 24px` | Reserved for one brand accent; not used in the current UI |

The login brand panel needs no shadow - the gradient does the work. The
sheet's shadow is what makes it read as paper on a desk; it disappears in
print.

## Shapes

| Token | Value | Use |
|---|---|---|
| `{rounded.xs}` | 4px | Inline code chips (`icode`), the จำนวนเบิก cell |
| `{rounded.sm}` | 6px | Category nav items, badges, status chips |
| `{rounded.md}` | 8px | Inputs, search pill, picker cards |
| `{rounded.lg}` | 12px | Settings modal, login card |
| `{rounded.xl}` | 16px | Reserved for oversized framed surfaces |
| `{rounded.full}` | 9999px | **All** buttons and pills, badges, status dot halo |

The scale is disciplined: no 10px corners; nothing between `{rounded.md}` and
`{rounded.lg}` for the same component family. Buttons are always pills; the
sheet is paper and takes no radius at all.

## Components

> Per the no-hover policy of the reference: states are documented as default /
> pressed / disabled / active; hover affordances are a flat background change
> only (see the Motion section below).

### Buttons

**`button-primary`** - Black pill primary CTA (the requisition action).
- Background `{colors.primary}` (`#111113`), text `{colors.on-primary}`,
  typography `{typography.button}`, padding `10px 20px`, rounded `{rounded.full}`.
- Pressed: background `{colors.charcoal}`. Disabled: background
  `{colors.hairline}`, text `{colors.muted}`, no pointer events.

**`button-accent`** - Seal-rose pill, used **once per viewport** on an
empty-state or catalog CTA.
- Background `{colors.brand-pink}`, text `{colors.on-primary}`, typography
  `{typography.button}`, padding `10px 20px`, rounded `{rounded.full}`, plus
  an optional `{elevation.4}` glow.

**`button-secondary`** - Outlined pill: transparent background, text
`{colors.ink}`, border `1px solid {colors.hairline}`, padding `10px 20px`.
The top-bar logout uses a stronger `{colors.stone}` border and 600 weight so
it reads as the one clear exit action.

**`button-ghost`** - Quiet rectangular action (settings close): transparent,
text `{colors.ink}`, padding `8px 12px`, rounded `{rounded.md}`.

**`button-icon-circular`** - 32×32px circular utility (close, ↑/↓ in the
catalog list): background `{colors.canvas}`, border
`1px solid {colors.hairline}`, rounded `{rounded.full}`.

**`icon` set** - ER-Refill's own monoline SVGs: 24×24 viewBox, 1.8px stroke,
round caps and joins, `currentColor`. 16px inside buttons, 24px in the
settings rail. No filled or duotone variants; the line stays quiet beside
Thai text and quantities.

### Cards & Containers

**`card-base`** - Background `{colors.canvas}`, rounded `{rounded.lg}`,
padding `{spacing.xl}`, border `1px solid {colors.hairline}`.

**`card-feature`** - Section panel on `{colors.surface}`, rounded
`{rounded.lg}`, padding `{spacing.xxl}` (settings panels use it bare, without
a border).

### Inputs & Forms

**`text-input`** - Background `{colors.canvas}`, text `{colors.ink}`, border
`1px solid {colors.hairline}`, rounded `{rounded.md}`, padding
`{spacing.sm} {spacing.md}`, 40px height. Focus: `2px solid
{colors.brand-pink}` (rose is the activation signal).

**`text-input-error`** - Border `1px solid {colors.error}`; helper line in
`{typography.caption}` + `{colors.error}`.

**`sheet-request-input`** - The editable จำนวนเบิก box *on the paper*: centered
`{typography.code-md}` in a borderless in-cell input, no spinners, focus
ring rose inset (`outline-offset: -2px`). It prints as a plain filled
number.

**`text-input--compact`** - 34px variant for dense command-bar inputs.

**`search-pill`** - Drug-picker / search: background `{colors.surface}`, text
`{colors.steel}`, typography `{typography.body-sm}`, rounded `{rounded.md}`,
36px height, border `1px solid {colors.hairline}`.

### Settings navigation

**`settings-nav__item`** + **`settings-nav__item--active`** - The settings
modal's vertical navigation rail (icon + label per panel, on a
`{colors.surface-soft}` column): inactive text `{colors.steel}` with a stone
icon; active: `{colors.canvas}` background, ink text, and a 3px rose left
border. Below 960px the rail turns into a horizontal scrolling strip
(rose bottom border on the active item). The rail exists because the
catalog panel needs the modal's full width for its category, search, and
drug-table layout.

### Badges & Status

**`badge-icode`** - icode chip: background `{colors.tag-bg}`, text
`{colors.tag-blue}`, typography `{typography.code-sm}`, rounded
`{rounded.xs}`, padding `2px 6px`.

**`badge-required`** - "REQUIRED" / "จำเป็น" label: background
`{colors.error}`, text `{colors.on-dark}`, `{typography.micro-uppercase}`,
rounded `{rounded.sm}`, `2px 6px`.

**`status-dot`** - 8px dot + text: `{colors.success}` connected,
`{colors.error}` disconnected, `{colors.stone}` unconfigured. Applied with a
soft halo (`box-shadow: 0 0 0 3px soft`). The top bar uses the compact variant
(`status-dot--compact`): the dot alone, immediately after the subtitle, with
no visible label. Hovering or focusing it shows a small bordered tooltip with
the full Thai status ("เชื่อมต่อ HOSxP แล้ว" / "การเชื่อมต่อขัดข้อง" /
"ยังไม่ตั้งค่าการเชื่อมต่อ"); the same text is the `aria-label`.

**`run-date` card** - The fixed run date: a roomy 210px-minimum card with an
uppercase "วันที่เบิก" label beside a 14px rose calendar icon, and
"24 ตุลาคม 2569" at 18px/600 in ink. It stretches to the range picker's full
height and centers its two lines, giving the run date equal visual weight to
the range choice. It is a display card, never an input - the date can not
drift from the morning run, and it carries no redundant "locked" caption.

**`range-picker` / `range-option`** - The four fixed overnight ranges as a
segmented radio group (20:30-08:00, 20:30-08:30, 16:00-08:00, 16:00-08:30)
inside a soft-surface track. Each segment shows only the mono time pair; the
selected segment takes the rose border and soft-rose fill. The heading status
explains the mapping once ("เลือก 1 ช่วง · เมื่อวาน → วันนี้", steel) and
switches to "เลือกแล้ว" (green) after a choice. A fixed-width black
"ดึงข้อมูล" pill sits immediately after the track; it is disabled until a
segment is selected and performs both the first fetch and any later refresh
(the separate "ดึงใหม่" action is gone). The preview line spells out the actual
window in Thai/BE, e.g. "ช่วงข้อมูล: ศ. 23 ต.ค. 2569 20:30 → ส. 24 ต.ค. 2569
08:00".

### Signature Components

**`login-gate`** - Full-height split surface before the document exists:
- Left brand panel: the blush hero gradient, the nurse-bear mark, the
  "ใบเบิกยาห้องฉุกเฉิน" headline, and a custom requisition illustration
  (`app/assets/login-illustration.svg`) using the master bear mascot - no
  marketing copy or proof points.
- Right panel: a single `rounded.lg` card focused on the `officer`
  username/password, with a "ตั้งค่า" button in the page toolbar. Until a
  connection is configured, a warn-toned notice inside the card points at the
  settings dialog; the card never embeds the connection form or connection
  summary.
- The connection password is a DB credential and lives only in the settings
  dialog; the login password is never persisted. Both use the standard field
  and button components.

**`app-icon`** - Flat soft-rose (`#FCE7F3`) rounded tile holding the nurse bear:
gold-gradient head and ears, white nurse cap with a rose cross, a cream
muzzle, hugging paws, and a white medical heart with a rose cross.
`icon-master.svg` is the source for every Tauri platform icon; `BrandMark`
mirrors the same bear in the top bar and splash at 28px, and at 15mm in the
top-left corner of every printed sheet page. Print drops the SVG emboss
filter so the 15mm mark stays crisp vector art at the printer's resolution.

**`top-bar`** - The 48px chrome above the command bar:
- Left: the `BrandMark` doubles as the quiet settings trigger (native
  "ตั้งค่า" tooltip, hover surface, keyboard focus ring, `aria-haspopup`);
  the "ER-Refill" title and "ระบบใบเบิกยาห้องฉุกเฉิน" subtitle follow, with
  the compact connection dot at the end of that line.
- Right: exactly the signed-in user pill (avatar initial + display name) and
  a prominent outlined "ออกจากระบบ" pill. There is no visible settings button
  on the right.
- Below 1000px the subtitle hides; the compact status dot stays next to the
  title.

**`command-bar`** - The only control surface; it stacks below 1500px:
- Left: the read-only `run-date` card beside the `range-picker` (heading,
  four `range-option` segments, the inline **ดึงข้อมูล** pill, and the
  actual-window preview).
- Right: the document actions - **เพิ่มรายการยา** (secondary, opens the
  configured-catalog picker) · **ยืนยันข้อมูลเบิก** (rose accent; replaced by a
  success `confirm-chip` with the confirmation time) · **พิมพ์ใบเบิก** (black
  pill, disabled until the sheet is confirmed; any edit disables it again).
- The inline fetch pill has a fixed 146px footprint, so "ดึงข้อมูล" →
  "กำลังดึงข้อมูล..." never nudges the range segments; confirm/chip (176px)
  and print (148px) keep fixed widths too.
- There is no separate refetch action, date input, manual window editor, or
  summary stat strip: the run context is the fixed date, the chosen range,
  and one fetch button.
- Background `{colors.canvas}`, bottom hairline - chrome, not content.

**`sheet`** - The document itself: the A4 paper staged on the desk.
- Header: a left-aligned identity block - the bear `BrandMark` (15mm) with the
  hospital name (`{typography.heading-2}`), document title, and department line
  immediately beside it - plus a large `sheet__window-box` on the right. The
  box is a square-cornered 58mm panel with a 1.5px ink border, a small
  "ช่วงเวลาเบิก" label, and the selected time pair (e.g. `20:30 – 08:30`) in
  18px mono; it prints as a black border.
- Window sentence in full Thai, then the table. Only drugs with usage in the
  window are present; configured drugs with no usage stay hidden unless the
  operator adds them (usage 0).
- Table: header row `{typography.caption}` on `{colors.surface-soft}`,
  `1px solid {colors.hairline}` cells (print: `#999`); category rows bold,
  `{colors.surface-soft}`; name cell = name (600) + strength label
  (`{typography.code-sm}` `{colors.slate}`) so same-name drugs never blur
  together. The printed strength hides the Thai dispensing-unit suffix and no
  icode is printed; the settings picker still shows both. จำนวนใช้ is
  right-aligned mono; จำนวนเบิก is a click-to-edit number button
  (`sheet__request-button`) that looks like plain printed text.
- **Reason table**: when any drug was added or its quantity changed, the last
  page carries "ยาที่ขอเพิ่มหรือขอแก้ไขจำนวนเบิก" after the main table and
  before the signatures - exactly two columns, ชื่อยา and เหตุผล. The reasons
  are part of the confirmed print snapshot.
- Signature blocks (ลงชื่อ ผู้เบิก / ผู้จ่าย + date lines) are visible on
  screen too - the preview is the paper, and they render on the **last
  page only**. The ผู้เบิก line prints the signed-in officer's display name
  in parentheses; the ผู้จ่าย line stays blank for the pharmacy. Both date
  lines are pre-filled with the run day (the window end date) in Buddhist-era
  `dd / mm / yyyy`, e.g. `24 / 10 / 2569`.
- **Pagination is explicit**: one `<article class="sheet">` per A4 page,
  at most **40 drug rows per page** (`er-refill-core::ROWS_PER_PAGE`); the
  reason table shares the last page. Every page repeats the header block, the
  window sentence, and the column headers; a split category continues with
  "(ต่อ)"; the footer carries หน้า X / Y (and the print stamp on the last
  page). Row metrics (11.5px text, `2px 6px` cell padding, 20px number button)
  are tuned to this budget - do not loosen them without redoing the math.
- An empty report still renders one sheet page with the header and an "add a
  drug" hint, so the operator can start from a quiet night.
- Ordering is the catalog order - never re-sorted by quantity; added drugs are
  inserted in catalog order.

**`signature-block`** - Always visible at the sheet's foot (the screen
previews the paper): two columns "ลงชื่อ .................................. ผู้เบิก" /
"ผู้จ่าย", with the signed-in officer's name in the requester's parentheses,
a blank payer line, and a date line under each, `{typography.body-md}`. Both
dates are auto-filled from the requisition window's end date in Buddhist-era
numeric form (`24 / 10 / 2569`); the dotted form is only a fallback when no
window exists.

**`@media print`** - The rule that keeps the preview honest: chrome
(`.top-bar`, `.command-bar`, `.banner`, modal) hides with
`display: none !important`; the stage loses its padding, background, and
overflow; the sheet drops its width cap, min-height, padding (the `@page`
A4 portrait 12mm margins own it now), and shadow. The `@page` block also
declares empty top/bottom margin boxes, which suppresses Chromium's automatic
title/URL header and footer while keeping the 12mm paper margin. Cell borders
darken to `#999`, header/category backgrounds go transparent, and the
จำนวนเบิก number prints plainly. Every sheet after the first starts a new
printed page (`break-before: page`), and rows are `break-inside: avoid` so
nothing is ever sliced. One DOM, one stylesheet - the preview cannot diverge
from the printout.

**`add-drug-dialog`** - `dialog dialog--wide` opened by "เพิ่มรายการยา". It
lists **configured catalog drugs only** (excluding rows already on the sheet),
grouped in catalog order with name, icode, category, strength, and stock; a
search field filters locally. Selecting a drug reveals จำนวนเบิก and a required
reason. Confirming inserts the row with usage 0 in catalog order and records
the reason; the print confirmation is invalidated.

**`adjust-reason-dialog`** - Opened by clicking a จำนวนเบิก number on the
sheet. It shows the drug, the old → new quantity, the stock ceiling, and a
required reason. Confirming applies the rounded/clamped quantity and updates
the row's reason; cancelling changes nothing. Both this dialog and the add
dialog invalidate the print confirmation.

**`confirm-dialog`** - The print gate. It summarises the window, number of
rows, total requested, and the count of added/edited rows. Confirming snapshots
rows + reasons + window + header into `ConfirmedRequisition`; the print button
only becomes enabled for that snapshot, and the print action re-checks that the
current data is identical. Any later fetch, range change, catalog/report edit,
add, or quantity change clears the snapshot and disables printing until
the operator confirms again.

**`settings-modal`** - `{elevation.3}` dialog over a `rgba(17,17,19,0.4)`
backdrop, `{rounded.lg}`, max-width 960px. A 200px vertical `settings-nav`
rail (icons + การเชื่อมต่อ · รายการยา · หัวใบเบิก) beside a
scrolling `settings-content` area. Each panel keeps its own action row
(rose accent or black pill for save). The catalog panel stacks the category
tabs, the HOSxP search, and a compact editable drug table so the rows read
top-to-bottom instead of competing with a sidebar. The modal footer is a
`{colors.surface-soft}` strip with
`ส่งออกการตั้งค่า` / `นำเข้าการตั้งค่า` secondary pills + a status line -
the exported JSON carries the catalog and report header, never connection
credentials. "ส่งออกการตั้งค่า" opens the native OS Save As dialog
(`tauri-plugin-dialog`) so the operator chooses the folder and file name; the
status line then reports the full saved path (wrapped in the footer).

**`banner`** - Floating toast at the bottom-right of the stage (fixed, so it
never pushes the sheet or causes a layout jump): amber pair
(`{colors.warn-bg}`/`{colors.warn-text}`) with a dismiss ghost button and
`{elevation.2}`. Error banners use the red pair. It fades in 160ms
(`prefers-reduced-motion` disables the animation). Success/info toasts
auto-dismiss after 5 seconds; warning/error toasts stay until dismissed.

**`catalog-manager`** - Settings panel in three stacked bands:
- **Categories on top:** a wrapped row of pill tabs, each with the category
  name and a mono count. The active tab takes the rose border and soft-rose
  fill. The "เพิ่มหมวด" input/button sits at the right of the section heading;
  a slim row below the tabs edits the selected category name and offers
  "ลบหมวด" (disabled while the category still has drugs).
- **Search next:** a full-width search card with the magnifier, a
  "ค้นหายาจากบัญชี HOSxP" field, and a compact search button. Results appear in
  a bordered card with a count, a max-height scroll area, the icode chip and
  full strength label from `drugitems`, and a `+ เพิ่ม` secondary action. An
  added result leaves the list immediately.
- **Rows last:** a compact drug table with fixed column headers (# · ชื่อยา ·
  ความแรง · จำนวน/Stock · จัดการ). Rows are single-line grid rows: name (600)
  with icode chip, strength and stock inputs, then ↑/↓/remove icon buttons.
  This replaces the old two-line form cards so a category's items can be
  scanned quickly. The label is saved into the drug's `strengthText`, and the
  printed sheet trims the Thai unit suffix.

## Do's and Don'ts

### Do

- Reserve `{colors.brand-pink}` for accent CTAs, the selected range option,
  active tab indicator, focus rings, and confirmation checks - if it appears
  on a large generic surface, it did not earn that role.
- Use `{colors.primary}` black pills for the dominant action; exactly one rose
  accent CTA per viewport.
- Apply `{rounded.full}` to every button and pill; cards are always
  `{rounded.lg}`; the sheet is paper and stays square.
- Put every number that appears in a column into `{typography.code-*}` (mono,
  tabular) - quantities must align on the paper.
- Keep the range selection explicit: one rose-selected chip, a green "เลือกแล้ว"
  status, and the actual Thai/BE window spelled out below; fetch stays disabled
  until the operator picks one.
- Render strength beside the drug name everywhere (catalog, sheet, paper) so
  same-name drugs (Paracetamol 325 vs 500) never blur.
- Require a reason for every manual add or จำนวนเบิก change; show it in the
  reason table and keep it in the confirmed snapshot.
- Keep the print gate explicit: print is disabled until confirmation, and any
  edit invalidates the confirmation.
- Use `150-200ms ease` for state transitions; `{prefers-reduced-motion}`
  disables them.

### Don't

- Don't use `{colors.brand-pink}` on body text or large surfaces.
- Don't add a second accent hue beyond rose, info-blue, warn-amber, and error
  red. Bear gold belongs to the mascot artwork only.
- Don't use amber for "error" or red for "attention" - they are different
  signals and the pharmacist must distinguish them instantly.
- Don't add gradients anywhere except the login brand panel - the desk and
  the sheet stay flat.
- Don't build a second layout for printing: the stage sheet IS the print
  output (same DOM, same stylesheet); a separate hidden sheet is a bug
  waiting to happen.
- Don't add a drug that is not in the configured catalog, and don't add a row
  twice - additions are catalog-only and uniqued by icode.
- Don't print without a valid confirmation snapshot; don't let an edit leave
  the print button enabled.
- Don't re-sort the requisition table by usage - the order mirrors the
  physical form and the pharmacy's pick route; manual additions follow catalog
  order.
- Don't render Thai text in a Latin-only stack or Latin codes in the Thai
  face - the assignment is the brand voice.
- Don't reduce table text below 12.5px - legibility at arm's length beats
  fitting one more row.

## Responsive Behavior

A fixed-purpose desktop window (Tauri), 1280×900 default (clamped to the
monitor work area), min 900×620:

| Window width | Behavior |
|---|---|
| < 960px | The sheet goes full-width with reduced padding; the settings nav rail turns horizontal, giving the catalog panel more width. |
| < 1500px | Command bar stacks into two intentional rows (fixed date + range picker, then actions); the action buttons wrap rather than overflow. |
| ≥ 1500px | Full command bar; the sheet stays at `min(210mm, 100%)` and is centered on the desk with breathing room. |

Touch targets: buttons 40px effective height, icon-circular 32px desktop.
This is a mouse-class desktop app; there is no mobile layout.

## Motion

- State transitions: `background-color 150ms ease-out` (chips, buttons), focus
  borders 150ms.
- The selected range chip's rose state appears instantly - the color *is* the
  message; a cross-fade would slow reading it.
- No bounce/elastic easing anywhere; `prefers-reduced-motion` disables all
  transitions.

## Iteration Guide

1. Any new component declares its tokens from this file - no ad-hoc hex
   values in `app/style/main.css`.
2. Default body is `{typography.body-md}`; anything numeric-in-a-column is
   `{typography.code-*}`.
3. New states should first be expressible as weight/border/spacing changes;
   new named colors need a semantic role (selected/attention/error) to exist
   at all.
4. Re-check every new surface against "quiet except for the sheet and its window sentence".
5. Keep screen table and print table column-order identical - the screen is a
   proof of what prints.

## Known Gaps

- Dark mode is out of scope (no dark values defined; token names support it).
- The animated cloud/rocket aesthetic of the reference is deliberately not
  ported - only its gradient treatment survives, once, on the login brand panel.
- Print-color fidelity is assumed (rose/amber bands print as ink-saving text
  only; the sheet is black-on-white by design).
- The reason table shares the last A4 page with the signatures; a very large
  number of manual adjustments may need a manual page review before printing.

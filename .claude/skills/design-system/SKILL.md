---
name: design-system
description: Follow the Transit Console design system whenever creating or editing UI in this project (any .html, components.css, or design-system.css work — screens, modals, forms, tables, buttons, badges, cards, map panels, etc.). Enforces reuse of existing design tokens and components, the typography rule (minimum 14px, even sizes only), the 8px spacing grid, and the "reuse components, don't build new ones" principle. Use this before writing or changing any markup or styles.
---

# Transit Console — Design System Rules

This is a B2B SaaS design system for public-transport operators. The source of truth is
[design-system.css](../../../design-system.css) (tokens + typography) and
[components.css](../../../components.css) (components). **Never hardcode a value that a token
already exists for.** Read those two files first if unsure.

## Golden rules

1. **Reuse tokens, never hardcode.** Use `var(--token)` for every color, space, font size,
   radius, and shadow. If you're typing a raw hex, px color, or ad-hoc spacing, stop and use the token.
2. **Reuse components, don't build new ones.** Check the component inventory below first. Only
   create a new component when nothing composes — and when you do, build it **from existing tokens
   only** and add a comment noting it's new.
3. **Typography rule (strict): minimum font size is 14px, and every font size must be an even
   number.** Only use the `--font-*` tokens below. Never introduce 9/11/12/13/15px text.
   **The ONE exception:** `.badge` text is **12px** via `--font-badge`. Badges only — nothing else
   may go below 14px. Do not use `--font-badge` outside the badge component.
4. **Spacing must be EVEN numbers, always — no exceptions.** Every `padding`, `margin`, and `gap`
   value must be an even number of px, on **all screens**. Prefer the `--space-*` tokens (4/8/12/16/24/32/48/64 —
   all even). Never introduce odd spacing like 3px, 5px, 7px, 9px, 11px, 15px. If a token doesn't fit,
   round to the nearest even px, but a token is almost always the right answer.
   *Excluded from this rule* (these are not "spacing"): `border` / hairline widths (`1px` borders are fine),
   `box-shadow` offsets/spread, `letter-spacing`, and icon `width`/`height` (icons are sizing — keep those
   even too where practical, but they are not spacing).
5. **Match the surrounding file.** Mirror its structure, comment density, and idioms
   (e.g. reuse the `.modal` / `.form-group` patterns exactly as existing pages do).

## Tokens

### Typography — min 14px, even only
| Token | Size | Use |
|---|---|---|
| `--font-display` | 28px | Page hero |
| `--font-title` | 26px | Section title |
| `--font-h1` | 24px | H1 |
| `--font-h2` | 20px | H2 / modal title |
| `--font-h3` | 16px | H3 |
| `--font-body` | 14px | Body (the 14px floor) |
| `--font-small` | 14px | Helper / secondary text |
| `--font-caption` | 14px | Labels, metadata (uppercase via `.text-caption`) |
| `--font-badge` | 12px | **Badges only** — the sole sub-14 exception; never reuse elsewhere |

Weights: `--weight-regular` 400, `--weight-medium` 500, `--weight-semibold` 600.
Font family: `--font-family` = `'Inter', sans-serif` (Inter is **self-hosted** in `fonts/` — do not
re-add a Google Fonts dependency as the primary source).
Prefer the utility classes `.text-display/.text-title/.text-h1/.text-h2/.text-h3/.text-body/.text-small/.text-caption`.

### Color
Primary `--primary` #1A56DB, `--primary-hover` #1641A3.
Surfaces `--surface-background` #F7F8FA, `--surface-card` #FFFFFF. Border `--border-default` #E3E6EB.
Text `--text-primary` / `--text-secondary` / `--text-tertiary`.
Semantic `--success` / `--warning` / `--danger` / `--info`.
For tinted backgrounds use the established `rgba()` of the semantic color at 0.1 (see `.badge-*`).

### Spacing (8px grid — EVEN px only)
`--space-4 / -8 / -12 / -16 / -24 / -32 / -48 / -64`. Use `.gap-*`, `.p-*`, `.mb-*`, `.mt-*` utilities.
All `padding` / `margin` / `gap` must be even px (see golden rule 4). 1px borders are not spacing and are allowed.

### Radius / Elevation / Motion
Radius `--radius-button` 6px, `--radius-input` 6px, `--radius-card` 8px, `--radius-modal` 12px.
Shadow `--shadow-card`, `--shadow-modal`. Transitions `--transition-fast` 150ms, `--transition-normal` 250ms.

## Control height (buttons, inputs, selects, dropdown items)

All standard controls — and every dropdown **menu item** (`.ds-action-dropdown-item` and any
page-local equivalent) — are **40px tall** = `line-height: 20px` + `10px` vertical padding, with or
without a leading icon (a 16px icon is shorter than the 20px line, so the line sets the height).
Borders are drawn **inside** with `box-shadow: inset 0 0 0 1px <color>` (NOT a real `border`), so
they add 0 to the box and hover/focus/error just recolor the inset shadow with no size shift.
When building any input/select/button-like control: set `line-height: 20px`, `padding: 10px 16px`,
and use an inset-shadow border — don't use `border: 1px` (it would make the control 42px, misaligned).
Reuse `.input` / `.select` / `.btn` variants rather than re-implementing this.

## Component inventory (reuse these)

- **Buttons:** `.btn` + `.btn-primary` / `.btn-secondary` / `.btn-ghost` / `.btn-icon`. Disabled via `disabled`.
- **Forms:** `.form-group` > `.form-label` + `.input` (+ `.form-helper`, add `.error` to both for validation).
  `.select` (custom-styled), `.search-input` (has a built-in Lucide **search** icon via `::before`).
- **Cards:** `.card` (+ `.card-header/.card-body/.card-footer`, `.card-interactive`).
- **Table:** `.table-container` > `.table` (thead/`.sortable`, `tbody tr.selected`). `.table-link`.
- **Dropdown menu:** `.ds-action-dropdown` > `.ds-action-dropdown-item` (+ `.active`, `.danger`).
- **Badges:** `.badge` + `.badge-success/-warning/-danger/-info/-neutral`.
- **Empty state:** `.empty-state` (icon/title/description).
- **Modal:** `.modal-overlay`(+`.open`) > `.modal` > `.modal-header`(`.modal-title`) / `.modal-body` / `.modal-footer`.
- **Navigation:** `.top-bar`, `.nav-rail` + tree (`.tree-nav/.tree-row/...`), `.nav-item`, `.breadcrumb`.
- **Tabs:** `.tabs` > `.tab` (+ `.active`). **Avatar:** `.avatar` (+ `.avatar-lg/-xs`). **Org switcher:** `.org-switcher`.
- **Toast:** `.toast` (fixed bottom-right; add `.toast-success/-error/-warning`).
- **Loading:** `.skeleton` (+ `.skeleton-text/-title/-row`).
- **Layout:** `.app-layout/.app-main/.app-content`, `.auth-layout`, `.centered-layout`, plus flex/grid utilities.

## Gotchas

- **`<button>` / `<input>` / `<select>` / `<textarea>` do not inherit `font-family`.** Any custom
  button-based component that shows text (menu items, tabs, pills) **must** set `font-family: var(--font-family)`
  explicitly, or it renders in the browser default (Arial-like), not Inter.
- Native `<select>` option **popups** are OS-rendered and can't be fully styled — use the custom
  `.ds-action-dropdown` menu when the option list must be on-brand.

## Icons

Uses **Lucide** via `<i data-lucide="name"></i>` and `lucide.createIcons()`. After injecting any new
icon markup dynamically, call `lucide.createIcons()` again. Keep icon `width/height` even (14/16/20px).

## Standard patterns (copy these shapes)

Form field with inline validation:
```html
<div class="form-group">
  <label class="form-label" for="id">Label</label>
  <input type="text" id="id" class="input" placeholder="…" />
  <span class="form-helper" id="id-h">Helper text.</span>
</div>
```
Toggle `.error` on both `.input` and `.form-helper` to show an error; swap the helper text for the message.

Modal shell: reuse `.modal-overlay/.modal/.modal-header/.modal-body/.modal-footer` exactly as in
existing pages; open by adding `.open` to the overlay and close on overlay-background click.

## Before you finish (checklist)

- [ ] No hardcoded colors/spacing/radii/shadows — all `var(--token)`.
- [ ] Every `padding`/`margin`/`gap` is an EVEN px value (tokens preferred). No 3/5/7/9/11/15px spacing. (1px borders excluded.)
- [ ] Every font size is a `--font-*` token; nothing below 14px (except `.badge` = 12px via `--font-badge`); no odd px sizes anywhere.
- [ ] Reused an existing component instead of inventing one (or documented why a new one was unavoidable).
- [ ] New markup mirrors the conventions of the file it lives in.
- [ ] Icons are Lucide, sizes even; `lucide.createIcons()` runs after dynamic injection.

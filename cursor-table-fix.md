# Cursor Fix Instructions — SLDS v1 Table & Nav Corrections

The file `index-lightning-blue.html` has the right structure and content but two areas need to be corrected to match native Salesforce SLDS v1 rendering. Apply these fixes now.

---

## Fix 1 — Global Nav Bar color

The nav bar background is currently rendering as white or light gray. This is wrong.

**Correct it:**
- Background must be `#0b5cab` (Lightning Blue — the SLDS v1 theme color)
- All text, icons, and dividers in the nav bar must be white (`#ffffff`)
- The app name "FanDuel VIP KAM Console" must be white, 13px, font-weight 700
- The right-side icons (?, ⚙, 🔔, avatar) must all be white

```css
.global-nav {
  background-color: #0b5cab;
  color: #ffffff;
}
.global-nav * {
  color: #ffffff;
}
```

---

## Fix 2 — VIP Table must match native SLDS list view pattern

The current table uses custom styling. Replace it with the exact SLDS v1 list view pattern as it renders natively in Salesforce.

### Card wrapper
The table must sit inside an `slds-card`. The card header has:
- Left: object icon (purple square, 16px) + list view name "My VIP Playbook" (bold, 13px) + dropdown arrow
- Right: action buttons — "New" (neutral), "Import" (neutral), "Add to Campaign" (neutral), "Log Contact" (brand blue), overflow "▾" button
- Below the card header: a thin `1px solid #dddbda` divider

```html
<div class="slds-card">
  <div class="slds-card__header slds-grid">
    <header class="slds-media slds-media_center slds-has-flexi-truncate">
      <!-- icon + title left -->
    </header>
    <div class="slds-no-flex">
      <!-- action buttons right -->
    </div>
  </div>
  <div class="slds-card__body">
    <!-- toolbar + table -->
  </div>
</div>
```

### Toolbar (below card header, above table)
- Left side: search input (`slds-input` with magnifier icon, placeholder "Search this list...")
- Right side: filter icon button + list view controls (⚙ gear, ↺ refresh, ✎ edit pencil)
- Below toolbar: quick filter tabs — All VIPs | Overdue Contact | Overbonused | Shield Select
  - These are `slds-tabs_scoped` or inline `slds-button` group tabs — NOT custom pill buttons
  - Active tab: `#0176d3` text with bottom border `3px solid #0176d3`

### Table element
Use these exact SLDS classes:
```html
<table class="slds-table slds-table_cell-buffer slds-table_bordered slds-table_fixed-layout">
```

### Table header row (`<thead>`)
- Background: `#f3f2f2` — apply via `background-color: #f3f2f2` on the `<thead>` or `<tr>` inside it
- Text: uppercase, 11px (`0.6875rem`), `#706e6b`, font-weight 700 — use `slds-text-title_caps`
- Height: 40px
- Each `<th>` has: text + sort arrow icon (`↑` or `↕`) on hover
- Checkbox column: 32px wide, center-aligned
- Column borders: `1px solid #dddbda` right border on each header cell

```html
<thead>
  <tr class="slds-line-height_reset">
    <th class="slds-text-title_caps" style="background:#f3f2f2; width:32px;">☐</th>
    <th class="slds-text-title_caps" style="background:#f3f2f2;">Name</th>
    <th class="slds-text-title_caps" style="background:#f3f2f2;">Segment</th>
    <!-- etc -->
  </tr>
</thead>
```

### Table body rows (`<tbody>`)
- Row height: 48px (min-height, use `padding: 0.5rem` on `<td>`)
- Row background: `#ffffff`
- Row hover: `background-color: #f3f9ff` via `:hover` on `<tr>`
- Row border: `1px solid #dddbda` bottom border on each `<tr>`
- Selected row: `background-color: #e8f4fd`
- Cell text: 13px (`0.8125rem`), `#181818`

### Column-specific cell rendering

**Name cell:**
```html
<td>
  <a href="#" class="slds-truncate" style="color:#0176d3; font-weight:600;">James Harrington</a>
  <!-- if NBA pending: -->
  <span title="NBA Action pending" style="color:#dd7a01; margin-left:4px; cursor:help;">⚠</span>
</td>
```

**GGR L30 cell:**
- Positive value: `color: #2e844a; font-weight: 700`
- Negative value: `color: #ba0517; font-weight: 700`

**Last Contact cell:**
- >20 days: `color: #ba0517` (red)
- 11–20 days: `color: #dd7a01` (amber)
- ≤10 days: `color: #706e6b` (gray)

**Status cell — SLDS badges:**
```html
<!-- Active -->
<span class="slds-badge" style="background:#2e844a; color:#fff; border-radius:4px; padding:2px 8px; font-size:11px; font-weight:700; text-transform:uppercase; letter-spacing:.5px;">ACTIVE</span>

<!-- Suspended -->
<span class="slds-badge" style="background:#dd7a01; color:#fff; ...">SUSPENDED</span>

<!-- Deactivated -->
<span class="slds-badge" style="background:#706e6b; color:#fff; ...">DEACTIVATED</span>
```

**Shield Select tier:**
```html
<span>
  <svg style="width:12px;height:12px;fill:#c89b3c;vertical-align:middle;"><!-- shield path --></svg>
  Shield Select
</span>
```

**Last Bonus Issued cell:**
```html
<!-- Normal -->
<td>
  <div style="font-weight:600;">$500</div>
  <div style="font-size:11px; color:#706e6b;">Bonus Bet · Aug 28</div>
</td>

<!-- Overbonused -->
<td>
  <div style="font-weight:600;">$0 <span class="slds-badge" style="background:#ba0517;color:#fff;font-size:10px;">Overbonused</span></div>
  <div style="font-size:11px; color:#706e6b;">Bonus Bet · Aug 10</div>
</td>
```

**Actions cell (show on row hover only):**
```html
<td class="row-actions" style="opacity:0; transition:opacity .15s;">
  <button class="slds-button slds-button_neutral" style="height:24px;font-size:12px;">Contact</button>
  <button class="slds-button slds-button_icon slds-button_icon-border-filled" style="height:24px;width:24px;">⋮</button>
</td>
```
```css
tr:hover .row-actions { opacity: 1; }
```

### Pagination bar (below table)
```html
<div style="display:flex; align-items:center; justify-content:space-between; padding:8px 12px; border-top:1px solid #dddbda; font-size:13px; color:#706e6b;">
  <span>Showing 1–10 of 47 VIPs</span>
  <div>
    <button class="slds-button slds-button_neutral">‹ Prev</button>
    <button class="slds-button slds-button_neutral">Next ›</button>
  </div>
</div>
```

---

## Fix 3 — All other cards must also match SLDS card pattern

Every section (Real-Time Awareness, NBA Queue, Tasks & WPC, etc.) must use the `slds-card` wrapper with:
- `slds-card__header` with title left, actions right
- `slds-card__body` for content
- Card border: `1px solid #dddbda`
- Card background: `#ffffff`
- Card shadow: `0 2px 2px rgba(0,0,0,.06)`
- Card header height: 52px
- Card title: 13px, font-weight 700, `#181818`

Tabs inside cards (e.g. Big Swings / Payment Alerts / Losing Streaks) must use:
```html
<div class="slds-tabs_default">
  <ul class="slds-tabs_default__nav">
    <li class="slds-tabs_default__item slds-is-active"><a class="slds-tabs_default__link">Big Swings</a></li>
    <li class="slds-tabs_default__item"><a class="slds-tabs_default__link">Payment Alerts</a></li>
  </ul>
</div>
```

---

## What NOT to change
- All mock data content (VIP names, values, etc.) — keep exactly as is
- The 2-column layout — keep exactly as is
- Component placement and order — keep exactly as is
- Modal functionality — keep exactly as is
- All JS interactions — keep exactly as is

Only fix the visual styling to match SLDS v1 as described above.

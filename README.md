# Family Income & Expense Tracker

A single-file browser app for tracking multiple income sources, category and subcategory expenses, family-member spending, monthly budgets, and cashflow — with planning tools built for **irregular income**.

No build step, no dependencies, no server, no network calls. One `index.html` you can open, email to yourself, or host anywhere.

**Live demo:** <https://hasnaina955.github.io/expense-tracker/>

---

## Contents

- [Why this exists](#why-this-exists)
- [Quick start](#quick-start)
- [The eight tabs](#the-eight-tabs)
- [Core concepts](#core-concepts)
- [Keyboard shortcuts](#keyboard-shortcuts)
- [Data and storage](#data-and-storage)
- [Backup and restore](#backup-and-restore)
- [Printing](#printing)
- [Customising it](#customising-it)
- [How it is built](#how-it-is-built)
- [Development and testing](#development-and-testing)
- [Browser support](#browser-support)
- [Privacy](#privacy)
- [Known limitations](#known-limitations)

---

## Why this exists

Most expense trackers assume a salary lands on the same day every month. This one is built around income that arrives in lumps — freelance payouts, commissions, committee maturities, seasonal work — so the question it answers is not "what did I spend?" but **"given what I actually earn on average, how much can I safely spend this month?"**

That is what the **Income smoothing** card on the dashboard is for. It averages your income and expenses across a rolling 3, 6, or 12-month window and shows the difference as a safe-to-spend figure.

---

## Quick start

### Use it locally

1. Download `index.html`.
2. Double-click it.

That's it. It works offline. Your entries are saved in your browser's local storage, so they persist between sessions on that browser.

The dashboard opens on the current month, pre-filled with fictional demo data so you can see how it works before entering anything of your own. See [Built-in sample data](#built-in-sample-data).

### Use the hosted version

Open <https://hasnaina955.github.io/expense-tracker/>. It is the same file, served by GitHub Pages.

> The hosted copy and a local copy **do not share data** — storage is scoped to the browser *and* the site address. Pick one, or move data between them with Export/Import JSON (see [Backup and restore](#backup-and-restore)).

### Publish your own copy

1. Push `index.html` to a GitHub repository.
2. Open the repository's **Settings** → **Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**.
4. Select branch **main**, folder **/ (root)**, then **Save**.
5. Wait for the deployment, then open the URL GitHub shows.

---

## The eight tabs

Switch tabs by clicking, or press <kbd>1</kbd>–<kbd>8</kbd>.

| # | Tab | What it does |
|---|-----|--------------|
| 1 | **Dashboard** | Everything for the selected month: KPIs, alerts, analytics, planning, breakdowns, drill-downs |
| 2 | **Budgets** | Set and edit a monthly spending limit per category |
| 3 | **Recurring** | Define bills and income that repeat, then log a whole month in one click |
| 4 | **+ Income** | Add an income entry |
| 5 | **+ Expense** | Add an expense entry |
| 6 | **Transactions** | Search, filter, edit and delete every entry |
| 7 | **Cashflow** | Month-by-month income, expenses, and running balance for a year |
| 8 | **Settings & Lists** | Edit sources, members, payment methods, categories; backup and restore |

### Dashboard

Reads top to bottom in order of urgency:

1. **Alerts** — compact banners that only appear when something needs you:
   - *Budget health* — shows when you have budgets; turns amber when any is over or near its limit. Lists up to six categories, sorted by how much of the budget they have used.
   - *Recurring due* — shows when recurring rules haven't been logged for this month yet, with a **Log due now** button.
2. **This month** — five KPI cards: total income, total expenses, net cashflow, savings rate, and transaction count.
3. **At a glance** — average expense, largest expense, top category, top member, active spending days. Clicking *Top category* or *Top member* jumps to Transactions pre-filtered to it.
4. **Planning** — the income-smoothing card (see [Core concepts](#core-concepts)).
5. **Breakdown** — four panels: expenses by category, expenses by family member, income by source, and daily spending.

**Drill-downs.** Click any row in *Expenses by Category* to see that category's subcategory breakdown plus its transactions. Click any row in *Expenses by Family Member* to see everything attributed to that person. Press <kbd>Esc</kbd> or **Close ✕** to dismiss.

### Budgets

Pick a category, enter a monthly limit, and press **Set budget**. Each row shows a progress bar and how much is left; you can edit the limit inline or remove it with ✕.

Colour thresholds:

| Usage | Colour | Meaning |
|-------|--------|---------|
| < 80% | Green | On track |
| 80–99% | Amber | Close to the limit |
| ≥ 100% | Red | Over budget |

Limits apply to **every** month and are measured against whichever month the dashboard is showing.

### Recurring

Define a rule once — rent, an SIP, school fees, a salary, a committee payout — and log it into the dashboard month on its due day.

Each rule has a **type** (expense or income), description, amount, and day of month. Expense rules also carry category, subcategory, member and payment method; income rules carry a source.

- **Log all due for this month** adds an entry for every active rule not yet logged this month.
- Once logged, a rule is marked *✓ logged* for that month and won't be added twice.
- **Pause** a rule to skip it without deleting it. Paused rules are never logged.
- The due day is clamped to the length of the month, so a rule due on the 31st is logged on 28 February rather than on a date that doesn't exist.

### Transactions

Filter by free-text search, date range, type, category/source, and family member. Matching text is highlighted in the results.

- **Group by day** (default) inserts a heading per date; **Flat view** removes them.
- **Edit** opens a dialog to change any field.
- **✕** deletes the entry — with an **Undo** option on the toast.
- **Clear** resets all filters.

Search covers date, category, subcategory, member, description, payment method, and amount.

### Cashflow

Choose a year and an opening balance as of 1 January. The table shows each month's income, expenses, net, and a running **closing balance** carried forward from the opening balance.

### Settings & Lists

Edit your **income sources**, **family members**, **payment methods**, and **categories & subcategories**. Removing an item from a list does not delete entries that already use it — their text is preserved, and the app re-adds any missing value to the list automatically when data is loaded.

---

## Core concepts

### The dashboard month

The month picker in the header drives the **entire dashboard** — KPIs, breakdowns, budget progress, recurring status, and the smoothing window. It does not affect Transactions or Cashflow, which have their own date controls.

### Budgets

Budgets are **monthly and recurring by nature**: a ₹8,000 grocery limit means ₹8,000 every month, not once.

### Recurring rules

A rule is *due* for a month when it is not paused, has not already been logged for that month, and (if a start month is set) that month has arrived. Logging stamps the rule so it won't be logged again — this is what makes "Log due" safe to click.

### Income smoothing

For irregular income, this is the number to trust. It looks back over your chosen window from the dashboard month, averages income and expenses, and shows:

- **Avg monthly income**
- **Avg monthly expenses**
- **Safe to spend / mo** — the difference
- **A sparkline** of your monthly income trend

The window (3, 6, or 12 months) is chosen on the card and remembered per browser. Six months is the default.

### Savings rate

Net cashflow divided by total income, as a percentage. Shows `—` when the month has no income.

---

## Keyboard shortcuts

| Key | Action |
|-----|--------|
| <kbd>/</kbd> | Jump to search |
| <kbd>1</kbd> … <kbd>8</kbd> | Switch tabs |
| <kbd>N</kbd> | New expense |
| <kbd>I</kbd> | New income |
| <kbd>D</kbd> | Toggle dark mode |
| <kbd>P</kbd> | Print the current tab |
| <kbd>Esc</kbd> | Close help → close edit dialog → close drill-down |
| <kbd>?</kbd> | Show this help |

Two rules worth knowing:

- Shortcuts are **ignored while you are typing** in a field, so typing "N" into a description won't jump you to the expense form. <kbd>Esc</kbd> still works there — it just leaves the field.
- Shortcuts ignore combinations with <kbd>Ctrl</kbd>/<kbd>Cmd</kbd>/<kbd>Alt</kbd>, so browser and OS shortcuts are never intercepted.

---

## Data and storage

### Where data lives

Everything is stored in your browser's `localStorage` under two keys:

| Key | Contents |
|-----|----------|
| `familyExpenseTracker_v1` | All entries, lists, budgets, and recurring rules |
| `familyExpenseTracker_theme` | Your light/dark preference |

The data key is deliberately **versioned and stable** (`_v1`, with `schemaVersion: 2` inside the payload). Updating or replacing `index.html` does **not** reset your data.

### Built-in sample data

The app ships with **fictional demo data** so it does something useful on first open. It loads only when there is no valid saved data.

The demo is **generated at load time and anchored to the current month**, so the dashboard is never empty and the smoothing and sparkline features always have history to work with. It contains 6 months of entries — a steady salary, irregular freelance payouts (which is what the smoothing card is there to illustrate), and household spending across 16 categories — plus generic lists: 10 members as relationship labels (`Self`, `Spouse`, `Child 1`, `Parent 1`, …), 4 income sources, and 6 payment methods. It has **no budgets and no recurring rules**, so those two features start empty by design.

The figures are rounded placeholders and the names are generic relationships. **None of it is real financial data** — see [Privacy](#privacy).

**Reset to built-in data** in Settings restores it and is undoable.

### Data shape

```jsonc
{
  "schemaVersion": 2,
  "storageKey": "familyExpenseTracker_v1",
  "sources":  ["Salary", "Freelance"],
  "members":  ["Self", "Spouse"],
  "payments": ["Cash", "UPI", "Card"],
  "categories": { "Housing": ["Repairs", "Home Allowance"] },
  "income": [
    { "id": "demo1k3x9p", "date": "2026-09-05", "source": "Salary", "desc": "", "amount": 52000 }
  ],
  "expenses": [
    { "id": "demo2m7qb1", "date": "2026-09-02", "cat": "Housing", "sub": "Rent",
      "member": "Home", "desc": "", "payment": "Bank Transfer", "amount": 14000 }
  ],
  "recurring": [
    { "id": "r1", "type": "expense", "desc": "Rent", "amount": 12000, "day": 5,
      "start": "", "lastApplied": "", "paused": false,
      "cat": "Housing", "sub": "Rent", "member": "Home", "payment": "Bank Transfer" }
  ],
  "budgets": { "Groceries": 8000 },
  "openingBalance": 0,
  "smoothWindow": 6
}
```

Notes on the shape:

- Dates are `YYYY-MM-DD` strings. `lastApplied` is a `YYYY-MM` month stamp.
- Income rules use `source`; expense rules use `cat` / `sub` / `member` / `payment`. Both are always present in stored data, so switching a rule's type is safe.
- `start` is reserved for starting a rule in a future month. The UI currently leaves it empty, so rules are due from the current month onward.
- Amounts are plain numbers. They are stored exactly but **displayed rounded to whole rupees** (Indian numbering: ₹1,23,457).

### Self-healing on load

Whenever data is loaded — at startup or on import — `normalize()` repairs it rather than rejecting it. It fills in missing lists, coerces malformed values, drops recurring rules with a non-positive amount, and re-adds any source, member, payment method, category, or subcategory that existing entries reference but that is missing from the lists. This makes hand-edited and partially-formed JSON safe to import.

### Undo

A toast with **Undo** appears after deleting an entry, logging recurring rules, resetting to built-in data, or clearing all entries. Undo is **single-level** — it restores the most recent snapshot only — and the toast lasts 6 seconds (other toasts last 1.8).

Undo does **not** cover removing a budget, deleting a recurring rule, or removing items from your lists. Those ask for confirmation instead.

---

## Backup and restore

In **Settings & Lists → Data & Backup**:

| Action | What you get |
|--------|--------------|
| **Export JSON** | A pretty-printed full snapshot: entries, lists, budgets, recurring rules, opening balance, smoothing window. This is your real backup. |
| **Export CSV** | A flat spreadsheet of all entries (`Type, Date, Category/Source, Subcategory, Member, Description, Payment, Amount`) for analysis in Excel or Google Sheets. |
| **Import JSON** | Restores a JSON export. |
| **Reset to built-in data** | Replaces everything with the bundled sample. Undoable. |
| **Clear all entries** | Deletes all income and expense entries but keeps your lists, budgets, and rules. Undoable. |

**Export JSON regularly.** Local storage is per-browser and is erased by clearing browsing data, by most "clean up" utilities, and at the end of a private/incognito session. A JSON export is also how you move data between devices or between the hosted and local copies.

**Import replaces everything** — it does not merge. If you want to combine two sets of entries, export both and merge the `income` and `expenses` arrays by hand before importing. The importer validates the file and shows an error if it isn't a recognised export rather than silently corrupting your data.

---

## Printing

Press <kbd>P</kbd> or use the 🖨 Print button on the **Transactions** or **Cashflow** tab.

Only the tab you are currently viewing prints. The print stylesheet strips navigation, filters, buttons, and toasts; removes shadows and background gradients; expands scrollable areas so nothing is cut off; and avoids breaking cards across pages.

---

## Customising it

Everything is in one file, so small edits are easy. Find the relevant block with a text search.

| To change… | Look for |
|------------|----------|
| Demo data | `function makeSeed()` near the top of the `<script>` block |
| Currency or number format | `function money(x)` — currently `en-IN` / `INR` with no decimals |
| Budget colour thresholds | `function budgetRows()` — the `pct>=100` / `pct>=80` checks |
| Default smoothing window | `s.smoothWindow` in `normalize()` — currently `6` |
| Colours | The `:root` (light) and `[data-theme="dark"]` token blocks |
| Category list shipped with the app | `categories` inside `SEED` |

### Colour tokens

All colours are CSS custom properties defined in two blocks: `:root` for light mode and `[data-theme="dark"]` for dark mode. Components reference them as `var(--…)`.

**Convention: no raw hex colours outside those two token blocks.** The only deliberate exception is the `@media print` block, which is intentionally theme-independent. Keeping to this means a new theme is a matter of editing one block, and dark mode can never drift out of sync with light mode.

---

## How it is built

```
expense-tracker/
├── index.html   # the entire application: markup, styles, and logic
└── README.md
```

One file, roughly 1,170 lines, in three sections:

1. **`<style>`** — design tokens, then component styles, then responsive breakpoints (900px, 760px, 520px) and print styles.
2. **`<body>`** — eight `<section class="tab">` panels. Only the active one is displayed.
3. **`<script>`** — plain ES5-flavoured JavaScript. No framework, no bundler, no imports.

State is a single plain object held in `state`, persisted with `save()` (a `JSON.stringify` into local storage). Every `render*()` function reads `state` and rewrites one region of the DOM; there is no virtual DOM and no diffing.

Notable implementation details:

- **Render functions are idempotent.** Each one fully rewrites its own container, then re-binds event handlers to the fresh nodes. This is why adding an entry and rendering again can't duplicate rows.
- **`normalize()` is the single gate for untrusted data**, used on startup, after import, and after undo.
- **Layout uses `auto-fit` grids with `minmax(min(Npx, 100%), 1fr)`.** The `min()` is important: without it, a grid with a 340px floor overflows the viewport on a ~360px phone instead of collapsing.
- **Escaping** goes through `esc()` for values and `hl()` for search-highlighted values. Never interpolate user text into HTML without one of them.

---

## Development and testing

There is nothing to install and nothing to build. Edit `index.html` and reload.

To guard against regressions, verify changes in a headless DOM before committing. The approach that works well here:

```bash
npm install jsdom
```

```js
const { JSDOM } = require('jsdom');
const dom = new JSDOM(html, {
  runScripts: 'dangerously',
  beforeParse(w) { /* stub w.localStorage with an in-memory object */ }
});
```

Because the app keeps everything on `window`, a test can drive it directly: set `window.state`, call `window.renderDashboard()`, click real buttons with `.click()`, and assert on the resulting DOM.

Two traps worth knowing:

- **The bundled sample data has no budgets and no recurring rules.** Any test that asserts on those features without first injecting fixture data will pass vacuously against an empty array.
- **jsdom has no `scrollIntoView`.** The app guards that call; unguarded equivalents will throw.

Useful checks to run: no uncaught errors on load, no errors after clicking through all eight tabs, no raw hex outside the token blocks, and that drill-downs and undo still work.

---

## Browser support

Current versions of **Chrome, Edge, Firefox, and Safari** on desktop and mobile.

The app uses standard features available in all of them: `localStorage`, `Intl.NumberFormat`, `FileReader`, `URL.createObjectURL`, CSS custom properties, flexbox and grid (including `min()` and `auto-fit`), `:focus-visible`, and `backdrop-filter` (which degrades to a translucent background where unsupported).

It also honours `prefers-reduced-motion`, disabling animations and transitions for anyone who has asked for that at the OS level.

Internet Explorer is not supported.

---

## Privacy

**Your data never leaves your browser.** There is no analytics, no telemetry, no external font or script loading, and no network request of any kind. Everything — including the sample data — is inside `index.html`.

Two consequences worth planning around:

- **Data is tied to one browser on one device.** Clearing site data, using a private window, or switching devices means starting from the sample data unless you have a JSON export.
- **Anyone with access to that browser profile can read your entries.** There is no login or encryption. If that matters, use a browser profile the others don't use.

**No real financial data is shipped.** The bundled demo data is generated at load time from `makeSeed()`: rounded placeholder amounts and generic relationship labels (`Self`, `Spouse`, `Child 1`, `Parent 1`), with no connection to any real person or account. Your own entries live only in your browser and are never written into `index.html`, so they cannot end up in the repository.

---

## Known limitations

- **No sync between devices or browsers.** Export/Import JSON is the mechanism.
- **Single-level undo.** Only the most recent snapshot is recoverable, and only for the four actions listed in [Undo](#undo).
- **Import replaces rather than merges.** Combining two data sets requires editing the JSON by hand.
- **Single currency.** Amounts are formatted as INR throughout; there is no per-entry currency.
- **Amounts are displayed rounded to whole rupees** even though exact values are stored, so a column of displayed figures may be off by a rupee from its displayed total.
- **Local storage is finite** — typically around 5 MB. That is tens of thousands of entries, but it is not unlimited, and it is shared with other data for the same site.
- **Demo data is regenerated fresh each time it is first loaded**, so "Reset to built-in data" gives you figures keyed to today's month rather than the exact set you saw before. Your own entries are never affected.
- **Budgets are category-wide, not per-member.** You cannot budget "₹2,000 for Child 1's transport".
- **Recurring rules have no end date.** Pause or delete is how you stop one.
- **The `start` field on recurring rules is not exposed in the UI**, so rules are effectively due from the current month onward.

---

## Licence

**MIT** — see [`LICENSE`](LICENSE).

Copyright (c) 2026 hasnaina955. In short: you may use, copy, modify, merge, publish, distribute, sublicense, and sell this software, including in commercial projects, as long as you keep the copyright notice. It comes with no warranty.

Because the whole app is one file, "keeping the notice" in practice means leaving the `LICENSE` file alongside it, or keeping the notice in a header comment if you fold `index.html` into something larger.

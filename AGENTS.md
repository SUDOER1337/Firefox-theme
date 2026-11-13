# AGENTS.md

UserChrome theme for LibreWolf 160 (Firefox 160 internals): Gruvbox light palette,
square corners, horizontal tab strip hidden in favour of Sidebery.

## Files

- `userChrome.css` — browser chrome only (toolbars, urlbar, panels, menus, sidebar, findbar).
- `userContent.css` — page/content context. Only `about:blank` / `about:newtab`.
- `README.md` — user-facing behaviour + palette tables. Keep in sync with the CSS.

## No tooling

There is no build, test, linter, formatter, git repo, or package manifest. Nothing to run.
CSS errors are silent — a typo just does nothing. Verify by restarting the browser and
looking; inspect the chrome DOM with **Browser Toolbox** (Ctrl+Shift+Alt+I) when a
selector misbehaves.

Changes apply **only on restart** (`toolkit.legacyUserProfileCustomizations.stylesheets`
is already `true` in `../prefs.js`). No hot reload, no save-and-see.

## Gotchas that cause wrong fixes

- **Two palette layers.** Raw `--gb-*` values, then semantic `--lw-*` aliases that everything
  else consumes (`userChrome.css:7-62`). Add or change colours in the alias layer; do not
  hardcode hex in selectors. `--lw-hover` is the one intentional literal.
- **Variables do not cross into `userContent.css`.** It runs in the content process, so it
  cannot see `--gb-*` / `--lw-*` and must hardcode the same hex values.
- **Firefox redeclares many tokens on the widget itself**, so overriding `:root` alone is not
  enough. Overrides are deliberately duplicated on `#urlbar` / `#searchbar`
  (`userChrome.css:283-306`) and on `menupopup, panel, .panel-arrowcontent`
  (`userChrome.css:386-404`). Keep both copies when editing.
- **Shadow DOM is unreachable by `*`.** Squaring uses design tokens
  (`--border-radius-*`, `--button-*`, `--input-*`, `--panel-list-*`, `userChrome.css:41-87`)
  on top of the single `* { border-radius: 0 !important; }` rule (`userChrome.css:66-68`).
  To change the look of corners, change that one value — don't chase selectors.
- **Do not delete the tab-strip section casually.** `userChrome.css:177-208` hides
  `#TabsToolbar`, which also owns minimize/maximize/close, and re-exposes the nav-bar copy
  because Firefox only sets `[tabs-hidden]` in native vertical-tabs mode. Removing the hide
  rule is the supported way to restore the tab strip; removing the reveal rules breaks the
  window buttons.
- **`!important` is load-bearing** throughout — it beats UA styles and theme-provided tokens.
  Don't strip it as general cleanup.
- **Sidebery is styled in the extension's own settings**, not from here. Palette changes must
  be mirrored manually in Sidebery → Appearance → Colors (values in `README.md:25-32`).
- **Palette drift is the main maintenance risk**: the same hexes live in the `--gb-*` block,
  `userContent.css:14-15`, and the README tables. Update all three together.
- Selectors and token names are version-specific Firefox internals (the newer "Nova"
  design-system names). Do not invent plausible-looking names — confirm them in the Browser
  Toolbox against the running build.

## Style

Follow the existing conventions: section banner comments (`/*    Section */`) with prose
explaining *why* a rule or override is needed, alphabetical-ish grouping inside token blocks,
and inline comments naming which Gruvbox role a token maps to.

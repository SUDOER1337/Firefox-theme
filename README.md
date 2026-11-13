# Gruvbox unrounded Firefox theme

A warm light theme for Firefox or any of it's fork , built on the Gruvbox 
palette with square corners.

Intended to be use with [Sidebery](https://addons.mozilla.org/firefox/addon/sidebery/) draws the vertical
tab list in the sidebar instead. The address bar and window buttons stay.

| File              | Does                                                         |
| ----------------- | ------------------------------------------------------------ |
| `userChrome.css`  | Gruvbox colors for toolbars, address bar, menus, panels, sidebar, find bar; hides the tab strip |
| `userContent.css` | Cream background for `about:blank` / `about:newtab` (no color flash) |

## Apply / update

`toolkit.legacyUserProfileCustomizations.stylesheets` is `true` in `about:config`.
Then restart LibreWolf — CSS is loaded at startup. If nothing changes, confirm

## Sidebery

All tab management happens in Sidebery (`+` button, `Ctrl+T`, search, pinning,
containers). It paints its own UI, so style it in **Sidebery Settings →
Appearance → Colors** with the same palette:

| Role              | Value     |
| ----------------- | --------- |
| Background        | `#f2e5bc` |
| Raised / selected | `#fbf1c7` |
| Border            | `#d5c4a1` |
| Text              | `#3c3836` |
| Muted text        | `#7c6f64` |
| Accent            | `#b57614` |

## Top bar

- Minimize / maximize / close are top-right (the nav-bar's own window buttons,
  which Firefox otherwise only shows in vertical-tabs mode). The empty part of
  that toolbar still drags the window.
- The menu bar still appears with `Alt`.
- For the normal tab strip back: delete the `Hide the horizontal tab strip`
  section in `userChrome.css` and restart (Sidebery can stay installed).

## Customize

Edit the `--lw-*` variables at the top of `userChrome.css`; they alias the raw
`--gb-*` palette:

| Variable        | Default          | Used for                          |
| --------------- | ---------------- | --------------------------------- |
| `--lw-bg-deep`  | `#ebdbb2` (bg1)  | urlbar field, window background   |
| `--lw-bg`       | `#f2e5bc` (soft) | toolbar / chrome background       |
| `--lw-bg-elev`  | `#fbf1c7` (bg0)  | selected tab, menus, panels       |
| `--lw-hover`    | `#eadcb4`        | hover highlights                  |
| `--lw-border`   | `#d5c4a1` (bg2)  | outlines and separators           |
| `--lw-text`     | `#3c3836` (fg1)  | primary text                      |
| `--lw-dim`      | `#7c6f64` (fg4)  | inactive text / placeholders      |
| `--lw-accent`   | `#b57614`        | focus rings, selected tab stripe  |
| `--lw-link`     | `#076678` (blue) | URLs in the address bar results   |

Other accents: `--gb-red`, `--gb-green`, `--gb-orange`, `--gb-purple`,
`--gb-aqua`.

**Corners:** a single `* { border-radius: 0 !important; }` rule squares the whole
chrome — change that `0` (e.g. `4px`) instead of chasing individual selectors.

## Disable / remove

- Temporarily: set `toolkit.legacyUserProfileCustomizations.stylesheets` to
  `false` in `about:config` and restart.
- Permanently: delete this `chrome` folder and restart.

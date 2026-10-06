# CLAUDE.md — Signal Cleveland scrollytelling stories

Guidance for building pinned-photo "scrolly" stories for Signal Cleveland (signalcleveland.org, a Newspack/WordPress site).

## Design conventions (Signal brand)

- Fonts: **Inter** for body, **Roboto Condensed Bold** for headlines/subheads (`--font-body`, `--font-heading`). Headline sizes mirror Newspack's `.entry-title` scale via `--newspack-theme-font-size-*`. Not all-caps except note-card eyebrow labels.
- Palette used so far: text/background slate `#404f54`, teal `#23685b` / light `#51a89a`, accent yellow `#f4c913` (highlights, note rule), accent red `#d64d4d` (emphasis), gray `#879599` (meta text), cream `#f4f0e6` (note card), border `#ccd8db`. Reddit avatar gradients vary per commenter.
- Byline markup copies the live site's `.entry-subhead` / `.entry-meta` classes so it matches theme styles. Photo credit sits directly under it.
- Cards: white, `box-shadow: 0 20px 50px rgba(0,0,0,.4)`, body copy `clamp(18px,1.9vw,21px)` / 1.75. Desktop cards are capped near `46vw` so the photo stays visible; mobile widens to 82–90vw.
- Accessibility: photo layers use `role="img"` + `aria-label`; static-fallback photos are `aria-hidden`; the real title stays in the DOM (visually hidden) for screen readers.

## Embedding on WordPress (pinned iframe)

- `senior-tips.html` is the child page and `wordpress-embed-snippet.html` is the parent snippet for a Custom HTML block. Prefix is `st-` (`#st-embed-wrap`, `#st-story-hook`). Same pattern as the "senior story" folder: the parent pins a full-screen iframe and sends `progress`; the child scrolls itself to match and reports its height as `trackHeight`.
- Modes are classes on `<html>`, set in the head script: `st-embedded`, `st-flat` (reduced motion, nothing pins), `st-px` (both: no `vh` sizing, the parent sizes the iframe to the content).
- Contents links and the tip arrows can't scroll the iframe; they send `jumpTo` and the parent scrolls. The tips script calls `window.stScrollTo` when it exists.
- Images are referenced by full Media Library URL (`.../uploads/2026/09/senior-tips-*.png`), so the HTML can live in any month folder; `CHILD_URL` in the snippet must match wherever the HTML actually is (currently `/2026/10/`). New images must be uploaded to WordPress before the page can show them.
- `_embed-test.html` is a local stand-in for the post (run `python3 -m http.server` and open it). Rebuild it if the snippet changes.
- Reader view: the snippet carries a visually hidden text copy of the whole story (`#st-text`, between the `st-text` markers), generated from `senior-tips.html` by `node build-reader-copy.js` (`npm install` once). Re-run it after any copy change and re-paste the snippet. The iframe is `aria-hidden` so screen readers read only the text copy.

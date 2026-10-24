# Logseq OG 0.10 compatibility fix

## Environment
- Logseq OG 1.0.0 (Electron 38.4.0), macOS
- Theme: Things - Dark, accent colour disabled for QA
- Test page: code block with a long line

## Problem
Every fenced code block hides the first two characters. `(defn` renders as
`:fn`, `(str` as `:r`, because the 31px CodeMirror gutter is visible while the
sizer is still shifted only 5px.

## Cause
`logseq-css-system` hides gutters only for `.cm-s-solarized`, but current
Logseq uses `cm-s-lsradix`. It also forces
`.CodeMirror-sizer { margin-left: 5px !important }`.

## Fix
- Hide the `cm-s-lsradix` gutter and set the sizer margin to 0.
- Vendor the remote CSS locally (`things-remote.css`) so the theme works offline
  and the fix is applied after the imported rules.

## Verification
Before/after: `docs/logseq-og-qa/things-code.png`.
The code starts with `(defn greet` after the fix.

## Upstream
Shared root cause: `rcvd/logseq-css-system`.

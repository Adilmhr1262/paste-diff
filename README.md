# paste-diff

A single-page, offline text comparison tool. Paste two versions of any text
(a contract clause, a README, an email draft, config files) and get an instant
side-by-side diff with word-level highlights. Everything runs in your browser;
nothing is uploaded anywhere.

## How to run

Open `index.html` in any browser. No server, no build step, no dependencies.

## Features

- Two input panes (Original / Changed) with live diffing as you type, plus a
  Compare button for large inputs
- Line-level diff using a self-written LCS algorithm; added, removed, and
  unchanged lines are colored
- Word-level highlighting inside changed lines, so a one-word edit does not
  flag the whole line
- Toggle options: ignore whitespace, ignore case, hide unchanged lines
  (hidden blocks expand on click)
- Summary stats bar: lines added, removed, and changed, plus a similarity
  percentage
- Unified-diff export: copy to clipboard or download as a `.diff` file
- Swap-panes button and a sample data loader for quick testing

## How the stats are counted

Each changed line pair counts as one "changed" line. Lines that only appear on
one side count as "added" or "removed". Similarity is
`unchanged lines / max(lines on either side)`, shown with one decimal.

## Limits

The line diff table is quadratic in the number of lines, so the comparison is
capped at about 16M table cells (roughly 64 MB). Before diffing, the common
prefix and suffix are trimmed without any table, so large files that differ
only in the middle still diff quickly. Past the cap, the app shows a message
instead of freezing the tab. Word-level highlighting is likewise capped per
changed line (about 4M token cells); very long lines render as plain changed
lines with no word marks.

## Verification

The diff engine was tested with 30 automated checks covering: identical input,
empty versus non-empty input, hand-counted line diffs, word-level highlighting
(one-word change flags only that word), the ignore-whitespace and ignore-case
toggles, unified-diff output format, the 16M-cell comparison guard,
prefix/suffix trimming on large inputs, the word-level cap on huge lines,
CRLF handling, and HTML escaping. A further 18 headless-Chromium checks
exercised the real page: sample loading, stats bar, word marks,
collapse/expand of unchanged lines, toggles, swap, export content, live
typing, empty inputs, and the too-large message, with zero console or page
errors.

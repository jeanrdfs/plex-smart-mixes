# Contributing

Recipes and artwork are both welcome.

## Adding a playlist recipe

Open an issue or PR with:

- the filter rows, exactly as they appear in the Plex editor
- whether it needs a limit and sort, and why
- roughly how many tracks it returned for you, and how big your library is

The last one matters. A filter that returns 200 tracks on a 40,000-track library
might return four on a small one.

## Adding artwork

Match the existing spec in the README so the set stays consistent — same lockup,
same margins, same two-line title structure. New colourways are more useful than
new layouts.

## What isn't a good fit

Filters that depend on fields Plex doesn't expose (duration, BPM, track number),
or that need a script or external tool to work. Everything here should be
buildable in the Plex web UI in under a minute.

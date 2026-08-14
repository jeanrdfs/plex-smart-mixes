# Plex Smart Mixes

Smart playlist recipes and cover art to make Plexamp feel like a streaming app.
27 filters, each with a poster, background and logo.

![Cover art preview](art/previews/posters.png)

---

## Why

Plex already has everything needed to build the shelves Spotify and Apple Music
put on their home screens. The filters are just buried, and the results come out
looking like a folder of files.

A few of these can't exist on a streaming service at all. **Critics Were Right**
finds acclaimed albums you own and have never once played. **The Ghost Shelf**
finds albums that have sat untouched for over a year. Neither is possible unless
the app knows what's on your shelf, not only what you streamed.

---

## Contents

- [Building a smart playlist](#building-a-smart-playlist)
- [Setting the description](#setting-the-description)
- [Setting the artwork](#setting-the-artwork)
- [Sorting and limits](#sorting-and-limits)
- [The playlists](#the-playlists)
  - [Streaming staples](#streaming-staples)
  - [Time machine](#time-machine)
  - [Mood and context](#mood-and-context)
  - [The ones streaming can't do](#the-ones-streaming-cant-do)
  - [Library hygiene](#library-hygiene)
- [Artwork specification](#artwork-specification)
- [Notes](#notes)

---

## Building a smart playlist

Once you've done one, the rest take about a minute each.

1. Open your **Music** library in Plex web
2. Set the type dropdown to **Tracks**
3. Click **Advanced Filters**
4. Build the filter rows from the recipes below
5. Set the sort from the column header — it's saved with the playlist, so do it
   before saving
6. Fill **LIMIT TO** only when the recipe asks for one
7. **Save As…** → name it → **Save as Smart Playlist**

**Sorting in Tracks view works differently than Albums view.** Albums view has a
sort dropdown. Tracks view doesn't, instead, click the column header itself
(Plays, Date Added, Rating, etc.) to sort by that field, click it again to flip
between ascending and descending. It's not a dropdown, but it's not broken either,
it's just a different control for the same thing.

### Match all vs Match any

Most recipes use **Match all**, where every row must be true. A few need a nested
**Match any** block, which is the `⋮` button beside the `+`. The mood playlists
use it to catch several mood tags at once.

### A note on Mood and Style fields

`Track Mood` and `Album Style` only support **is**, not **contains**, in the
current Plex Web filter editor. If a recipe below shows `contains`, build it as
one **is** row per value inside a **Match any** group instead. For example:

```
Match any
Track Mood   is   melancholy
Track Mood   is   reflective
Track Mood   is   sombre
Track Mood   is   dreamy
```

rather than a single `Track Mood contains melancholy` row, which isn't available
as an option.

### A note on Album Critic Rating

This field shows as a five-star click control in Plex Web, not a numeric input.
There's no way to type a decimal like `7.9`. Use the star equivalent instead,
roughly `rating ÷ 2`, so 7.9/10 becomes **4 stars**. It's an approximation, not
an exact conversion.

It's also not currently sortable directly. **Popularity** is a reasonable
stand-in if you want a ranked order, it's independent of your own play count, so
it won't just re-surface whatever you've already played the most.

---

## Setting the description

Descriptions show under the playlist title in Plexamp and in the Plex apps. They
have to be set from Plex web — Plexamp reads them but can't edit them.

1. Open the playlist in **Plex web**
2. Click the **⋯** menu → **Edit**
3. Fill in the **Summary** field
4. **Save Changes**

Each playlist below has a description written for this field. Copy it as-is or
rewrite it in your own voice.

---

## Setting the artwork

Plex gives every playlist three separate artwork slots, and this repo has a file
for each one.

| Slot | What it does | Folder |
|---|---|---|
| **Poster** | The square cover in every grid and list | `art/posters/` |
| **Background** | Fills the playlist page behind the title | `art/backgrounds/` |
| **Logo** | Replaces the plain-text title on the playlist page | `art/logos/` |

**To set them:**

1. Open the playlist in **Plex web**
2. Click the **⋯** menu → **Edit**
3. Across the top of the edit dialog you'll see tabs for **Poster**, **Background**
   and **Logo**
4. Pick a tab → **Upload** → choose the matching file from this repo
5. Repeat for the other two tabs, then **Save Changes**

Every playlist below lists its three filenames. They all share the same numbered
prefix, so `01-new-to-you.png`, `01-new-to-you-bg.png` and `01-new-to-you-logo.png`
belong together.

**Don't skip the logo.** It's the slot people ignore, and it's the one that
carries the look — without it Plex renders your playlist name in its own font
over the background, which undoes most of the effect.

---

## Sorting and limits

This part trips people up, so it's worth being precise.

**With a LIMIT, the sort decides what's in the playlist.** `Limit 40` sorted by
`Track Plays (desc)` means "the 40 most-played." Change the sort and you get 40
completely different tracks. That's the definition of the playlist, not the
running order.

**Without a LIMIT, the sort is only the stored order** — and Plexamp's shuffle
overrides it the moment you press play. So it doesn't matter.

That's why only 10 of the 27 below specify a sort at all. The rest are unlimited:
press shuffle and let them run.

**There is no Random sort in the smart playlist editor.** It exists while
browsing a library but doesn't survive Save As. You don't need it — shuffle
re-rolls on every play anyway.

Each playlist is tagged:

- **Open** — no limit, shuffle at playback
- **Ordered** — no limit, but play it top-down
- **Selected** — the limit and sort define the contents

---

## The playlists

### Streaming staples

#### 1. New To You — *Mix* · Open

> Music you own but have never played, by the artists you've been listening to
> lately. Your library's blind spots, surfaced.

```
Match all
Track Plays          is            0
Artist Last Played   in the last   90 days
```

Discover Weekly's logic, pointed at your own library. Don't add a limit — every
sort option hands you the same handful of artists forever.

`01-new-to-you.png` · `01-new-to-you-bg.png` · `01-new-to-you-logo.png`

---

#### 2. On Repeat — *Mix* · Selected

> The songs you can't stop going back to this month. Updated as your habits
> shift.

```
Match all
Track Plays          is greater than   4
Track Last Played    in the last       30 days

Limit 40 · Sort: Track Plays (desc)
```

`02-on-repeat.png` · `02-on-repeat-bg.png` · `02-on-repeat-logo.png`

---

#### 3. Repeat Rewind — *Mix* · Selected

> Songs you played into the ground and then walked away from. Enough time has
> passed to miss them.

```
Match all
Track Plays          is greater than   9
Track Last Played    not in the last   120 days

Limit 50 · Sort: Track Plays (desc)
```

`03-repeat-rewind.png` · `03-repeat-rewind-bg.png` · `03-repeat-rewind-logo.png`

---

#### 4. Just Landed — *Mix* · Ordered

> Everything that arrived in the library this month, newest first. The first
> place to look after an import.

```
Match all
Track Added At       in the last   30 days

Sort: Track Added At (desc)
```

Don't shuffle this one. You read it top-down to see what just showed up.

`04-just-landed.png` · `04-just-landed-bg.png` · `04-just-landed-logo.png`

---

#### 5. Loved — *Smart Mix* · Ordered

> Every track you've rated four stars or higher. The permanent collection.

```
Match all
Track Rating         is greater than   3

Sort: Track Last Rated (desc)
```

`05-loved.png` · `05-loved-bg.png` · `05-loved-logo.png`

---

### Time machine

#### 6. The 80s — *Mix* · Open

> Big rooms, bigger reverb, and drum machines that refused to apologise.
> Everything in the library from 1980 to 1989.

```
Match all
Album Decade         is    1980
```

`06-the-80s.png` · `06-the-80s-bg.png` · `06-the-80s-logo.png`

---

#### 7. The 90s — *Mix* · Open

> The decade the guitars got heavier and the CDs got longer. Ten years of the
> library, shuffled.

```
Match all
Album Decade         is    1990
```

`07-the-90s.png` · `07-the-90s-bg.png` · `07-the-90s-logo.png`

---

#### 8. The 2000s — *Mix* · Open

> Burned discs, ripped MP3s, and the last decade before an algorithm started
> deciding what came next.

```
Match all
Album Decade         is    2000
```

`08-the-2000s.png` · `08-the-2000s-bg.png` · `08-the-2000s-logo.png`

---

#### 9. Your Year — *In Music* · Selected

> The hundred tracks you actually played most over the last twelve months.
> Rebuild it every January.

```
Match all
Track Last Played    in the last       365 days
Track Plays          is greater than   2

Limit 100 · Sort: Track Plays (desc)
```

A countdown, so the sort does double duty — it picks the hundred and ranks them.

`09-your-year-in-music.png` · `09-your-year-in-music-bg.png` · `09-your-year-in-music-logo.png`

---

### Mood and context

These lean on Plex's Mood and Style metadata, which comes from the metadata agent
and varies with how well-matched your library is. Check a few tracks before
trusting it — if Track Mood is thin, Album Mood and Album Style are usually
better populated.

#### 10. Late Night — *Mix* · Open

> For the hours after midnight, when the volume comes down and everything sounds
> further away.

```
Match any
Track Mood   is   melancholy
Track Mood   is   reflective
Track Mood   is   sombre
Track Mood   is   dreamy
```

`10-late-night.png` · `10-late-night-bg.png` · `10-late-night-logo.png`

---

#### 11. Upbeat — *Smart Mix* · Open

> Anything in the library with a pulse. Built for kitchens, commutes, and getting
> out the door.

```
Match any
Track Mood   is   energetic
Track Mood   is   rowdy
Track Mood   is   exuberant
```

`11-upbeat.png` · `11-upbeat-bg.png` · `11-upbeat-logo.png`

---

#### 12. Deep Focus — *Mix* · Open

> Instrumental and atmospheric music that stays out of the way. Nothing here asks
> you to sing along.

```
Match all
  Match any
  Album Style   is   ambient
  Album Style   is   post-rock
  Album Style   is   instrumental
Track Plays    is greater than   0
```

`12-deep-focus.png` · `12-deep-focus-bg.png` · `12-deep-focus-logo.png`

---

#### 13. Sunday Kitchen — *Mix* · Open

> Warm, familiar, mid-tempo. Music to cook to with someone else in the room.

```
Match all
Track Rating   is greater than   2
  Match any
  Track Mood   is   warm
  Track Mood   is   laid-back
  Track Mood   is   gentle
```

`13-sunday-kitchen.png` · `13-sunday-kitchen-bg.png` · `13-sunday-kitchen-logo.png`

---

### The ones streaming can't do

#### 14. Critics Were Right — *Essentials* · Selected

> Acclaimed albums you own with at least one track you've never played. No
> streaming service can build this list, because none of them know what's on
> your shelf.

```
Match all
Album Critic Rating  is greater than   ★★★★  (4 stars)
Track Plays          is                0

Limit 50 · Sort: Popularity (desc)
```

`Album Critic Rating` is a five-star click control in Plex Web, not a numeric
field, there's no way to type `7.9` directly. Four stars is roughly the
equivalent. It's not sortable on its own either, Popularity is a reasonable
stand-in since it's independent of your own play count.

This uses `Track Plays`, not `Album Plays`, on purpose. An earlier version
filtered on `Album Plays is 0`, meaning the whole album had to be completely
untouched, but that turned out to be a much stricter bar than it sounds:
sample one track off a well-reviewed album years ago and it's disqualified
forever. `Track Plays is 0` catches acclaimed albums where you've heard
something but not most of it, which returns useful results for a lot more
libraries.

Want the stricter version back, whole album untouched, not just one track?
Swap the second row for `Album Plays is 0`. It can return very few results,
or none, depending on how much of your acclaimed music you've already sampled.

`14-critics-were-right.png` · `14-critics-were-right-bg.png` · `14-critics-were-right-logo.png`

---

#### 15. Second Chances — *Mix* · Open

> Tracks you skipped past months ago. Streaming treats a skip as a permanent
> verdict — this gives them another hearing.

```
Match all
Track Skips          is greater than   1
Track Last Skipped   not in the last   180 days
```

`15-second-chances.png` · `15-second-chances-bg.png` · `15-second-chances-logo.png`

---

#### 16. The Ghost Shelf — *Essentials* · Selected

> Albums that have sat in the library for over a year, completely untouched. The
> guilt list, oldest first.

```
Match all
Date Album Added     not in the last   365 days
Album Plays          is                0

Limit 50 · Sort: Date Album Added (asc)
```

Longest-neglected first. The sort is the guilt.

**If this comes back empty**, that's not necessarily a bug, it can genuinely
mean you've listened to at least one track off every album in your library
that's over a year old. `Album Plays is 0` is a strict filter, unlike the star
rating field above, it works correctly here, an empty result is a real result.

`16-the-ghost-shelf.png` · `16-the-ghost-shelf-bg.png` · `16-the-ghost-shelf-logo.png`

---

#### 17. Almost Loved — *Smart Mix* · Open

> Everything stuck at three stars. Songs you liked enough to rate and never
> resolved — promote them or let them go.

```
Match all
Track Rating         is        3
```

`17-almost-loved.png` · `17-almost-loved-bg.png` · `17-almost-loved-logo.png`

---

#### 18. Passport — *Mix* · Open

> Everything from outside the usual three countries. Your library's world
> service.

```
Match all
Artist Country       is not    United States
Artist Country       is not    United Kingdom
Artist Country       is not    Canada
```

`18-passport.png` · `18-passport-bg.png` · `18-passport-logo.png`

---

#### 19. Saudade — *Mix* · Open

> Brazilian and Portuguese artists — a word that doesn't translate, for music
> that doesn't need to.

```
Match any
Artist Country       is        Brazil
Artist Country       is        Portugal
```

Swap in whichever countries mean something to you.

`19-saudade.png` · `19-saudade-bg.png` · `19-saudade-logo.png`

---

#### 20. Homegrown — *Mix* · Open

> Canadian artists only. Deeper than you'd expect once you actually filter for
> it.

```
Match all
Artist Country       is        Canada
```

`20-homegrown.png` · `20-homegrown-bg.png` · `20-homegrown-logo.png`

---

#### 21. House Style — *Essentials* · Ordered

> One label, in release order. Pick a house you trust — 4AD, Blue Note, Sub Pop,
> Stones Throw, Warp — and let their A&R do the curating.

```
Match all
Record Label         contains   4AD

Sort: Year (asc)
```

The one playlist you should never shuffle. Hearing a label evolve
chronologically is the entire point.

`21-house-style.png` · `21-house-style-bg.png` · `21-house-style-logo.png`

---

#### 22. Live Wire — *Mix* · Open

> Concert recordings only. Longer, looser, and with a room full of people in the
> mix.

```
Match all
Album Type           is        Live
```

`22-live-wire.png` · `22-live-wire-bg.png` · `22-live-wire-logo.png`

---

#### 23. B-Sides — *Mix* · Open

> Singles and EPs — the tracks that never had an album to hide behind.

```
Match any
Album Format        is        Single
Album Format        is        EP
```

`23-b-sides.png` · `23-b-sides-bg.png` · `23-b-sides-logo.png`

---

#### 24. Audiophile — *Hour* · Open

> Lossless files, and only music you've already rated. For the good speakers and
> an evening with nothing else to do.

```
Match all
Audio Codec          is        flac
Track Rating         is greater than   3
```

`24-audiophile-hour.png` · `24-audiophile-hour-bg.png` · `24-audiophile-hour-logo.png`

---

#### 25. From The Crate — *Mix* · Open

> Vinyl rips, bootlegs, and anything else living in its own corner of the drive.
> Turns a folder into a station.

```
Match all
Folder Location      contains   /Vinyl
```

`25-from-the-crate.png` · `25-from-the-crate-bg.png` · `25-from-the-crate-logo.png`

---

### Library hygiene

Not for listening. Keep these off your Plexamp home screen.

#### 26. Needs Matching — *Library* · Ordered

> Tracks Plex couldn't identify. Fix these and half your mood filters start
> working properly.

```
Match all
Unmatched            is        true

Sort: Album Artist (asc)
```

Grouped by artist so you fix a whole catalogue in one pass. Worth doing early —
the mood playlists above depend on good metadata.

`26-needs-matching.png` · `26-needs-matching-bg.png` · `26-needs-matching-logo.png`

---

#### 27. Upgrade Queue — *Library* · Ordered

> Lossy files you play often enough to be worth re-ripping, most-played first.

```
Match all
Audio Codec          is        mp3
Track Plays          is greater than   3

Sort: Track Plays (desc)
```

Highest impact at the top. Work down and stop whenever you get bored.

`27-upgrade-queue.png` · `27-upgrade-queue-bg.png` · `27-upgrade-queue-logo.png`

---

## Artwork specification

```
art/
├── posters/       1000 × 1000 PNG   the cover in every grid and list
├── backgrounds/   1920 × 1080 PNG   behind the playlist page
├── logos/         transparent PNG   replaces the plain-text title
└── previews/      contact sheets
```

Everything is full-quality PNG, 24-bit, no lossy compression anywhere. Logos
carry an alpha channel.

Every poster uses one lockup, which is what makes the set read as a family:

| Element | Spec |
|---|---|
| Margin | 78 px |
| Plexamp mark | 34 px tall at y120, wordmark 38 px |
| Title line 1 | 98 px, weight 700, tracking −3.5, accent colour |
| Title line 2 | 98 px, weight 400, tracking −3, white |
| Finish | fine grain over a gradient or flat field |

Typeface is [Outfit](https://fonts.google.com/specimen/Outfit), SIL Open Font
License.

Second lines repeat across groups on purpose — *Mix*, *Smart Mix*, *Essentials*,
*Library*. That repetition is what makes 27 covers look like one system instead
of 27 one-offs.

---

## Notes

**Check your counts before committing.** These were built against a library of
around 2,000 tracks. The narrow filters — House Style, B-Sides, Audiophile Hour,
From The Crate — can come back nearly empty depending on what you own.

**Fields Plex gives you that streaming doesn't:** `Album Critic Rating`,
`Track Skips`, `Artist Country`, `Record Label`, `Album Type`, `Folder Location`.

**Fields that don't exist:** Duration, BPM, Track Number. So no "songs over seven
minutes", no album openers, no tempo-based workout lists — not without tagging
files yourself first.

**Start with eight or ten.** A crowded shelf is the problem you're trying to
solve.

**The same song can show up more than once** if it exists on multiple entries
in your library, an original album, a greatest hits, a soundtrack, a box set.
Plex doesn't have a native way to deduplicate by title across releases, so
count-based playlists like On Repeat or Your Year can show what looks like the
same track several times. No clean fix for this without manually tagging
duplicates yourself.

---

## License

Recipes and artwork are released under [CC0](LICENSE) — use, modify and
redistribute freely, no attribution needed.

Outfit is licensed separately under the SIL OFL. The Plexamp mark belongs to Plex
Inc. and is included for personal use; this project isn't affiliated with or
endorsed by Plex.

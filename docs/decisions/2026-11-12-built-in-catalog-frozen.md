# The built-in catalog is frozen, and games.json replaces it whole

## The problem

The page carries `BUILT_IN` in `index.html`, a full second copy of the
catalog, for when `games.json` cannot be fetched: a preview, or the page
opened from disk. Its comment said it was "kept identical to the
published file", and #5 asked for that to be held, by a check or by
removing the copy.

The user, on 12 November 2026: "The default games in the index.html, we
don't touch that anymore. Let's just leave it as it is. And I think this
part should only be displayed and shown if there is no games JSON. So
moving forward, we will just use the games JSON in order to override the
defaults and then show the games that we are interested in."

Measured on the code as it is: `loadBase` already returns either
`games.json` or `BUILT_IN`, never a merge of the two. The local layer is
worked out against whichever of them loaded. On main the two catalogs
hold the same 12 games, and nothing checks that. Nothing else in the
code reads `BUILT_IN`.

## Five simulations

### 1. Who may know about whom: a check holds the copy to games.json

A CI check compares `BUILT_IN` with `games.json` and fails when they
differ. Every game is then added twice, in two files, in one pull
request. There is no CI yet, #7, so the check would come first. It keeps
the copy and makes it costlier to change, the opposite of what was
asked.

### 2. What changes together: remove the copy

`BUILT_IN` goes, and without `games.json` the page shows an empty
catalog or an error. One home for the catalog. A preview and the page
opened from disk show nothing, which the user relies on today when
trying a pull request. It also edits `BUILT_IN`, which the user asked to
leave alone.

### 3. Where state lives: freeze the copy, games.json replaces it whole

`BUILT_IN` stays as it is and is never changed again. It is no longer a
copy of the published catalog but a catalog of its own, shown only when
there is no `games.json`. The code already behaves this way. Only the
comment, the architecture file and the expectation change. The two
catalogs drift from today, on purpose.

### 4. What runs together: build BUILT_IN from games.json

A build step writes `games.json` into the page. One source, no drift. It
adds the build step the project does not have, for the one case of a
page opened without its file, and it changes `BUILT_IN` on every
release.

### 5. What a test has to build: inline games.json, drop the fetch

The page carries the catalog and never fetches it. Committing an export
then means editing `index.html`, a 100 KB file, for every game. It
undoes the split between the page and its data that the server move,
#4, builds on.

## What converges

Three of the five keep a catalog in the page for when the file is
missing (1, 3, 4). Two of them make every new game touch `index.html`
(1, 4), and 5 makes it touch nothing else. Only 3 keeps new games in
`games.json` alone and leaves `BUILT_IN` untouched, which is what was
asked. Only 2 removes the second home, at the cost of an empty preview.

## The decision

Candidate 3. Layers, top down, unchanged: the page, which loads one base
catalog, `games.json` or else `BUILT_IN`, lays the local layer on top,
and draws it.

## What follows

- Costs: the catalog without `games.json` grows old. A preview without
  the file shows the 12 games it holds today.
- Buys: one place to add a game, `games.json`, and no check to keep.
  Closes #5.
- Risk: an export from a page without `games.json` holds only the
  built-in games, and committing it would remove every game added
  since. Export now asks before it does that.
- Not simulated: fetching the published catalog from the live site when
  the file next to the page is missing. It would make a preview show
  the current games, at the cost of the page reaching another origin.
- What would show it wrong: someone relying on the built-in catalog
  being current, or a local change that was made against `BUILT_IN`
  and is then shown against `games.json`. Both were possible before
  too.
- Gone for good when the catalog moves to a server.

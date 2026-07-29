# TVTimeClone

A TV Time–style movie/show tracker: browse trending content, save it to lists, track what
you've watched (per-episode for shows), rate things, and mark favorites. A traditional
server-backed multi-user website — sign up, log in, your data lives in a shared database.

> Looking for the offline Android app instead? See the
> [`mobile-local-app`](https://github.com/EasyBuster200/Movie-and-Show-tracker-Website/tree/mobile-local-app)
> branch — a from-scratch rebuild of this same idea with local per-device profiles and no
> server at all.

## Features

- **Accounts** — sign up / log in, session-based (no JWT).
- **Browse & search** — trending, popular, and recommended movies/shows via TMDB, plus a live
  search bar.
- **Lists & Bookmarks** — save anything to custom lists, with a dedicated Bookmarks page for
  quick-saves.
- **Per-episode watch tracking** — movies are a simple watched/unwatched toggle; shows track
  every episode individually, with a "catch up" prompt that bulk-marks earlier episodes when you
  jump ahead.
- **Ratings & favorites** — a 5-star (half-increment) rating widget on the detail page, and a
  heart toggle for favorites.
- **Keep Watching / Airing Soon** — surfaces shows with new episodes since you last watched, and
  upcoming air dates for anything you're tracking.
- **Recommendations** — seeded from your own most-recently-watched/listed items, ranked against
  TMDB's recommendation graph.

## Getting started

### Prerequisites

- [Node.js](https://nodejs.org/) (`better-sqlite3` needs a native build step during install)
- A free [TMDB API access token](https://www.themoviedb.org/settings/api)

### Setup

```bash
npm install
```

Create a `.env` file in the project root (gitignored, not included in the repo):

```bash
TMDB_ACCESS_TOKEN=your-tmdb-api-read-access-token
SESSION_SECRET=any-random-string
PORT=3000   # optional, defaults to 3000
```

Then run it:

```bash
npm start
```

The whole app — API and static frontend — is served from one Express process on `PORT`. Open
`http://localhost:<PORT>` and sign up to create an account.

There's no build step or bundler — the frontend is plain HTML/CSS/JS loaded directly via
`<script>` tags, served as static files by the same Express process.

## Project structure

```
server/          Express app: routes, SQLite access (better-sqlite3), session store, TMDB proxy
public/          the entire frontend (HTML/CSS/JS) — served statically, one page per route
docs/icons/      source Material Symbols SVGs, copy-pasted as reference for inline <svg> icons
```

SQLite is the only datastore (`server/schema.sql`, applied idempotently on every boot — no
migration system) — no separate database server to run. `server/tmdbClient.js` is the only
thing that ever talks to TMDB directly, injecting the bearer token server-side so it never
reaches the browser.

See [CLAUDE.md](CLAUDE.md) for a much deeper architectural writeup (data flow, auth gating,
the standard item contract used across pages, and known rough edges).

## Attribution

This product uses the TMDB API but is not endorsed, certified, or otherwise approved by TMDB.

## License

ISC

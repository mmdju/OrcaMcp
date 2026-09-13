# Orca MCP - Movie/TV intelligence for AI agents

A public MCP server that gives AI agents real movie and TV knowledge: search, ratings across sources, IMDb Top 250 and Letterboxd Top 500 ranks, streaming availability, episodes, artwork. Read-only, no key needed.

**Live endpoint:** `https://orca-mcp.mmdju.workers.dev/mcp` (Streamable HTTP, stateless)

## Connect in 30 seconds

Any MCP client, one URL. OpenCode (`opencode.jsonc`):

```json
{
  "mcp": {
    "orca-mcp": {
      "type": "remote",
      "url": "https://orca-mcp.mmdju.workers.dev/mcp",
      "enabled": true
    }
  }
}
```

Claude Desktop / Cursor (`mcp.json` style):

```json
{
  "mcpServers": {
    "orca-mcp": { "url": "https://orca-mcp.mmdju.workers.dev/mcp" }
  }
}
```

Then just talk: "what rank is Godfather on IMDb and Letterboxd?", "a good sci-fi movie after 2020 rated above 8", "who directed Dune and what else did they make?", "where can I watch Breaking Bad?".

## 21 tools

| Tool | What it answers |
|---|---|
| `movies_search` | Search films, shows, people by title (EN/FA) |
| `movies_details` | Plot, genres, all ratings, cast, director, trailer, awards, posters |
| `movies_discover` | Filter by genre, keyword, studio, year, min rating, min votes, sort |
| `movies_trending` | What is hot now (day/week) |
| `movies_where_to_watch` | Netflix, Prime, Disney+ etc. per country |
| `movies_compare_lists` | IMDb Top 250 rank vs Letterboxd Top 500 rank |
| `movies_ratings` | TMDB + IMDb + Rotten Tomatoes + Metacritic, all ids resolved |
| `movies_collection` | Franchise watch order with runtime each, total runtime, per-part streaming |
| `movies_similar` | "Like Whiplash, what next?" |
| `movies_person` | Bio, photo, top acting and directing credits |
| `movies_artwork` | Poster, backdrop, logo, banner |
| `tv_episodes` | Season/episode list with airdates and summaries |
| `movies_reviews` | What people say: author reviews with ratings |
| `movies_videos` | Trailers, teasers, clips with YouTube links |
| `tv_season` | One full season: ratings, runtimes, stills |
| `tv_episode` | One episode: director, writer, guest stars |
| `find_by_external_id` | IMDb id (tt...) to TMDB id |
| `movies_keywords` | Theme words to keyword ids ("zombie", "heist") |
| `movies_companies` | Studio names to company ids ("A24", "Pixar") |
| `person_watch_path` | Where to start with an actor/director |
| `release_calendar` | Upcoming movies / on-air shows |

Notes for agent builders:

- Accepts titles, TMDB ids and IMDb ids (`tt...`) interchangeably - ids resolve internally.
- `movies_discover` defaults to `min_votes: 300` because averages with few voters are noise. Items below that carry `low_votes: true` - warn the user instead of presenting the score as fact.
- New releases have few votes by nature: use `movies_trending` or `discover` with `sort: newest` and low `min_votes`.
- Results are capped (default 10, max 50) to protect agent context.

## Data sources

- TMDB (metadata, trending, streaming) - one shared server key, centrally throttled and cached
- IMDb ratings snapshot (offline dataset, refreshed periodically)
- Rotten Tomatoes + Metacritic + awards via OMDb (when configured)
- IMDb Top 250 and Letterboxd Top 500 via our own public APIs
- TVMaze (episodes), Fanart.tv (artwork), Cinemeta (fallback)

## Attribution

This product uses the TMDB API but is not endorsed or certified by TMDB. See https://www.themoviedb.org. TV episode data by TVMaze (CC BY-SA 4.0). Watch-provider data by JustWatch, via TMDB. IMDb rating data: Information courtesy of IMDb (https://www.imdb.com). Used with permission. Artwork by Fanart.tv contributors.

## Status

Free public service on Cloudflare Workers. Fair use applies - if you hammer it, you will be rate-limited.

## License

Showcase repository (docs only, no source published) - see [LICENSE](LICENSE). Security notes in [SECURITY.md](SECURITY.md).

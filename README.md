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

Then just talk: "what rank is Godfather on IMDb and Letterboxd?", "a good action movie after 2022 rated above 7", "in what order do I watch Godfather?", "where can I stream Dune?".

## 12 tools

| Tool | What it answers |
|---|---|
| `movies_search` | Search films, shows, people by title |
| `movies_details` | Plot, genres, cast, director, trailer, posters |
| `movies_discover` | Filter by genre, year, min rating, min votes, sort |
| `movies_trending` | What is hot now (day/week) |
| `movies_where_to_watch` | Netflix, Prime, Disney+ etc. per country |
| `movies_compare_lists` | IMDb Top 250 rank vs Letterboxd Top 500 rank |
| `movies_ratings` | TMDB + IMDb scores and votes, all ids resolved |
| `movies_collection` | Franchise watch order by release date |
| `movies_similar` | "Like Whiplash, what next?" |
| `movies_person` | Bio, photo, top acting and directing credits |
| `movies_artwork` | Poster, backdrop, logo, banner |
| `tv_episodes` | Season/episode list with airdates and summaries |

Notes for agent builders:

- Accepts titles, TMDB ids and IMDb ids (`tt...`) interchangeably - ids resolve internally.
- `movies_discover` defaults to `min_votes: 300` because averages with few voters are noise. Items below that carry `low_votes: true` - warn the user instead of presenting the score as fact.
- New releases have few votes by nature: use `movies_trending` or `discover` with `sort: newest` and low `min_votes`.
- Results are capped (default 10, max 50) to protect agent context.

## Data sources

- TMDB (metadata, trending, streaming) - one shared server key, centrally throttled and cached
- IMDb ratings snapshot (offline dataset, refreshed periodically)
- IMDb Top 250 and Letterboxd Top 500 via our own public APIs
- TVMaze (episodes), Fanart.tv (artwork), Cinemeta (fallback)

## Attribution

This product uses the TMDB API but is not endorsed or certified by TMDB. See https://www.themoviedb.org. TV episode data by TVMaze (CC BY-SA 4.0). Watch-provider data by JustWatch, via TMDB. IMDb rating data: Information courtesy of IMDb (https://www.imdb.com). Used with permission. Artwork by Fanart.tv contributors.

## Status

Free public service on Cloudflare Workers. Fair use applies - if you hammer it, you will be rate-limited.

## License

MIT - see [LICENSE](LICENSE). Security notes in [SECURITY.md](SECURITY.md).

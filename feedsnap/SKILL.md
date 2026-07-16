---
name: feedsnap
description: Use the feedsnap CLI to fetch, filter, and summarize RSS and Atom feeds. Activate when asked to read or summarize a blog, news site, or podcast feed; monitor a URL for new articles; fetch entries published since a given date; process multiple feeds from an OPML file; or produce an RSS-to-markdown digest. Requires Python 3.10+ and pip.
license: MIT
metadata:
  author: rook-builds
  version: "0.6.0"
  homepage: https://github.com/rook-builds/feedsnap
  pypi: https://pypi.org/project/feedsnap/
compatibility: Requires Python 3.10+ and pip (or uv). Network access required to fetch feeds.
---

# feedsnap

feedsnap is a CLI tool that converts RSS and Atom feeds into clean markdown, structured JSON, or table output — purpose-built for use in agent pipelines.

## Install

Check if feedsnap is already installed:

```bash
feedsnap --version
```

If not installed:

```bash
pip install feedsnap
# or, with uv:
uv tool install feedsnap
```

## Core Usage

```bash
# Markdown digest — 8 latest entries (default)
feedsnap <feed-url>

# Structured JSON envelope — best for agent pipelines
feedsnap <feed-url> --output json

# Table view (TITLE | DATE | URL columns)
feedsnap <feed-url> --output table
```

## Key Options

| Option | Short | Description |
|--------|-------|-------------|
| `--limit N` | `-n N` | Max entries to return (default: 8) |
| `--output MODE` | `-o MODE` | `text` (markdown), `json` (ACLI envelope), or `table` |
| `--since DATE` | | Only entries published on or after DATE. Accepts `YYYY-MM-DD` or `Nd` (e.g. `2d` = two days ago). |
| `--dedup` | | Skip entries already seen in a previous run. Enables stateful feed monitoring across sessions. |
| `--seen-db PATH` | | Custom SQLite DB for seen-entry tracking (implies `--dedup`). |
| `--opml FILE` | | Process multiple feeds from an OPML subscriptions file. |
| `--title` | | Include feed title as an H1 header. |

## Common Patterns

### Summarize the latest entries from a feed

```bash
feedsnap https://hnrss.org/frontpage --limit 5
```

### Get only articles from the last 48 hours

```bash
feedsnap https://hnrss.org/frontpage --since 2d
```

### Monitor a feed for new content across agent sessions

```bash
# First run: fetches entries, marks them seen
feedsnap https://example.com/feed.xml --dedup

# Next run: only NEW entries appear (previously seen entries skipped)
feedsnap https://example.com/feed.xml --dedup
```

### Process multiple feeds from an OPML file

```bash
feedsnap --opml subscriptions.opml --since 7d --output table
```

### Structured JSON for pipelines

```bash
feedsnap https://example.com/feed.xml --output json
```

Output format (ACLI envelope):

```json
{
  "ok": true,
  "command": "feedsnap",
  "version": "0.6.0",
  "duration_ms": 187,
  "data": {
    "feed": {
      "title": "Example Blog",
      "url": "https://example.com/feed.xml"
    },
    "entries": [
      {
        "title": "Article Title",
        "url": "https://example.com/article",
        "published": "2026-07-16",
        "summary": "First 300 chars of the article summary..."
      }
    ]
  }
}
```

## Self-Documentation

feedsnap can describe itself to agents at runtime:

```bash
# Full command tree as JSON (ACLI v0.1.0 spec)
feedsnap introspect

# Regenerate this SKILL.md from the installed version
feedsnap skill > SKILL.md
```

## Exit Codes

| Code | Meaning |
|------|---------|
| 0 | Success |
| 1 | Error (network failure, invalid URL, parse error, bad arguments) |

## Notes

- Supports RSS 2.0 and Atom 1.0 feeds
- `--dedup` stores seen entry URLs in `~/.feedsnap/seen.db` (SQLite, created automatically on first use)
- In OPML mode, failed individual feeds are skipped gracefully; `--limit` applies per-feed
- `--output json` is the recommended format for agent pipelines
- Source: https://github.com/rook-builds/feedsnap
- PyPI: `pip install feedsnap`

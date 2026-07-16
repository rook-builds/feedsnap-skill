# feedsnap-skill

[![skills.sh](https://skills.sh/b/rook-builds/feedsnap-skill)](https://skills.sh/rook-builds/feedsnap-skill)

> Agent Skills for [feedsnap](https://github.com/rook-builds/feedsnap) — give any AI agent the ability to read, filter, and summarize RSS and Atom feeds.

## Install

```bash
npx skills add rook-builds/feedsnap-skill
```

Or target a specific agent:

```bash
npx skills add rook-builds/feedsnap-skill -a cursor
npx skills add rook-builds/feedsnap-skill -a claude-code
```

## What this skill does

Once installed, your agent can:

- **Fetch feeds** — "Summarize the latest 5 posts from Hacker News"
- **Filter by date** — "Get articles from lobste.rs from the last 2 days"
- **Monitor for new content** — "Check my RSS feed for anything I haven't seen yet"
- **Process OPML files** — "Read all my subscriptions from feeds.opml"
- **Output structured JSON** — for chaining into other pipeline steps

The skill teaches the agent to install and invoke `feedsnap`, a CLI tool built specifically for agent pipelines. It supports markdown, JSON (ACLI envelope), and table output modes.

## Skills included

| Skill | Description |
|-------|-------------|
| `feedsnap` | Read, filter, and summarize RSS/Atom feeds |

## Manual install

The skill is a standard [Agent Skills](https://agentskills.io) directory. You can also install it manually:

1. Copy the `feedsnap/` directory to your agent's skills folder
2. Or run `feedsnap skill > SKILL.md` to regenerate it from your installed feedsnap version

## Updating

```bash
npx skills update rook-builds/feedsnap-skill
```

The skill version tracks feedsnap CLI releases. Check [github.com/rook-builds/feedsnap](https://github.com/rook-builds/feedsnap) for the changelog.

## About feedsnap

feedsnap is a Python CLI tool that turns RSS and Atom feeds into clean digests. Built by [Rook](https://github.com/rook-builds) — an AI agent that reads RSS feeds every session and got tired of writing the pattern by hand.

```bash
pip install feedsnap
feedsnap https://lobste.rs/rss
```

## License

MIT

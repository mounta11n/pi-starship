# pi-starship

A [pi](https://github.com/badlogic/pi-mono) extension that replaces the default footer with your [Starship](https://starship.rs) prompt — so colours, icons, and segments match your terminal exactly — and adds pi-specific context on the right: model, token counts, cost, and thinking level.

```
your-project on feature/your-feature [!?] via 🐍 v3.11.15 (venv) PR #42        anthropic → Claude Haiku 4.5 ◆ medium ↑12k ↓4.2k $0.043
```

## Features

- **Left side** — delegates to `starship prompt`, so every segment, colour, and icon comes directly from your `~/.config/starship.toml`.
  - Automatically shows git branch, status (`!` modified, `?` untracked, `⇣` behind, `⇡` ahead), language versions, virtualenvs, cloud contexts — whatever starship shows in your shell
  - Appends a clickable **PR #N** link (OSC-8 hyperlink) when there is an open GitHub PR for the current branch
- **Right side** — pi-specific info starship can't know about:
  - `provider → Model Name` (e.g. `anthropic → Claude Haiku 4.5`)
  - `◆ thinking-level` when extended thinking is active
  - `↑` input tokens · `↓` output tokens · `$` cost for the session

## Prerequisites

| Tool                                   | Purpose                         |
| -------------------------------------- | ------------------------------- |
| [Starship](https://starship.rs)        | Left side rendering             |
| [Nerd Font](https://www.nerdfonts.com) | Branch icon () and other glyphs |
| [gh CLI](https://cli.github.com)       | PR number + URL lookup          |

`gh` and `starship` must be in your `PATH`. The extension degrades gracefully when either is missing — the PR segment simply won't appear, or the left side falls back to empty.

## Installation

```bash
pi install npm:pi-starship
```

Or from source:

```bash
pi install git:github.com/rajivm1991/pi-starship
```

## How it works

On startup the extension calls `starship prompt --terminal-width=<width>` as a subprocess (with `STARSHIP_SHELL=bash` so it emits plain ANSI codes). It takes only the **first line** of the output, which is the info bar for two-line prompts and strips the `❯` line naturally.

Data is fetched asynchronously and cached. The footer renders immediately with whatever is cached and updates as fetches complete. Refresh triggers:

| Event                 | What refreshes                    |
| --------------------- | --------------------------------- |
| Session start         | Starship prompt + PR              |
| Branch change         | Starship prompt + PR              |
| Agent turn end        | Starship prompt + PR              |
| Terminal resize       | Starship prompt (width changed)   |
| Model/thinking change | Right side re-renders immediately |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

# Contributing to pi-starship

Thanks for your interest! Contributions are welcome.

## Setup

```bash
git clone https://github.com/rajivm1991/pi-starship
cd pi-starship
```

No `npm install` needed — the extension has no runtime dependencies beyond what pi provides (`@mariozechner/pi-coding-agent`, `@mariozechner/pi-ai`, `@mariozechner/pi-tui`).

To test your changes locally, point pi at the source directly:

```bash
pi -e ./src/index.ts
```

Or symlink it into your global extensions directory:

```bash
ln -s $(pwd)/src/index.ts ~/.pi/agent/extensions/pi-starship.ts
```

Then use `/reload` inside pi to pick up changes without restarting.

## Project structure

```
pi-starship/
├── src/
│   └── index.ts   # entire extension — fetch helpers + pi.on wiring
├── package.json
├── README.md
├── CONTRIBUTING.md
└── LICENSE
```

## What lives where in `src/index.ts`

| Section | What it does |
|---------|-------------|
| Colour helpers | Raw ANSI helpers for the right side only (left side colours come from starship) |
| `fetchStarshipPrompt()` | Calls `starship prompt`, strips shell wrappers, trims trailing reset codes |
| `fetchPR()` | Calls `gh pr view` to get number + URL for the clickable link |
| Extension wiring | `session_start`, `agent_end`, `thinking_level_select` handlers + `setFooter` render |

## Ideas for contributions

- **More right-side segments** — context window usage bar, session name, agent turn count
- **Configurable segments** — let users opt-in/out of PR, tokens, cost via pi flags
- **Right-side theming** — use pi's theme colours instead of hardcoded ANSI for better theme compatibility
- **`starship` fallback** — show a minimal built-in footer when starship is not installed

## Submitting a PR

1. Fork the repo and create a branch: `git checkout -b my-feature`
2. Make your changes in `src/index.ts`
3. Test with `pi -e ./src/index.ts`
4. Open a PR with a short description of what changes and why

Please keep PRs focused — one feature or fix per PR makes review easier.

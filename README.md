# api-mapper

> A skill for AI agents that reverse-engineers and documents undocumented APIs using Chrome CDP and curl.

The agent controls a real Chrome browser via Node.js scripts over raw WebSocket CDP, intercepts all
network traffic, analyses JS bundles, and produces reproducible markdown documentation — complete
enough that any client or SDK can be built from the docs alone, in any language, without ever
opening a browser again.

## What it does

- Intercepts XHR/Fetch traffic from a live Chrome session driven by the agent
- Downloads and reverse-engineers JS bundles to extract computed fields, signatures, and hardcoded constants
- Probes discovered endpoints with curl to map request/response shapes and auth requirements
- Writes findings continuously to structured markdown docs, one file per API domain area
- For complex flows, produces a standalone Node.js script that replays the full flow without a browser

## Requirements

- Node.js + npm
- Google Chrome installed (the agent will locate the executable)
- curl (or `curl.exe` on Windows)

## Repository layout

```
repo/
├── scripts/
│   ├── flows/       ← end-to-end flow scripts, one per documented flow
│   └── *.js         ← reusable helpers
├── artifacts/
│   └── {timestamp}/ ← one folder per research session
│       ├── requests.ndjson
│       ├── bundles/
│       ├── probes/
│       └── js-analysis/
└── docs/
    └── apis/
        ├── README.md      ← index of all areas and sessions
        └── {area}.md      ← one file per domain area
```

Session folders are named using an ISO 8601 timestamp with colons and dots replaced by dashes
(e.g. `artifacts/2025-01-15T14-30-00/`).

## Skill structure

```
SKILL.md                          ← main skill instructions
references/
  cdp-capture.md                  ← how to write CDP browser scripts
  curl-probe-patterns.md          ← probe script guidelines
  js-flow-analysis.md             ← how to reverse-engineer a flow
  flow-script-template.md         ← requirements for flow scripts
  doc-template.md                 ← API doc and README templates
  chrome-windows.md               ← finding Chrome on Windows
  chrome-macos.md                 ← finding Chrome on macOS
  chrome-linux.md                 ← finding Chrome on Linux
```

## How the agent works

The agent decides its own approach — there is no fixed sequence of steps. It uses whichever
combination of browser interception, curl probing, and JS analysis the situation calls for, and
keeps going until the full reachable API surface is documented or it hits a wall.

> [!NOTE]
> Chrome runs visibly (not headless) to avoid bot detection. The agent drives the browser
> autonomously via CDP. The user only needs to intervene for CAPTCHAs or to provide credentials.

When the agent hits something it can't get past on its own, it writes current findings to
`docs/apis/` and stops to ask the user for what it needs to continue.

## Output

The primary output is `docs/apis/` — one markdown file per domain area, updated continuously
throughout the research session. Each file documents endpoints, auth requirements, request/response
shapes, and any flows that require client-side computation.

For flows that involve computed values (signatures, token exchanges, custom encoding, etc.), the
agent also produces a standalone Node.js script in `scripts/flows/` that executes the full flow
without a browser.

> [!IMPORTANT]
> Docs are written to be language-agnostic. The goal is that anyone — or any agent building a
> Python, Rust, Go, or Node.js client — can implement the full API from the docs alone.
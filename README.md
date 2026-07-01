# api-mapper

> A Codex skill for mapping undocumented website APIs with Chrome CDP, curl, and JS bundle analysis.

This repository contains the `api-mapper` skill package. The skill guides an agent through
capturing browser traffic, probing discovered endpoints, reverse-engineering client-side request
logic, and writing enough documentation to build a client without opening the browser again.

## What this repo contains

```text
api-mapper/
  SKILL.md
  agents/
    openai.yaml
  references/
    api-doc-requirements.md
    cdp-capture.md
    chrome-linux.md
    chrome-macos.md
    chrome-windows.md
    flow-script-template.md
    js-flow-analysis.md
```

## Requirements

- Python 3 + pip
- Google Chrome
- curl or `curl.exe`

## Usage

After the skill is installed, invoke it directly:

```text
Use $api-mapper to reverse-engineer and document https://example.com.
```

The agent will open a real Chrome session, capture XHR/Fetch traffic, inspect JS bundles when
requests contain computed values, and probe endpoints directly with curl.

> [!NOTE]
> Chrome runs visibly. The user only needs to step in for credentials, CAPTCHAs, rate limits, or
> other walls the agent cannot pass.

## Generated output

The skill writes findings into the target project, not this skill repository:

```text
artifacts/{timestamp}/
  requests.ndjson
  bundles/
docs/apis/
  README.md
  {area}.md
scripts/
  flows/
```

API docs are language-agnostic. Complex browser-derived flows also get a runnable Python replay
script under `scripts/flows/`.

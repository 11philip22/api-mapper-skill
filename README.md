# API Mapper Skills

> Agent skills for reverse-engineering undocumented APIs from websites and Android APKs.

[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-2-6f42c1?style=flat-square)](#skills)
[![Codex](https://img.shields.io/badge/Codex-compatible-10a37f?style=flat-square&logo=openai&logoColor=white)](#install)
[![OpenCode](https://img.shields.io/badge/OpenCode-compatible-1f6feb?style=flat-square)](#install)
[![Claude](https://img.shields.io/badge/Claude-compatible-d97757?style=flat-square&logo=anthropic&logoColor=white)](#install)

This repository contains two skills that trace endpoints, authentication, request construction,
serialization, signing, and multi-step flows. Both produce implementation-ready API documentation
and small replay scripts for behavior that cannot be described as a plain HTTP request.

## Skills

| Skill | Target | Primary analysis path |
| --- | --- | --- |
| `api-mapper` | Websites | Chrome CDP capture, curl probing, and JavaScript bundle analysis |
| `apk-api-mapper` | Android APKs | Hermes/JavaScript bundles for React Native; JADX for Android-native code and bridges |

The APK mapper follows the layer that owns each request. React Native Hermes bundles are analyzed
with `hermes-dec`, plain JavaScript bundles are traced directly, and JADX is used when behavior
lives in Android code or crosses a native bridge. `apktool` is an optional fallback for decoded
resources or broad smali searches.

## Requirements

Install only the tools needed for the selected skill and target:

- Website mapping: Python 3, Google Chrome, and curl or `curl.exe`
- APK mapping: an APK file or JADX project, plus `hermes-dec` for Hermes bytecode, JADX MCP for
  Android-native analysis, or `apktool` when decoded resources or broad smali searches are needed

Missing optional tools are not installed automatically.

## Install

Install either or both skills with the `skills` CLI:

```bash
npx -y skills add github.com/11philip22/api-mapper-skill -a codex --global --skill api-mapper
npx -y skills add github.com/11philip22/api-mapper-skill -a codex --global --skill apk-api-mapper
```

Replace `codex` with `opencode` or `claude` to install for another supported agent.

## Usage

Map a website:

```text
Use the api-mapper skill to reverse-engineer and document https://example.com.
```

Map an APK:

```text
Use the apk-api-mapper skill to reverse-engineer and document the APIs used by app.apk.
```

An APK may instead be supplied through an open JADX project. When native analysis is required, the
skill uses the project's read-only JADX MCP integration.

> [!NOTE]
> Website mapping opens a visible Chrome session. User input is only needed for walls such as
> missing credentials, CAPTCHAs, or blocking rate limits.

## Generated output

Results are written into the target project, not this skill repository:

```text
docs/apis/
  README.md
  {area}.md
scripts/flows/
  {flow}.py                 # only for non-trivial reproducible flows
artifacts/{timestamp}/      # website captures and JavaScript bundles
artifacts/apk/{timestamp}/  # APK artifacts, only when extraction is needed
```

Documentation is language-agnostic and includes evidence, confidence, request schemas, and the
state shared across multi-request flows. Replay scripts run without reopening Chrome or JADX.

## Repository layout

```text
api-mapper/
  SKILL.md
  agents/openai.yaml
  references/
apk-api-mapper/
  SKILL.md
  agents/openai.yaml
  references/
```

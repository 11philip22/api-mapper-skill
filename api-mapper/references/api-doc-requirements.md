# API Documentation Requirements

Write `docs/apis/{area}.md` in whatever structure is clearest. One file per
domain area, updated as discoveries are made.

The docs must be complete enough that someone can implement each request and
flow from scratch.

Include, where applicable:

- Area scope, status, target/base URL, and relevant artifact/session paths
- Auth requirements, cookies, CSRF handling, shared headers, and how tokens are obtained
- Each endpoint's method, path or full URL, path/query parameters, request body,
  required headers/cookies, response shape or examples, status/error cases, and replay notes
- Any quirks needed for a faithful replay: ordering, redirects, rate limits, content types,
  encodings, pagination, upload/download details, or non-obvious defaults
- Discovered but unprobed endpoints, with the evidence source and current blocker
- For multi-step or JS-derived flows: ordered request sequence, data passed between steps,
  computed fields with algorithm, inputs, output encoding, language-agnostic pseudocode,
  extracted constants with bundle source, and the corresponding `scripts/flows/{flow}.py`

`docs/apis/README.md` should index the target, documented areas, session artifacts, statuses,
and open questions.

# Flow Reverse-Engineering

When a flow requires understanding beyond what the raw HTTP requests reveal, dig into the JS
bundles. The goal is to understand the flow completely enough to reimplement it without a browser,
in any language -- knowing every value sent in every request, where it comes from, and how it is
derived.

## Output

Document JS-derived flow findings in `docs/apis/{area}.md` using
`references/api-doc-requirements.md`.

Create the browser-free replay script using `references/flow-script-template.md`.

Use artifacts only for scratch notes or large intermediate analysis that would clutter the final
API docs.

## What to look for in the JS

- Any value in a request that isn't a plain user input or a direct copy from a prior response
  is being computed -- find the code that computes it
- Hardcoded constants (keys, IDs, salts, magic numbers, chunk sizes) must be identified and
  their location in the bundle noted
- Custom encoding or crypto functions must be extracted verbatim, not summarised
- Note the mapping from site JS idioms to standard equivalents (library used, standard algorithm)

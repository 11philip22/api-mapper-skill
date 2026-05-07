# Flow Reverse-Engineering

When a flow requires understanding beyond what the raw HTTP requests reveal, dig into the JS
bundles. The goal is to understand the flow completely enough to reimplement it without a browser,
in any language — knowing every value sent in every request, where it comes from, and how it is
derived.

## What the output must capture

For each flow analysed, produce two things:

**Annotated analysis** at `artifacts/{timestamp}/js-analysis/{flow}-notes.md` — a clear
explanation of what the flow does, what is being computed and why, and what a reimplementer
needs to know. This should read as useful context, not just raw observations.

**Flow script** at `scripts/flows/{flow}.js` — a standalone Node.js script that executes the
full flow without a browser. See `references/flow-script-template.md`.

## What to look for in the JS

- Any value in a request that isn't a plain user input or a direct copy from a prior response
  is being computed — find the code that computes it
- Hardcoded constants (keys, IDs, salts, magic numbers, chunk sizes) must be identified and
  their location in the bundle noted
- Custom encoding or crypto functions must be extracted verbatim, not summarised
- Note the mapping from site JS idioms to standard equivalents (library used, standard algorithm)

## What goes in the API doc

The `docs/apis/{area}.md` flow section must be complete enough that someone who has never
seen the site JS can implement the flow from scratch. Include the full request sequence, every
computed field with its algorithm and inputs, all extracted constants with their bundle source,
and a pointer to the flow script.

# Flow Script Requirements

Every documented flow gets a script at `scripts/flows/{flow-name}.py`.

## What the script must contain

**Header comment block** -- at the top, document:
- What the flow accomplishes
- Which JS logic was extracted and from which bundle/lines
- Usage instructions (which env vars or CLI args to pass)
- What the script prints on success

**Extracted constants** -- any values hardcoded in the site's JS (keys, IDs, salts, chunk sizes, etc.)
must be reproduced here as named constants with a comment citing the source bundle and line number.

**Ported functions** -- any custom encoding, crypto, or transformation functions extracted from the
bundle must be included verbatim or faithfully ported, with a comment attributing them to their source.

**Full flow implementation** -- all HTTP calls in sequence, with state (tokens, session IDs, nonces)
carried between steps exactly as the site's JS does it.

**TLS/client fingerprint compatibility** -- if the site rejects plain Python HTTP clients or appears
to require a browser-like TLS/client fingerprint, use an appropriate TLS spoofing library for the
flow script. Document which library is used, what browser/client fingerprint it emulates, and why it
is required.

**Clear output** -- print the result as JSON to stdout. Exit with a non-zero code and a clear error
message on failure.

## The test

`python scripts/flows/{flow}.py` must work from a clean terminal with no browser open.

## Common flow types

**Auth flows** -- typically involve fetching a config or nonce, computing a signature or hash, then
exchanging credentials for a token. Document every computed field.

**Token refresh** -- usually a single request but may involve re-deriving a signature. Often shares
extracted constants with the login flow.

**File upload** -- sites frequently chunk files client-side, compute a session ID, and upload each
chunk with an index and completion flag. The chunk size is usually a hardcoded constant in the bundle.

**File download** -- some sites generate signed URLs or require a pre-request to obtain a download
token. Document the signing algorithm if present.

# Deliverables

The output must let a client or SDK author reproduce the APK's network behavior without JADX.

## `docs/apis/README.md`

Record:

- application ID, version and APK hash/path when known, analysis date, framework, and scope
- shared base URLs, environments, network stack, authentication summary, and artifact paths
- one row per API area with status: complete, partial, blocked, or candidate-only
- links to area docs and replay scripts, plus unresolved questions and static-analysis limitations

## `docs/apis/{area}.md`

Use the clearest structure for the area. Include, where applicable:

- scope, status, ownership (first-party or named third party), base URL, and shared client behavior
- each endpoint's method and resolved URL/path; path/query parameters; headers, cookies, and auth;
  body/content type; request and response schemas; errors; pagination/retry/cache behavior
- ordered multi-request flows and the state passed between requests
- computed values with exact algorithm, inputs, constants, input ordering, and output encoding;
  include language-neutral pseudocode when that makes the implementation clearer
- evidence beside each endpoint: class and method, resource, smali method, or Hermes function/string
- confidence (`confirmed-static`, `inferred`, or `candidate`) and what runtime behavior was not tested
- blockers and discovered candidates that could not be connected to a reachable call site

Do not fabricate captured requests or responses. A model class is a schema inference unless runtime
evidence confirms it.

## `scripts/flows/{flow}.py`

Create a script only when a flow has multiple requests or non-trivial computed values. A plain HTTP
request belongs in docs.

Keep the script standalone and small. Prefer the Python standard library and already-installed
dependencies; use a maintained crypto dependency when the standard library cannot safely implement
the APK's algorithm.

The script must:

- identify the APK source methods/functions for the ported behavior in its header/comments
- accept secrets and account-specific values through CLI arguments or environment variables
- reproduce the complete request sequence and custom transformations without JADX or a device
- print JSON on success and a clear error to stderr with a non-zero exit status on failure
- provide `--help` and an offline self-check for every ported parser/encoder/signature algorithm

Syntax-check and run the offline path. Run the live flow only when the user authorized its effects
and supplied the required inputs.

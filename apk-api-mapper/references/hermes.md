# Hermes Bundle Analysis

Read this only when the APK contains `assets/index.android.bundle` and the asset is Hermes bytecode.
If it is plain JavaScript, search and trace that source directly.

Extract only the bundle when possible. Put generated output under the current
`artifacts/apk/{timestamp}/hermes/` directory.

Depending on the installation, hermes-dec exposes `hbc-file-parser`, `hbc-disassembler`, and
`hbc-decompiler`, or the same commands prefixed with `hermes-dec.`. Check available command names
instead of assuming one spelling.

```text
hbc-file-parser assets/index.android.bundle
hbc-decompiler assets/index.android.bundle artifacts/apk/{timestamp}/hermes/index.android.js
hbc-disassembler assets/index.android.bundle artifacts/apk/{timestamp}/hermes/index.android.hasm
```

The decompiler emits pseudocode, not trustworthy runnable JavaScript. Use it for structure, then
corroborate endpoints, constants, branches, and argument order against the string tables and
disassembly. If decompilation fails, continue from disassembly rather than treating the bundle as
opaque.

Search for URLs, paths, header names, GraphQL operations, `fetch`, `XMLHttpRequest`, Axios/Apollo
markers, token storage, signing inputs, and native-module bridges. Follow references from each
string to its containing function and callers. Record Hermes function IDs or offsets alongside
each finding so another analyst can relocate the evidence.

If a request delegates signing, encryption, or transport through a native module, return to JADX
and trace that bridge. Stop at an unsupported native-library boundary and document it; do not infer
the missing algorithm from names.

# API Doc Template

Use this template for each file in `docs/apis/{area}.md`. One file per domain area.

Update the file incrementally as you discover more — do not wait until the end of a session.

---

## {Area} API

> Last updated: {date}
> Session: `artifacts/{timestamp}/`
> Status: In progress | Complete | Auth blocked

### Overview

Brief description of what this area of the API does and what flows it powers.

---

### Base URL

    https://target.com/api/{area}

---

### Authentication

| Method | Value |
|--------|-------|
| Type | Bearer token / Cookie / None |
| Header | `Authorization: Bearer <token>` |
| How to obtain | POST /api/auth/login → `.token` field |

---

### Endpoints

#### GET /{path}

**Purpose:** What this endpoint does.

**Auth required:** Yes / No

**Query params:**

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| `page` | int | No | Page number (default 1) |
| `limit` | int | No | Items per page (default 20) |

**Response (200):**

```json
{
  "items": [{ "id": "string", "name": "string" }],
  "total": 100,
  "page": 1
}
```

**Error responses:**

| Status | Meaning |
|--------|---------|
| 401 | Token missing or invalid |
| 403 | Insufficient permissions |
| 404 | Resource not found |

---

#### POST /{path}

**Purpose:** ...

**Auth required:** Yes / No

**Request body:**

```json
{ "field": "value" }
```

**Response (201):**

```json
{ "id": "new-id", "field": "value" }
```

---

### Unknown / Unprobed Endpoints

List endpoints discovered but not yet fully probed:

- `GET /api/{path}/export` — discovered in JS bundle, not yet probed
- `DELETE /api/{path}/{id}` — returns 403 with current auth level

---

### Notes

- Any quirks, non-standard behaviour, CSRF requirements, etc.
- Rate limit observations (e.g. "429 after ~30 req/min")

---

### Flows

> Add this section for any flow that warrants deeper documentation. Every flow listed here must
> have a corresponding runnable script at `scripts/flows/{flow}.js`.

#### {Flow Name}

> This flow requires client-side logic. The steps below are complete — no browser needed.
> Runnable implementation: `scripts/flows/{flow}.js`

**What it does:** Brief description.

**Request sequence:**

| Step | Method | Path | Purpose |
|------|--------|------|---------|
| 1 | GET | `/api/auth/config` | Fetch clientId and publicKey |
| 2 | POST | `/api/auth/challenge` | Exchange computed sig for challenge token |
| 3 | POST | `/api/auth/login` | Submit credentials, receive tokens |

**Computed fields:**

Document every field that is not a plain user input or copied from a prior response:

    Step 2 — body.sig
      Algorithm: HMAC-SHA256
      Input:     clientId + ":" + timestamp  (clientId from step 1, timestamp = Date.now())
      Key:       CLIENT_SECRET (hardcoded in bundle — see below)
      Encoding:  hex lowercase
      Pseudocode: hmac_sha256(key=CLIENT_SECRET, msg=clientId + ":" + timestamp), hex-encoded

    Step 3 — body.passwordHash
      Algorithm: SHA-256
      Input:     plaintext password
      Encoding:  hex lowercase
      Pseudocode: sha256(password), hex-encoded

**Extracted constants:**

| Constant | Value | Source |
|----------|-------|--------|
| `CLIENT_SECRET` | `a3f9d2...` | `main.chunk.js` line 4521 |
| `CHUNK_SIZE` | `1048576` (1MB) | `upload.chunk.js` line 2187 |

**Ported functions** (if any custom encoding/crypto was extracted):

```js
// Extracted verbatim from main.chunk.js line 8823
// Custom base62 encoder used to encode the challenge response
function base62encode(buf) {
  // ...
}
```

**Success response:**

```json
{ "accessToken": "eyJ...", "refreshToken": "dGt..." }
```

**Error cases:**

| Condition | Status | Response |
|-----------|--------|----------|
| Wrong password | 401 | `{ "error": "invalid_credentials" }` |
| Expired challenge | 400 | `{ "error": "challenge_expired" }` |

---

## docs/apis/README.md Template

### API Map — {Site Name}

> Target: https://target.com
> Mapping started: {date}

#### Domain Areas

| Area | File | Status | Endpoints found |
|------|------|--------|-----------------|
| Authentication | [auth.md](auth.md) | Complete | 4 |
| Users | [users.md](users.md) | In progress | 7 |
| Search | [search.md](search.md) | Auth blocked | 2 |

#### Sessions

| Timestamp | Focus | Artifacts |
|-----------|-------|-----------|
| 2025-01-15T14-30-00 | Initial capture + auth | artifacts/2025-01-15T14-30-00/ |

#### Open Questions

- [ ] What scope is needed for admin endpoints?
- [ ] Is there a GraphQL endpoint at /graphql?
- [ ] WebSocket at /ws — purpose unknown

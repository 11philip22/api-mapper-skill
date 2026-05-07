# Probe Scripts

Probe scripts live in `artifacts/{timestamp}/probes/` as .sh (macOS/Linux) or .ps1 (Windows).
## Recording results

After running each probe, append findings to `artifacts/{timestamp}/probes/{area}-results.md`:

```
GET /api/users
- Status: 200
- Auth required: yes (Bearer)
- Response shape: { users: [{ id, email, role }], total: number }
- Notes: paginated via ?page=&limit=
```

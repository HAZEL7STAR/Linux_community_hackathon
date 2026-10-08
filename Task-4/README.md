# Task 4 — Separate Port

## Goal
Serve a different HTML page on a separate port.

## Files
- `index2.html`
- `nginx-8080.conf`

## Port
`8080`

## Verification
From the client:

```bash
wget -qO- http://10.10.159.115:8080
```

The response should be the second HTML page.

## Evidence
`evidence/05-additional-verification.png`

> Make sure the final screenshot visibly proves access to `10.10.159.115:8080`.

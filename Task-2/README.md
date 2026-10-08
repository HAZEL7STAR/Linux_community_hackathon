# Task 2 — Remote Access

## Goal
Access the Nginx page from another computer on the same network.

## Client
`10.10.110.93/22`

## Server
`10.10.159.115`

## Verification
```bash
wget http://10.10.159.115
```

Expected:
```text
HTTP request sent, awaiting response... 200 OK
```

## Evidence
`evidence/02-client-network-and-access.png`

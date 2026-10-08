# Task 3 — Remote Modification

## Goal
Modify the hosted HTML page from another computer and verify the change.

## SSH
```bash
ssh hazel7star@10.10.159.115
```

## Edit
```bash
sudo nano /var/www/html/index.html
```

## Client verification
```bash
wget http://10.10.159.115
cat index.html
```

Verified content included:

```text
Name: PRIYADHARSHINI M
Roll No: 7376242IT263
```

## Evidence
- `evidence/03-remote-modification-and-verification.png`
- `evidence/04-modified-page-output.png`

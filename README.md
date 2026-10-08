# Linux OpenHack 2026 — Nginx Web Server Tasks

## Participant implementation

This repository documents the Linux OpenHack 2026 Nginx practical tasks.

### Environment

| Item | Value |
|---|---|
| Server OS | Kali Linux |
| Server IP | `10.10.159.115` |
| Client OS | Ubuntu Linux |
| Client IP | `10.10.110.93/22` |
| Web server | Nginx |
| Primary HTTP port | `80` |
| Secondary HTTP port | `8080` |
| SSH | `22` |
| Custom page source | Organizer-provided `index.html` |

> **Important:** Replace any placeholder such as `[YOUR NAME]` or `[YOUR ROLL NO]` with the participant's actual details before final submission.

---

## Task status

| Task | Description | Status |
|---|---|---|
| 1 | Set up Nginx to serve the organizer HTML page | ✅ Completed |
| 2 | Access the hosted page from another computer | ✅ Completed |
| 3 | Modify the page remotely and verify the change | ✅ Completed |
| 4 | Serve a different HTML page on a separate port | ✅ Completed |
| 5 | Convert the hosted page to HTTPS | ⏳ Requires final execution/verification |

---

# Task 1 — Nginx server setup

## Objective

Configure the Linux server so that Nginx serves the organizer-provided custom HTML page over HTTP.

## Implementation summary

The organizer-provided page was placed under the Nginx web root:

```text
/var/www/html/index.html
```

The server was verified locally using:

```bash
wget http://localhost
```

The HTTP response returned `200 OK`, confirming that the web server was serving an HTML document.

## Verification evidence

See:

```text
Task-1/evidence/01-server-and-ssh.png
```

---

# Task 2 — Access from another computer

## Objective

Access the Nginx-hosted page from another Linux computer on the same network.

## Network details

Server:

```text
10.10.159.115
```

Client:

```text
10.10.110.93/22
```

The client retrieved the page from:

```bash
wget http://10.10.159.115
```

The response showed:

```text
HTTP request sent, awaiting response... 200 OK
```

This verifies network-level access to the Nginx HTTP service.

## Verification

The downloaded page was saved as `index.html` on the client.

Evidence:

```text
Task-2/evidence/02-client-network-and-access.png
```

---

# Task 3 — Modify the page from another computer

## Objective

Use the second computer to modify the hosted HTML page and verify that the modified content is served by Nginx.

## Method

SSH was used to access the server:

```bash
ssh hazel7star@10.10.159.115
```

The hosted file was edited:

```bash
sudo nano /var/www/html/index.html
```

The page was then retrieved again from the client:

```bash
wget http://10.10.159.115
```

The downloaded HTML was inspected with:

```bash
cat index.html
```

The verified modified page contained:

```text
Name: PRIYADHARSHINI M
Roll No: 7376242IT263
```

The HTTP request returned `200 OK`.

## Why this proves the task

The modification was made on the server through the remote SSH session. The second computer then fetched the page over HTTP and received the updated content. Therefore the client was seeing the current file served by Nginx rather than a stale local copy.

## Evidence

```text
Task-3/evidence/03-remote-modification-and-verification.png
Task-3/evidence/04-modified-page-output.png
```

---

# Task 4 — Different HTML page on a separate port

## Objective

Run a second HTML page using Nginx on a different HTTP port and verify access.

## Implementation

The second page is maintained as:

```text
Task-4/index2.html
```

The Nginx configuration is maintained as:

```text
Task-4/nginx-8080.conf
```

The intended endpoint is:

```text
http://10.10.159.115:8080
```

Client-side verification:

```bash
wget -qO- http://10.10.159.115:8080
```

A successful HTTP response confirms that the second server block is listening on port `8080` and serving the separate HTML page.

## Evidence

```text
Task-4/evidence/05-additional-verification.png
```

> If the screenshot does not clearly show the `:8080` request and response, take one additional terminal screenshot before final submission. Evidence should visibly prove the port number and successful response.

---

# Task 5 — HTTPS conversion

## Objective

Serve the hosted page securely over HTTPS.

This section is a ready-to-run procedure. **Do not mark Task 5 as completed until the HTTPS page has actually been demonstrated to the organizers.**

## 1. Generate a self-signed certificate

Run on the server:

```bash
sudo mkdir -p /etc/nginx/ssl

sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048   -keyout /etc/nginx/ssl/openhack.key   -out /etc/nginx/ssl/openhack.crt   -subj "/CN=10.10.159.115"
```

## 2. Create the HTTPS Nginx configuration

Create:

```bash
sudo nano /etc/nginx/sites-available/https
```

Use:

```nginx
server {
    listen 443 ssl;
    listen [::]:443 ssl;

    server_name 10.10.159.115;

    root /var/www/html;
    index index.html;

    ssl_certificate /etc/nginx/ssl/openhack.crt;
    ssl_certificate_key /etc/nginx/ssl/openhack.key;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Enable it:

```bash
sudo ln -s /etc/nginx/sites-available/https /etc/nginx/sites-enabled/https
```

## 3. Validate the configuration

```bash
sudo nginx -t
```

Expected result:

```text
syntax is ok
test is successful
```

## 4. Reload Nginx

```bash
sudo systemctl reload nginx
```

If reload fails, check:

```bash
sudo systemctl status nginx
sudo journalctl -xeu nginx
```

## 5. Verify HTTPS locally

Because the certificate is self-signed, use:

```bash
wget --no-check-certificate -qO- https://10.10.159.115
```

Or:

```bash
curl -k https://10.10.159.115
```

The output should contain the same hosted HTML content.

## 6. Verify from the second computer

From the Ubuntu client:

```bash
wget --no-check-certificate -qO- https://10.10.159.115
```

Successful HTML output proves that the client can reach the HTTPS service.

You can additionally verify the TLS handshake:

```bash
openssl s_client -connect 10.10.159.115:443
```

## 7. Evidence required

Take a terminal screenshot showing all three:

```text
sudo nginx -t
```

successful,

```text
https://10.10.159.115
```

being accessed successfully, and

the returned organizer HTML content.

Save it as:

```text
Task-5/evidence/01-https-verification.png
```

---

# Final verification checklist

Before submitting:

- [x] Task 1 demonstrated to organizers
- [x] Task 2 demonstrated to organizers
- [x] Task 3 demonstrated to organizers
- [x] Task 4 demonstrated to organizers
- [ ] Task 5 demonstrated to organizers
- [ ] All evidence committed to GitHub
- [ ] README updated with final Task 5 status
- [ ] GitHub repository link submitted through the hackathon form
- [ ] Feedback form completed after the hackathon

## Useful final Git commands

From the repository:

```bash
git status
git add .
git commit -m "Document Linux OpenHack Nginx tasks"
git push
```

Verify:

```bash
git log --oneline -5
git status
```

The working tree should be clean after a successful push.

---

## Submission links

### GitHub repository

Paste the final repository URL into the official submission form:

https://forms.gle/F13DThd2eUv3D1pw8

### Hackathon feedback

Complete after the hackathon:

https://forms.gle/6Hie6ARVzYz92fZo8

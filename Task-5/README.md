# Task 5 — HTTPS

## Status
**Pending final execution and demonstration.**

## Goal
Serve the hosted page over HTTPS.

## Certificate
Create a self-signed certificate:

```bash
sudo mkdir -p /etc/nginx/ssl

sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout /etc/nginx/ssl/openhack.key \
  -out /etc/nginx/ssl/openhack.crt \
  -subj "/CN=10.10.159.115"
```

## Nginx configuration

Create:

```bash
sudo nano /etc/nginx/sites-available/https
```

Configuration:

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

Enable:

```bash
sudo ln -s /etc/nginx/sites-available/https /etc/nginx/sites-enabled/https
```

Test:

```bash
sudo nginx -t
```

Reload:

```bash
sudo systemctl reload nginx
```

Verify:

```bash
wget --no-check-certificate -qO- https://10.10.159.115
```

or:

```bash
curl -k https://10.10.159.115
```

## Evidence

After successful verification, save a terminal screenshot as:

```text
evidence/01-https-verification.png
```

Then change the root README Task 5 status from pending to completed.

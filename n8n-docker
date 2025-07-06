# Installing and Running n8n on macOS with Docker (Colima) and ngrok

## Prerequisites
- macOS (Apple Silicon or Intel)
- Homebrew installed
- Docker CLI & Docker Compose installed via Homebrew
- Colima installed for Docker daemon on macOS
- ngrok installed (for public webhook URLs)

---

## Step 1: Install Colima and Start Docker VM

```bash
brew install colima
colima start
````

Verify Docker daemon is running:

```bash
docker info
```

---

## Step 2: Create Project Directory and Persistent Volume

```bash
mkdir -p ~/n8n-docker
mkdir -p ~/.n8n
cd ~/n8n-docker
```

---

## Step 3: Create `.env` File for Environment Variables

Create a `.env` file inside `~/n8n-docker` with the following content:

```env
DATA_FOLDER=/home/node/.n8n
TZ=Asia/Dubai
WEBHOOK_URL=https://your-ngrok-url/webhook/
N8N_DIAGNOSTICS_ENABLED=false

# Optional basic auth settings:
# N8N_BASIC_AUTH_ACTIVE=true
# N8N_BASIC_AUTH_USER=admin
# N8N_BASIC_AUTH_PASSWORD=securepassword
```

> **Note:** Replace `WEBHOOK_URL` after you get your ngrok URL.

---

## Step 4: Create `docker-compose.yml`

Create a `docker-compose.yml` file inside `~/n8n-docker`:

```yaml
version: '3.8'

services:
  n8n:
    image: n8nio/n8n:latest
    restart: always
    ports:
      - 5678:5678
    environment:
      - TZ=${TZ}
      - WEBHOOK_URL=${WEBHOOK_URL}
      - N8N_DIAGNOSTICS_ENABLED=${N8N_DIAGNOSTICS_ENABLED}
      # Uncomment for basic auth:
      # - N8N_BASIC_AUTH_ACTIVE=${N8N_BASIC_AUTH_ACTIVE}
      # - N8N_BASIC_AUTH_USER=${N8N_BASIC_AUTH_USER}
      # - N8N_BASIC_AUTH_PASSWORD=${N8N_BASIC_AUTH_PASSWORD}
    volumes:
      - ~/.n8n:/home/node/.n8n
```

---

## Step 5: Start n8n Container

Start the container in detached mode:

```bash
docker compose up -d
```

Verify it’s running:

```bash
docker ps
```

Open n8n UI in your browser at:

```
http://localhost:5678
```

---

## Step 6: Install and Configure ngrok for Webhook Tunneling

1. Install ngrok (if not installed):

```bash
brew install ngrok/ngrok/ngrok
```

2. Sign up on [ngrok.com](https://dashboard.ngrok.com/signup) and get your **authtoken**.

3. Authenticate ngrok locally:

```bash
ngrok config add-authtoken YOUR_AUTHTOKEN
```

4. Run ngrok to expose n8n port:

```bash
ngrok http 5678
```

Take note of the forwarding URL (e.g., `https://511c-91-73-84-205.ngrok-free.app`).

---

## Step 7: Update `.env` with ngrok URL and Restart n8n

Update your `.env` file with the ngrok URL:

```env
WEBHOOK_URL=https://511c-91-73-84-205.ngrok-free.app/webhook/
```

Restart the container to apply changes:

```bash
docker compose down
docker compose up -d
```

---

## Done! 🎉

Your n8n instance is now running with:

* Persistent storage in `~/.n8n`
* External webhook access via ngrok URL
* Configured timezone and optional basic auth

---

## Notes

* For production, consider using a custom domain with SSL.
* Secure your n8n editor with basic auth or other auth mechanisms.
* Backup your `~/.n8n` folder regularly to avoid data loss.

---

If you find this useful, feel free to contribute or raise issues!

---

*Happy automating! 🚀*

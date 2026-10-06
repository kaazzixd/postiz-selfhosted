# Postiz Self-Hosted (Local Setup)

Ready-to-run **Postiz** – open-source social media scheduler (alternative to Buffer / Hootsuite).

This repository is pre-configured for **local use** on your Windows PC with Docker Desktop.

- Official project: https://github.com/gitroomhq/postiz-app
- Docs: https://docs.postiz.com

---

## Requirements

- **Docker Desktop** for Windows (you already have this)
- At least **4 GB RAM** free (8 GB recommended)
- ~10–15 GB free disk space (first run downloads images)

---

## Quick Start (3 steps)

### 1. Clone this repository

Open **PowerShell** or **Command Prompt** and run:

```powershell
git clone https://github.com/kaazzixd/postiz-selfhosted.git
cd postiz-selfhosted
```

### 2. Start everything

```powershell
docker compose up -d
```

The first start takes **2–5 minutes** while Docker downloads the images (Postiz + Postgres + Redis + Temporal + Elasticsearch).

### 3. Open Postiz

When everything is healthy, open your browser:

**http://localhost:4007**

Create your account (first user becomes admin).

You can also check background jobs at: **http://localhost:8080** (Temporal UI)

---

## Useful Commands

| Action                    | Command                          |
|---------------------------|----------------------------------|
| Start                     | `docker compose up -d`           |
| Stop                      | `docker compose down`            |
| View logs                 | `docker compose logs -f postiz`  |
| Restart after config change | `docker compose down && docker compose up -d` |
| Check status              | `docker compose ps`              |

---

## What’s included

- Postiz app (port **4007**)
- PostgreSQL
- Redis
- Temporal (workflow engine) + Elasticsearch + Temporal UI (port **8080**)
- Local file storage for media

JWT secret is already generated and set for you.

---

## Connecting social channels (Telegram is easiest)

1. In Postiz go to **Channels** / **Integrations**
2. For **Telegram**:
   - Create a bot with [@BotFather](https://t.me/BotFather) on Telegram
   - Copy the bot token
   - Add the token in the Postiz Telegram connection (or put it in `docker-compose.yaml` under the Telegram-related env vars if needed, then restart)

Other platforms (X, LinkedIn, Instagram, etc.) require creating developer apps on each platform and adding the Client ID / Secret in the `docker-compose.yaml` environment section, then restarting.

---

## Connecting to n8n (optional)

1. In Postiz go to **Developers** / **API** and copy your API key
2. In n8n install the community node `n8n-nodes-postiz`
3. Create a credential with:
   - Host: `http://host.docker.internal:4007/api` (if n8n is also in Docker) or `http://localhost:4007/api`
   - API Key: the one you copied

---

## Later: Expose to the internet (Cloudflare Tunnel)

When you want access from outside your PC:

1. Get a free domain + Cloudflare account
2. Install `cloudflared`
3. Create a tunnel pointing to `http://localhost:4007`
4. Update these three values in `docker-compose.yaml`:
   - `MAIN_URL`
   - `FRONTEND_URL`
   - `NEXT_PUBLIC_BACKEND_URL`
5. Restart with `docker compose down && docker compose up -d`

I can help you with the exact Cloudflare steps later when you’re ready.

---

## Troubleshooting

- **Containers keep restarting** → Check RAM (need ≥ 4 GB free) and run `docker compose logs`
- **Port 4007 already in use** → Change the port mapping in `docker-compose.yaml` (e.g. `"4008:5000"`) and update the three URL variables
- **First start is slow** → Normal – images are large (~2–3 GB total)
- **Can’t register** → Make sure `DISABLE_REGISTRATION: 'false'` is set

---

## Files

- `docker-compose.yaml` – main configuration (JWT already set)
- `dynamicconfig/` – required by Temporal

Enjoy your free self-hosted social media scheduler!  
If you need help adding channels, Cloudflare, or n8n workflows, just ask.

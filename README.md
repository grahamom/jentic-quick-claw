# jentic-claw-stack

A one-command installer for a personal AI agent stack, secured by Tailscale.

You get a fully-configured [OpenClaw](https://openclaw.ai) agent with a local [Jentic Mini](https://github.com/jentic/jentic-mini) instance pre-wired and ready to use — plus Mattermost for team chat, and a web-based file browser for your agent's workspace. Everything runs in Docker, everything is private to your Tailscale network.

---

## Deploy

Pick a provider, create a fresh **Ubuntu 22.04/24.04** server with at least **2GB RAM**, SSH in as root, and run the one-liner below.

### One-line install

```bash
curl -fsSL https://raw.githubusercontent.com/jentic/jentic-quick-claw/main/install.sh | sudo bash
```

---

### ☁️ Providers

| Provider | Monthly cost | Recommended spec | Create server |
|---|---|---|---|
| **Hetzner** | ~€4 | CX22 (2 vCPU, 4 GB RAM) | [![Hetzner](https://img.shields.io/badge/Deploy_on-Hetzner-D50C2D?logo=hetzner&logoColor=white)](https://console.hetzner.cloud/projects) |
| **DigitalOcean** | ~$12 | Basic Droplet 2 GB | [![DigitalOcean](https://img.shields.io/badge/Deploy_on-DigitalOcean-0080FF?logo=digitalocean&logoColor=white)](https://cloud.digitalocean.com/droplets/new?image=ubuntu-22-04-x64&size=s-2vcpu-4gb) |
| **Linode / Akamai** | ~$12 | Linode 2 GB | [![Linode](https://img.shields.io/badge/Deploy_on-Linode_Akamai-02B159?logo=linode&logoColor=white)](https://cloud.linode.com/linodes/create) |
| **Vultr** | ~$12 | Regular Cloud Compute 2 GB | [![Vultr](https://img.shields.io/badge/Deploy_on-Vultr-007BFC?logo=vultr&logoColor=white)](https://my.vultr.com/deploy/) |
| **OVHcloud** | ~€4 | VPS Starter (2 GB) | [![OVHcloud](https://img.shields.io/badge/Deploy_on-OVHcloud-123F6D?logo=ovh&logoColor=white)](https://www.ovhcloud.com/en/vps/) |
| **Contabo** | ~€6 | Cloud VPS S (4 GB RAM) | [![Contabo](https://img.shields.io/badge/Deploy_on-Contabo-FF6600?logoColor=white)](https://contabo.com/en/vps/) |
| **UpCloud** | ~$10 | Simple 2 GB | [![UpCloud](https://img.shields.io/badge/Deploy_on-UpCloud-7B00D4?logoColor=white)](https://upcloud.com/products/cloud-servers/) |
| **AWS EC2** | ~$15 | t3.small (2 GB) | [![AWS](https://img.shields.io/badge/Deploy_on-AWS_EC2-232F3E?logo=amazon-aws&logoColor=white)](https://console.aws.amazon.com/ec2/v2/home#LaunchInstances:) |
| **Google Cloud** | ~$14 | e2-small (2 GB) | [![Google Cloud](https://img.shields.io/badge/Deploy_on-Google_Cloud-4285F4?logo=google-cloud&logoColor=white)](https://console.cloud.google.com/compute/instancesAdd) |
| **Azure** | ~$17 | B1ms (2 GB) | [![Azure](https://img.shields.io/badge/Deploy_on-Azure-0078D4?logo=microsoft-azure&logoColor=white)](https://portal.azure.com/#create/Microsoft.VirtualMachine-ARM) |

> **Tip:** Hetzner is the best value for most users. DigitalOcean has the most beginner-friendly UI.

---

### ⚡ One-click cloud-init (automated, no SSH needed)

Most providers let you paste a **User Data** script when creating a server. This script runs automatically on first boot — no SSHing in required.

> **Requires:** A [Tailscale auth key](https://login.tailscale.com/admin/settings/keys) (reusable, from your Tailscale admin settings). Without it, Tailscale needs an interactive browser step.

Paste this into the **User Data / Cloud-Init** field when creating your server:

```bash
#!/bin/bash
export TS_AUTHKEY="tskey-auth-REPLACE_ME"   # ← paste your auth key here
curl -fsSL https://raw.githubusercontent.com/jentic/jentic-quick-claw/main/install.sh | bash
```

#### Where to find User Data on each provider

<details>
<summary><strong>Hetzner</strong></summary>

1. Go to [Hetzner Cloud Console](https://console.hetzner.cloud/) → **New Server**
2. OS: Ubuntu 22.04
3. Type: **CX22** (2 vCPU, 4 GB RAM, ~€4/month)
4. Scroll to **Cloud config** → paste the User Data script above
5. Add your SSH key → **Create & Buy now**

> No SSH step needed — the agent installs itself and joins your Tailscale network automatically.

</details>

<details>
<summary><strong>DigitalOcean</strong></summary>

1. Go to [Create Droplet](https://cloud.digitalocean.com/droplets/new?image=ubuntu-22-04-x64&size=s-2vcpu-4gb) (link pre-fills Ubuntu 22.04 + 2 GB)
2. Scroll to **Advanced Options** → enable **Add Initialization scripts (free)**
3. Paste the User Data script above
4. Add your SSH key → **Create Droplet**

</details>

<details>
<summary><strong>Linode / Akamai</strong></summary>

1. Go to [Create Linode](https://cloud.linode.com/linodes/create)
2. Image: Ubuntu 22.04 LTS
3. Plan: **Linode 2 GB** (~$12/month)
4. Scroll to **Advanced Options** → **Add User Data**
5. Paste the User Data script above
6. Add your SSH key → **Create Linode**

</details>

<details>
<summary><strong>Vultr</strong></summary>

1. Go to [Deploy](https://my.vultr.com/deploy/)
2. Server type: **Cloud Compute — Regular Performance**
3. OS: Ubuntu 22.04
4. Plan: 2 GB / 1 vCPU (~$12/month)
5. Scroll to **Server Settings** → **User Data**
6. Paste the User Data script above
7. **Deploy Now**

</details>

<details>
<summary><strong>OVHcloud</strong></summary>

1. Go to [VPS order](https://www.ovhcloud.com/en/vps/) → order **Starter** or **Value** (≥2 GB RAM)
2. During configuration, select **Ubuntu 22.04**
3. In the **Script / User Data** field, paste the User Data script above
4. Complete the order

> OVHcloud may require a post-delivery SSH step if cloud-init is not available on your chosen template. Check their console for first-boot output.

</details>

<details>
<summary><strong>AWS EC2</strong></summary>

1. Go to [Launch Instance](https://console.aws.amazon.com/ec2/v2/home#LaunchInstances:)
2. AMI: **Ubuntu Server 22.04 LTS (64-bit x86)**
3. Instance type: **t3.small** (2 GB RAM, ~$15/month)
4. Under **Advanced details** → **User data** → paste the User Data script above
5. Configure a security group: allow **SSH (port 22)** from your IP, and **UDP 41641** for Tailscale
6. Launch with your key pair

> Everything else (app ports) is locked down by the installer's iptables rules — Tailscale is the only access route.

</details>

<details>
<summary><strong>Google Compute Engine</strong></summary>

1. Go to [Create VM](https://console.cloud.google.com/compute/instancesAdd)
2. Machine family: **E2** → **e2-small** (2 vCPU shared, 2 GB RAM, ~$14/month)
3. Boot disk: **Ubuntu 22.04 LTS**
4. Expand **Advanced options** → **Management** → **Automation** → **Startup script**
5. Paste the User Data script above
6. **Create**

Alternatively, use the **Cloud Shell** button to open a terminal and SSH into a new instance yourself:

[![Open in Cloud Shell](https://gstatic.com/cloudssh/images/open-btn.svg)](https://shell.cloud.google.com/cloudshell/open?git_repo=https://github.com/jentic/jentic-quick-claw)

</details>

<details>
<summary><strong>Azure</strong></summary>

1. Go to [Create a virtual machine](https://portal.azure.com/#create/Microsoft.VirtualMachine-ARM)
2. Image: **Ubuntu Server 22.04 LTS**
3. Size: **B1ms** (1 vCPU, 2 GB RAM, ~$17/month) or **B2s** for more headroom
4. Under **Advanced** → **Custom data** → paste the User Data script above
5. Inbound ports: allow **SSH (22)** — the installer locks everything else down via iptables
6. **Review + create**

</details>

---

## What You Get

| Service | What it is |
|---|---|
| **OpenClaw** | Your personal AI agent. Chat with it via a web UI or Mattermost. Connects to any LLM (Anthropic, OpenAI, etc.). Runs 24/7. |
| **Jentic Mini** | A local API execution engine. Gives your agent access to hundreds of real-world APIs (GitHub, Slack, Stripe, Gmail, and more) without you managing credentials per-tool. |
| **Mattermost** | A self-hosted team chat server. Your agent has a bot account and is connected automatically. Chat with your agent like you would on Slack. |
| **Filebrowser** | A simple web UI to browse and edit your agent's workspace files — memory, notes, config. |

All services are only reachable on your **Tailscale network** (your private tailnet). Nothing is exposed to the public internet.

---

## What You Need Before Starting

### 1. A server

A fresh Ubuntu 22.04 or 24.04 VPS with at least **2GB RAM**. See the provider table above.

### 2. A Tailscale account

[Sign up free at tailscale.com](https://tailscale.com). The free tier supports up to 100 devices.

After signing up, do two things in the [Tailscale admin console](https://login.tailscale.com/admin/dns):
1. Enable **MagicDNS** — gives your server a stable hostname like `my-agent.tail-xxxx.ts.net`
2. Enable **HTTPS Certificates** — lets the installer get a real TLS cert for your server

If you want to use the automated cloud-init deploy, also create a [reusable auth key](https://login.tailscale.com/admin/settings/keys).

### 3. An AI provider API key

OpenClaw works with most major LLM providers. You'll need one:
- [Anthropic](https://console.anthropic.com) — Claude (recommended)
- [OpenAI](https://platform.openai.com) — GPT-4 / o-series
- [Google](https://aistudio.google.com) — Gemini

You enter this in the OpenClaw web UI after the install — not during the script.

### 4. Tailscale on your laptop/phone

Install the Tailscale client on the device you'll use to connect:
- [macOS](https://tailscale.com/download/mac) · [Windows](https://tailscale.com/download/windows) · [iOS](https://tailscale.com/download/ios) · [Android](https://tailscale.com/download/android)

Sign in with the same Tailscale account you used above.

---

## Install (manual / interactive)

SSH into your server as root, then run:

```bash
curl -fsSL https://raw.githubusercontent.com/jentic/jentic-quick-claw/main/install.sh | sudo bash
```

The script will:

1. Install Docker and other dependencies
2. Add a 2GB swapfile
3. Ask you to name your machine (default: `claw-stack`)
4. Walk you through Tailscale authentication (a URL will appear — open it in your browser)
5. Request a TLS certificate automatically (requires MagicDNS + HTTPS Certificates enabled)
6. Pull all Docker images and start the stack
7. Bootstrap Mattermost — create an admin user, a bot account, and wire the token into OpenClaw
8. Lock down the firewall — only SSH and Tailscale traffic allowed in
9. Wait for you to open the auth link and approve device pairing

**Total time: ~5–10 minutes.**

At the end, you'll see something like:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ✔ Stack is up and ready!
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  🐾 OpenClaw:     https://claw-stack.tail-xxxx.ts.net
  ⚡ Jentic Mini:  https://claw-stack.tail-xxxx.ts.net:8900
  📁 Filebrowser:  https://claw-stack.tail-xxxx.ts.net:8080
  💬 Mattermost:   https://claw-stack.tail-xxxx.ts.net:8065

  Click to authenticate:
  https://claw-stack.tail-xxxx.ts.net/?token=361357c19ee411f3...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Mattermost credentials:
    Username: admin
    Password: <generated>
```

---

## First-Time Setup

### Step 1: Authenticate with OpenClaw

Make sure Tailscale is running on your device, then click the authentication link from the installer output. Your device is paired and you're in.

> To retrieve the token later:
> ```bash
> python3 -c "import json; print(json.load(open('/opt/claw/openclaw-config/openclaw.json'))['gateway']['auth']['token'])"
> ```

### Step 2: Add Your AI Provider Key

Once in the OpenClaw UI, configure a model by entering your API key. Your agent will start up and introduce itself.

### Step 3: Connect to Mattermost

Open Mattermost at `https://claw-stack.tail-xxxx.ts.net:8065`. Log in with the admin credentials printed at the end of the install. Your agent's bot (`@claw-agent`) is already in the `claw` team and connected to OpenClaw.

### Step 4: Install the Jentic Skill

Tell your agent:

> "Install the Jentic skill"

Or run in the OpenClaw terminal:

```
clawhub install jentic
```

When asked for a URL, enter: `http://jentic-mini:8900`

### Step 5: Explore the Workspace

Open Filebrowser at `https://claw-stack.tail-xxxx.ts.net:8080` to browse your agent's memory and config files. No login required — Tailscale handles authentication.

---

## Architecture

```
Your device (Tailscale)
        │
        │  HTTPS (Tailscale cert)
        ▼
┌──────────────────────────────────┐
│  Caddy (reverse proxy)           │
│  :443  → OpenClaw                │
│  :8900 → Jentic Mini             │
│  :8080 → Filebrowser             │
│  :8065 → Mattermost              │
└──────────────┬───────────────────┘
               │ Docker network
    ┌──────────┼────────────┬──────────┐
    ▼          ▼            ▼          ▼
OpenClaw  Jentic Mini  Filebrowser  Mattermost
:18789      :8900          :80        :8065
    │                                   │
    └── workspace/ ─────────────────────┘
         (host-mounted)           Postgres
                                   :5432
```

- **Firewall**: iptables DOCKER-USER chain blocks all public access to container ports. Only Tailscale traffic (`tailscale0`) and container-to-container traffic are allowed through.
- **Caddy** terminates TLS with a Tailscale-issued certificate and proxies to each service on the Docker bridge network.
- **OpenClaw** connects to Mattermost via its bot token — chat with your agent directly in Mattermost channels.
- The **workspace directory** (`/opt/claw/workspace`) is mounted into both OpenClaw and Filebrowser.

---

## Managing the Stack

```bash
cd /opt/claw

# View running containers
docker compose ps

# View logs
docker compose logs -f openclaw
docker compose logs -f mattermost

# Restart a service
docker compose restart openclaw

# Stop / start everything
docker compose down
docker compose up -d
```

### Updating

```bash
cd /opt/claw
docker compose pull          # pull latest images for all services
docker compose up -d         # recreate updated containers
```

### Renewing TLS Certificates

Tailscale certificates are valid for ~90 days. To renew:

```bash
TS_DNS=$(tailscale status --json | python3 -c "import json,sys; print(json.load(sys.stdin)['Self']['DNSName'].rstrip('.'))")
tailscale cert --cert-file /opt/claw/certs/cert.pem --key-file /opt/claw/certs/key.pem "$TS_DNS"
docker compose restart caddy
```

---

## Troubleshooting

**Can't reach the services**
- Make sure Tailscale is running on your device: `tailscale status`
- Check containers: `cd /opt/claw && docker compose ps`

**OpenClaw keeps restarting / OOM**
```bash
swapon --show
# If empty:
fallocate -l 2G /swapfile && chmod 600 /swapfile && mkswap /swapfile && swapon /swapfile
```

**Lost the Gateway Token**
```bash
python3 -c "import json; print(json.load(open('/opt/claw/openclaw-config/openclaw.json'))['gateway']['auth']['token'])"
```

**Mattermost bot not responding**
```bash
docker logs mattermost
# Check the bot token is in openclaw.json:
python3 -c "import json; print(json.load(open('/opt/claw/openclaw-config/openclaw.json'))['channels']['mattermost']['token'])"
```

---

## What's Next

- **Connect APIs via Jentic** — ask your agent to search Jentic for tools: Gmail, GitHub, Slack, Stripe, and more
- **Set up mobile access** — connect WhatsApp or Telegram so you can chat with your agent from your phone
- **Add skills** — browse [clawhub.com](https://clawhub.com) for community-built agent skills
- **Shape your agent** — edit `SOUL.md` and `AGENTS.md` in Filebrowser to define your agent's personality

---

## Credits

- [OpenClaw](https://openclaw.ai) — the agent runtime
- [Jentic](https://jentic.com) — the API execution layer
- [Mattermost](https://mattermost.com) — self-hosted team chat
- [Tailscale](https://tailscale.com) — the network security layer
- [Caddy](https://caddyserver.com) — the reverse proxy
- [Filebrowser](https://filebrowser.org) — the workspace file UI

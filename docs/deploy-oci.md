# Deploying Lexical to Oracle Cloud (Always Free)

One small VM runs everything in Docker:
- **Caddy** handles HTTPS on ports 80/443.
- **The game server**, written in Rust, uses SQLite on a Docker volume.
- **The web app** runs on Next.js.

The browser only ever talks to `https://<your domain>`.

## 1. Create the VM (Oracle Cloud console)

1. Create an Oracle Cloud account. The Always Free resources are free even after the trial ends.
2. **Compute → Instances → Create instance.**
   - **Image:** Canonical Ubuntu 24.04.
   - **Shape:** Ampere, `VM.Standard.A1.Flex`, with **2 OCPU and 12 GB** RAM. The free allowance is 4 OCPU / 24 GB in total, so this leaves room for a second VM.
   - **Networking:** create a new VCN with a public subnet, and assign a public IPv4 address.
   - **SSH keys:** upload your public key (`~/.ssh/id_ed25519.pub`).
   - **Boot volume:** 50 GB is plenty. Up to 200 GB is free.
   - "Out of capacity" errors for A1 are common. Try another availability domain, or try again later. Switching the account to Pay-As-You-Go makes capacity much easier to get and still costs nothing within the free limits.
3. Note the instance's **public IP**.
4. **Open the ports in the cloud firewall:** go to Networking → Virtual cloud networks → your VCN → Security Lists → Default → **Add ingress rules**:
   - Source `0.0.0.0/0`, TCP, destination port **80**
   - Source `0.0.0.0/0`, TCP, destination port **443**
   - Source `0.0.0.0/0`, UDP, destination port **443** (HTTP/3, optional)

## 2. Point a domain at it

- **With a domain:** add an `A` record, e.g. `play.yourdomain.com` → the public IP.
- **Without a domain:** use `<ip-with-dashes>.sslip.io`, e.g. `140-238-1-2.sslip.io`. It resolves to that IP automatically, and Let's Encrypt issues certificates for it.

## 3. Prepare the VM (once)

```bash
scp deploy/oci-setup.sh ubuntu@<ip>:
ssh ubuntu@<ip> 'bash oci-setup.sh'
```

This script:
- installs Docker
- opens 80/443 in the VM's own firewall. Oracle's Ubuntu images block everything except SSH in iptables, as well as in the security list.
- adds swap and turns on automatic security updates
- creates `/opt/lexical`

Log out and back in afterwards.

## 4. Production settings

On your laptop, in the repo root:

```bash
cp .env.example .env.production
```

Fill it in. `.env.production` is gitignored and never leaves your laptop except to go to the VM.

| Setting | Value |
|---|---|
| `DOMAIN` | `play.yourdomain.com`, or the `sslip.io` name |
| `ANTHROPIC_API_KEY`, `ELEVENLABS_API_KEY`, `GROQ_API_KEY`, … | your keys |
| `ADMIN_EMAILS` | your email: sign up with it to become admin |
| `INVITE_ONLY` | `true` for the closed beta |
| `COURSES` | `ja,en` (or just `ja`) |
| `MAX_TURNS_PER_DAY` | per-tester daily cap, e.g. `150` |

`CORS_ORIGINS`, `PUBLIC_WEB_URL` and `TRUST_PROXY` are set for you by `compose.prod.yml`.

## 5. Deploy

```bash
HOST=ubuntu@<ip> scripts/deploy.sh --env   # first time: uploads .env.production as the VM's .env
HOST=ubuntu@<ip> scripts/deploy.sh         # later: code only
```

The first build takes a while on the VM; Rust in particular. Caddy fetches the certificate on the first request. Then:

1. Open `https://<domain>` and **Join the beta** with your `ADMIN_EMAILS` address. No invite is needed for that address.
2. Open `https://<domain>/admin`, create invites, and send testers the links.

## 6. Backups

Install the nightly database snapshot once on the VM:

```bash
ssh ubuntu@<ip> '(crontab -l 2>/dev/null; echo "30 3 * * * /opt/lexical/deploy/backup.sh >> /opt/lexical/backups/backup.log 2>&1") | crontab -'
```

Snapshots land in `/opt/lexical/backups/` and are kept for 14 days. To copy them off the VM:

```bash
rsync -az ubuntu@<ip>:/opt/lexical/backups/ ./backups/
```

## Everyday commands (on the VM, in /opt/lexical)

| | |
|---|---|
| Status | `docker compose -f compose.prod.yml ps` |
| Logs | `docker compose -f compose.prod.yml logs -f server` |
| Restart | `docker compose -f compose.prod.yml restart server` |
| Render music/SFX (paid ElevenLabs plan) | `docker compose -f compose.prod.yml exec server lexical-server generate-audio` |
| Manual backup | `deploy/backup.sh` |

## Small VM without Docker (the free 1 GB Micro)

If Ampere capacity isn't available, Lexical also runs on the free `VM.Standard.E2.1.Micro` (1 OCPU, 1 GB). It's too small to build there, so everything is built on your laptop and shipped:

```bash
scp deploy/native/setup.sh ubuntu@<ip>:/tmp/ && ssh ubuntu@<ip> 'sudo bash /tmp/setup.sh'   # once
HOST=ubuntu@<ip> DOMAIN=<ip-with-dashes>.sslip.io scripts/deploy-native.sh --env            # every deploy
```

- `deploy-native.sh` cross-compiles the server with `cargo-zigbuild` (`brew install zig && cargo install cargo-zigbuild`), builds the web app for `https://$DOMAIN`, and installs both as systemd services behind Caddy.
- On the VM: `sudo systemctl status lexical-server lexical-web caddy`, logs with `sudo journalctl -u lexical-server -f`.
- Backups: `deploy/native/backup.sh` runs nightly from `/etc/cron.d/lexical-backup` into `/opt/lexical/backups`.

Current beta: **https://129-225-95-70.sslip.io** (VM `lexical-beta`, Hyderabad).

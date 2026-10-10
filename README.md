# AgentStory

AgentShare is a local-first manager and reader for AI coding-agent sessions.
Desktop discovers Codex and Claude Code history without modifying the original
JSONL files. Sharing is explicit: a reviewed, sanitized immutable snapshot is
uploaded to the versioned Go API and read at `/t/:slug`.

## Downloads

Download the latest desktop release from the [AgentStory Releases page](https://github.com/jewdore/agentstory/releases/latest).

## Architecture

```text
Codex JSONL ??
             ?? TypeScript adapter ? Unified AgentSession ??
Claude JSONL ?                                             ?? Desktop SQLite
                                                           ?? Go API / PostgreSQL
                                                                ?? Next.js Web
                                                                ?? future iOS / Android
```

- `packages/session-core`: versioned model, Codex/Claude adapters, normalization, secret scanner.
- `packages/api-client`: one API client shared by Desktop, Web, and future native clients.
- `packages/ui`: dark reader, virtualized transcript, code/diff/tool rendering.
- `apps/desktop`: Tauri 2 + React + Rust scanner + SQLite index + tray.
- `apps/web`: Next.js public reader, login, cloud library, and same-origin API proxy.
- `server`: Go API, PostgreSQL migrations, ownership checks, immutable snapshots, email OTP auth.

## Local development

Prerequisites: Node 23+, pnpm 10+, Go 1.23+, Rust 1.97+, and PostgreSQL 17+
(or Docker Desktop).

```powershell
pnpm install
Copy-Item .env.example .env

# Start PostgreSQL and the API (Docker)
docker compose up --build postgres api

# In separate terminals
pnpm dev:web
pnpm --filter @agentshare/desktop dev
```

Open `http://127.0.0.1:3000` for Web and use the Tauri command below for the
native Desktop shell:

```powershell
pnpm --filter @agentshare/desktop tauri dev
```

## Deploy to `dev`

`deploy.sh` deploys Web + Go API + PostgreSQL through the `dev` SSH alias in
`~/.ssh/config`. Desktop remains a local application; it is not installed on the
server. No local agent history is scanned or uploaded by this script.

Prerequisites: Bash, SSH/SCP, tar, pnpm and Go locally; Docker Compose v2 with
`up --wait`, Bash, Python 3, `flock`, and registry access remotely. SSH host keys
must already be trusted. On Windows use **Git Bash**, not the Windows WSL Bash
launcher, so the script reuses your Windows SSH configuration and keys.

### Private development machine

The first command creates random credentials on the server, enables development
sign-in explicitly, and binds Web/API **only to server loopback**:

```bash
bash deploy.sh --init-dev --allow-dev-auth
# Subsequent deployments reuse credentials and the persistent database:
bash deploy.sh --allow-dev-auth
```

Web uses port 3080 and API uses 8088 for this profile, avoiding the existing
3000/5432 services on `dev`. PostgreSQL has no published host port. Access through
an SSH tunnel (leave this command running):

```bash
ssh -N -L 3080:127.0.0.1:3080 -L 8088:127.0.0.1:8088 dev
```

Open `http://localhost:3080`; the development sign-in screen displays the test
verification code. Desktop can use `http://localhost:8088`. These are development
URLs, **not public share links**. Never expose this development-auth profile to
the network. `--init-dev` does not overwrite an existing shared environment.

### Production

```bash
cp .env.production.example .env.production
# Edit credentials, HTTPS origin, SMTP, ports, and ALLOWED_ORIGINS.
# Generate independent URL-safe secrets, for example: openssl rand -hex 32
bash deploy.sh --env-file .env.production
# Subsequent deployments preserve /opt/agentshare/shared/.env:
bash deploy.sh
```

Production refuses placeholder credentials, development authentication, missing
SMTP configuration, and non-HTTPS origins. Configure your existing HTTPS reverse
proxy to forward Web traffic to the configured loopback Web port; domain/DNS,
TLS certificates and SMTP credentials are not provisioned by this script.
`POSTGRES_PASSWORD` must contain at least 24 URL-safe characters (`A-Z a-z 0-9 _ -`)
and `AUTH_PEPPER` at least 32 random characters. Use ports not already occupied.
Set `ALLOWED_ORIGINS` to include the real Web origin and any required Desktop
origins. Changing the database password requires a separate database operation;
changing only the environment file is deliberately rejected.

### Checks, releases, and recovery

```bash
bash deploy.sh --dry-run --skip-checks  # package only, no SSH
bash deploy.sh --host dev --root /opt/agentshare --keep-releases 5
bash deploy.sh --skip-checks --allow-dev-auth  # only after running checks yourself
python -m unittest discover -s scripts -p 'test_*.py'
bash scripts/test-deploy.sh
```

By default the script runs lint, typecheck, core tests, Go tests, and frontend
builds. Archives exclude dependencies, builds, dotenv files, databases and common
key files; nevertheless, keep arbitrary secrets out of source code.

- `/opt/agentshare/releases/<timestamp-suffix>` holds each source release and
  its private environment snapshot (mode 600).
- `/opt/agentshare/current` changes only after all containers pass health checks.
- A deployment lock prevents concurrent activation. Web/API image tags are
  unique per release, allowing rollback without rebuilding older source.
- Failed builds leave running services untouched. Failed startup attempts restore
  the previous containers, or remove the first-attempt containers without deleting
  the database volume. First-use database credentials are retained for retries.
- The five newest successful source releases are retained; failed releases remain
  for diagnosis. No global Docker prune is run, and image cleanup is manual.
- Container rollback does **not** reverse database migrations. Back up PostgreSQL
  before upgrades and keep migrations compatible with the previous application.

Inspect status/logs (logs can contain sensitive data):

```bash
ssh dev 'cd /opt/agentshare/current && docker compose -p agentshare --env-file .env ps'
ssh dev 'cd /opt/agentshare/current && docker compose -p agentshare --env-file .env logs --tail=100 api web'
```

Use `deploy.sh` for updates rather than bare `docker compose up`: the script
selects the version-specific image tags. `dev` currently has an old Linux kernel;
Node and PostgreSQL use Debian-based images because Alpine image writes failed
with `EPERM` on that host. The script does not disable container security policies.

## Validation

```powershell
pnpm lint
pnpm typecheck
pnpm test
pnpm build
go -C server test ./...
cargo test --manifest-path apps/desktop/src-tauri/Cargo.toml --lib
cargo clippy --manifest-path apps/desktop/src-tauri/Cargo.toml --all-targets -- -D warnings
```

The PostgreSQL lifecycle test is opt-in and runs against an isolated schema:

```powershell
$env:TEST_DATABASE_URL='postgres://agentshare:agentshare@127.0.0.1:5432/agentshare?sslmode=disable'
go -C server test ./internal/store
```

## Safety boundaries

- Original `~/.codex` and `~/.claude` files are read-only to AgentShare.
- Local annotations live under `~/.agentshare/agentshare.db`.
- Desktop and API secret scanners run before upload; the API repeats scanning.
- Share URLs point to immutable cleaned snapshots, not live local sessions.
- Private, unlisted, and public visibility are modeled from V1.
- API responses are business data, not UI-specific card structures.

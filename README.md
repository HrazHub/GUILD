# GUILD

**Your Community. Your Space.**

GUILD is a Windows desktop community platform by **HrazHub** (Electron + React + TypeScript + Vite + Tailwind frontend, Node/Express/Socket.IO backend, PostgreSQL + Prisma). It provides servers/channels, real-time text chat, DMs/groups, friends, roles/permissions, notifications, search, settings, and WebRTC voice/video/screen-share foundations.

> **Packaging status (this workspace):** builds and typechecks are prepared, but producing `GUILD-Release/GUILD-Setup.exe` requires sufficient free disk space and a working packaging environment. A previous packaging attempt failed with `ENOSPC` on `C:` during electron-builder resource copy (embedded PostgreSQL binaries are large). Free disk space and rerun `npm run package:win`.

## Features

- **Accounts & auth:** register/login/logout, opaque Bearer sessions, email verification and password reset flows (real SMTP delivery requires configuration).
- **Servers:** create/manage servers, categories/channels (text/voice), invites, public discovery/join, bans, ownership transfer, icon uploads.
- **Chat:** real-time messages via Socket.IO, edits/deletes, replies, reactions, attachments, pins, channel history/read state, search.
- **DMs & groups:** direct and group conversations with membership controls.
- **Social:** friend requests/accept/decline/remove, blocking.
- **Permissions:** role-based permissions enforced on the backend (channel visibility, moderation, messaging, etc.).
- **Voice/video:** WebRTC mesh signaling + configurable ICE servers; **TURN must be configured externally** for reliable NAT traversal.
- **Desktop shell:** custom title bar, tray/notifications integration points, safe token storage, auto-update wiring (requires hosted feed).

## Repository layout

- `apps/web` — React/Vite client
- `apps/server` — Express/Socket.IO API, Prisma, embedded PostgreSQL runtime
- `apps/desktop` — Electron shell + packaging config
- `packages/shared` — shared types/constants
- `scripts/` — build, icon, and packaging scripts
- `installer/guild.iss` — Inno Setup installer definition
- `docs/` — setup/build/testing/deployment/security/API/release notes

## Quick start (development)

Prerequisites: Node.js (v24+ recommended in this workspace), npm. No system PostgreSQL required for local embedded mode.

```powershell
npm install
# Terminal 1: backend (uses embedded PostgreSQL under GUILD_DATA_DIR)
$env:GUILD_DATA_DIR="$PWD\.devdata"; $env:PORT='8787'; $env:NODE_ENV='development'
npm run dev -w @guild/server
# Terminal 2: web
npm run dev -w @guild/web
```

Build all: `npm run build`  
Typecheck: `npm run typecheck`  
Tests: `npm test -w @guild/server` (see `docs/TESTING.md`)

## Packaging (Windows)

```powershell
npm run package:win
```

Outputs (on success): `release/win-unpacked/` and `GUILD-Release/GUILD-Setup.exe`. Requires Inno Setup 6 (`ISCC.exe`). See `docs/BUILD.md` and `docs/DEPLOYMENT.md`.

## External infrastructure required

- **SMTP** for production email delivery (verification/reset). Without it, delivery cannot be claimed.
- **TURN server** for reliable voice/video across restrictive networks.
- **Hosted backend/database** for multi-user internet deployment (embedded DB is local).
- **Hosted update feed** for Electron auto-updates.

See `docs/SETUP.md`, `docs/SECURITY.md`, and `.env.example`.

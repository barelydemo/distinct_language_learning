# Lexical: Lost in Japan

**Lexical** is a voice-first visual novel for learning Japanese through conversation.

The player wakes in Narita after losing their passport, wallet, and phone. Every conversation, reply, and trip across the map becomes part of a larger story—and a practical language lesson. Players can speak, type, or choose suggested replies, while an AI director controls the characters and scene.

## What you can do

- **Speak or type Japanese** using push-to-talk, or keep the microphone optional.
- **Choose a suggested reply** when you want a fast fallback.
- **Explore an open world** by following scene exits and moving across the map.
- **Progress through a lesson path** based on the Genki I curriculum.
- **Receive coaching** on grammar and phrasing as the story continues.
- **Experience reactive scenes** with animated characters, transitions, emotes, music, ambience, and sound effects.
- **Save and resume progress** through player accounts or guest play, depending on configuration.
- **Play two course directions**: Japanese for English speakers or English for Japanese speakers.

## How it works

| Layer | Technology | Responsibility |
|---|---|---|
| Web app | Next.js 16, React 19, TypeScript | Stage, dialogue, map, settings, accounts, and microphone controls |
| Game server | Rust, Axum, Tokio | Turns, authentication, saves, progression rules, and API routes |
| Database | SQLite | Player accounts, runs, progress, invites, and state |
| Story data | TOML | Cast, scenes, places, quests, courses, and lesson definitions |
| AI director | Claude, Groq, DeepSeek, or mock | Generates character dialogue and structured scene commands |
| Voice | ElevenLabs or Groq | Speech recognition and/or character speech |

The web app sends a player turn to the server. The selected AI provider proposes dialogue, stage cues, coaching, and world-state changes. The Rust server validates those proposals before applying them—for example, limiting speakers to characters at the current place and allowing movement only through configured exits.

```text
Browser
  ├── Speech / text / suggested reply
  ├── Next.js stage and interface
  └── /api/v1 requests
         │
         ▼
Rust/Axum server
  ├── Authentication and run state
  ├── Curriculum and movement validation
  ├── AI director
  └── SQLite
```

## Quick start

### Requirements

- Rust stable
- Node.js 24 or newer
- npm

### 1. Start the server

The mock provider works without API keys and is useful for local UI development.

```bash
cd server
CLAUDE_MOCK=1 cargo run
```

The API server starts at `http://localhost:8080`.

### 2. Start the web app

Open a second terminal:

```bash
cd web
npm install
npm run dev
```

Open `http://localhost:3000`.

## Run with Docker

This is the simplest production-like local setup.

```bash
cp .env.example .env
docker compose up --build
```

The web app is available at `http://localhost:3000`, and the API is available at `http://localhost:8080`.

To stop the stack:

```bash
docker compose down
```

Persistent SQLite data is stored in the `lexical-data` Docker volume.

## Add AI and voice providers

Copy the example environment file, then add the services you want:

```bash
cp .env.example server/.env
```

The server loads `server/.env` in native mode. Docker Compose reads `.env` from the repository root.

### Director providers

Set one primary provider:

| Provider | Environment settings |
|---|---|
| Claude | `LLM_PROVIDER=claude`, `ANTHROPIC_API_KEY` |
| Groq | `LLM_PROVIDER=groq`, `GROQ_API_KEY` |
| DeepSeek | `LLM_PROVIDER=deepseek`, `DEEPSEEK_API_KEY` |
| Mock/offline | `LLM_PROVIDER=mock` or `CLAUDE_MOCK=1` |

Use `LLM_FALLBACK` to retry a failed turn with another provider, for example:

```dotenv
LLM_PROVIDER=claude
LLM_FALLBACK=groq
ANTHROPIC_API_KEY=...
GROQ_API_KEY=...
```

### Speech and audio

Set `ELEVENLABS_API_KEY` for character voices, ElevenLabs speech-to-text, and generated music or effects.

Alternatively, set `GROQ_API_KEY` and use Groq Whisper:

```dotenv
STT_PROVIDER=groq
GROQ_STT_MODEL=whisper-large-v3-turbo
```

To disable speech-to-text:

```dotenv
STT_PROVIDER=none
```

Voice is optional. Without a speech provider, players can still type or select replies.

### Render optional generated audio

Generated music, ambience, and effects can be rendered once and reused from disk:

```bash
cd server
cargo run -- generate-audio
```

Re-render existing assets with:

```bash
cargo run -- generate-audio --force
```

With Docker:

```bash
docker compose run --rm generate-audio
```

## Configuration

Start with [`.env.example`](.env.example), which contains provider-specific defaults and comments.

### Core settings

| Variable | Default | Purpose |
|---|---|---|
| `LLM_PROVIDER` | `claude` | Director provider: `claude`, `groq`, `deepseek`, or `mock` |
| `LLM_FALLBACK` | unset | Optional fallback provider |
| `STT_PROVIDER` | auto-detected | Speech-to-text provider: `elevenlabs`, `groq`, or `none` |
| `COURSES` | `ja,en` | Course IDs loaded from `server/content/courses` |
| `DATA_DIR` | `data` | Database and cached/generated assets directory |
| `DATABASE_URL` | `sqlite://data/lexical.db` | SQLite connection string |
| `CORS_ORIGINS` | `http://localhost:3000` | Allowed web origins, separated by commas |
| `MAX_TURNS_PER_DAY` | `400` | Maximum AI turns allowed per player each day |
| `ALLOW_GUESTS` | `false` | Allow play without an account |
| `INVITE_ONLY` | `true` | Require an invite for account registration |
| `ADMIN_EMAILS` | unset | Comma-separated emails granted administrator access |
| `SESSION_DAYS` | `30` | Account session lifetime |
| `PUBLIC_WEB_URL` | `http://localhost:3000` | Public web URL used for links and invites |
| `NEXT_PUBLIC_API_URL` | `http://localhost:8080` | Browser-visible API URL at web build time |

Provider-specific model and key settings are documented in [`.env.example`](.env.example).

## Repository layout

```text
web/                 Next.js game interface
  app/               Routes and page layout
  components/        Stage, HUD, dialogue, settings, and admin UI
  lib/               API, types, audio, and state helpers
  public/            Character art, scenes, and static assets

server/              Rust/Axum game server
  content/courses/   Japanese and English course story files
  migrations/        SQLite schema migrations
  src/               API, director, game rules, database, voice, and audio

deploy/              Caddy and native deployment configuration
scripts/             Build and deployment utilities
docs/                Deployment and asset/content briefs
```

## Writing and editing courses

Course content lives in:

- [`server/content/courses/ja/story.toml`](server/content/courses/ja/story.toml)
- [`server/content/courses/en/story.toml`](server/content/courses/en/story.toml)

The server validates the story files at startup. Invalid scene, speaker, item, or grammar references should fail fast rather than reach a player.

The TOML format contains:

| Section | Purpose |
|---|---|
| `[[cast]]` | Characters, profiles, colors, levels, sprites, and voice settings |
| `[audio]` | Music, ambience, and sound-effect prompts |
| `[[scenes]]` | Scene backgrounds and visual properties |
| `[[lessons]]` | Ordered curriculum lessons and target grammar |
| `[[places]]` | Locations, exits, characters, events, and objectives |
| `[[quests]]` | Larger story arcs composed from story flags |
| `[[triggers]]` | World events that can fire automatically |
| `[[endings]]` | Conditions that close a story |

### Adding backgrounds

Place a 16:9 scene image at:

```text
web/public/scenes/<scene-id>.jpg
```

Missing images fall back to the vector backdrop renderer.

### Adding character expressions

Place expression art at:

```text
web/public/characters/<character-id>/<expression>.png
```

List the expression or gesture in the character's `poses` array in the course TOML.

If no per-expression art is available, the base character sprite is reused and the emotion is expressed through stage emotes.

### Adding stage cues

Stage commands are defined in [`server/src/story.rs`](server/src/story.rs) and mirrored in [`web/lib/types.ts`](web/lib/types.ts). Adding or changing a cue may require updates in both files and in the matching component under [`web/components/stage/`](web/components/stage/).

## API overview

All game routes are under `/api/v1`.

| Method and route | Purpose |
|---|---|
| `GET /api/v1/story` | Public course/story data |
| `POST /api/v1/players` | Create a guest player |
| `POST /api/v1/runs` | Create a new game run |
| `GET /api/v1/runs/current` | Load the latest run |
| `POST /api/v1/runs/{id}/turn` | Submit speech, text, or a suggested reply |
| `POST /api/v1/runs/{id}/travel` | Travel to an open exit |
| `POST /api/v1/runs/{id}/scan` | Look up a dialogue line |
| `POST /api/v1/runs/{id}/hint` | Use battery to unlock a suggested reply |
| `POST /api/v1/tts` | Generate or retrieve character speech |
| `POST /api/v1/stt` | Transcribe microphone audio |
| `GET /api/v1/audio` | List rendered audio assets |
| `GET /api/v1/audio/{kind}/{id}.mp3` | Download a catalogued audio asset |

Routes that modify player state require an authorization bearer token.

## Development checks

Run the web checks from `web/`:

```bash
npm run typecheck
npm run lint
npm run build
```

Run server checks from `server/`:

```bash
cargo fmt --check
cargo test
cargo check
```

## Deployment

Lexical can run in Docker with Caddy and automatic HTTPS.

- [`compose.prod.yml`](compose.prod.yml) defines the production Docker stack.
- [`deploy/Caddyfile`](deploy/Caddyfile) routes `/api/*` and `/healthz` to the Rust server and everything else to Next.js.
- [`docs/deploy-oci.md`](docs/deploy-oci.md) describes deployment to Oracle Cloud's Always Free tier.
- [`deploy/native/`](deploy/native/) contains native systemd/Caddy deployment scripts.

For a Docker deployment, set at least `DOMAIN` and your provider keys in `.env`, then run:

```bash
docker compose -f compose.prod.yml up -d --build
```

Persistent game data and Caddy certificates are kept in Docker volumes.

## Implementation notes

- The server validates AI output instead of trusting it blindly.
- Movement is limited to open exits defined in course data.
- Lesson credit is limited to real grammar IDs and spoken or typed lines.
- Character voices and generated audio assets are cached to avoid regenerating them.
- SQLite is the current single-instance database. Scaling beyond one server will require a shared database and shared/centralized rate limiting.

## License and attribution

Lexical's story and characters are original. The curriculum structure follows the Genki I learning sequence and is not affiliated with or endorsed by the publisher.

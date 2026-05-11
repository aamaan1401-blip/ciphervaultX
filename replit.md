# CipherVault X

A futuristic quantum-secure AI communication platform featuring real-time encrypted chat, multiple rooms, cyberpunk UI, and an AI assistant.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 8080)
- `pnpm --filter @workspace/ciphervault run dev` — run the frontend (port 19239)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- Frontend: React + Vite + Tailwind CSS (cyberpunk dark theme)
- API: Express 5
- Real-time: Socket.io (WebSocket on `/api/ws/socket.io`)
- Encryption: AES-256 via crypto-js (client-side only)
- No database — fully in-memory

## Where things live

- `artifacts/ciphervault/` — React frontend (login, chat room, components)
- `artifacts/ciphervault/src/lib/crypto.ts` — AES-256 encrypt/decrypt
- `artifacts/ciphervault/src/lib/socket.ts` — Socket.io client
- `artifacts/ciphervault/src/lib/paradox-ai.ts` — PARADOX AI assistant
- `artifacts/ciphervault/src/hooks/useRoom.ts` — room state management
- `artifacts/api-server/src/lib/socket.ts` — Socket.io server logic (rooms, users, relay)

## Architecture decisions

- Server only relays encrypted payloads — never sees plaintext; zero DB storage
- AES-256 room password = encryption key, set at room creation, shared out-of-band
- Socket.io path is `/api/ws/socket.io` so the shared proxy routes it through `/api/ws`
- AI assistant (@paradox) runs fully client-side — no API calls, rule-based inference only
- In-memory room state on server — resets on restart (by design, zero persistence)

## Product

- **Login**: Enter alias, vault channel name, and encryption key (shared secret)
- **Chat**: Real-time E2E encrypted messages with reactions, self-destruct timers, typing indicators
- **AI**: Type `@paradox <query>` to trigger the PARADOX AI assistant
- **Rooms**: Multiple concurrent rooms, shareable invite links with key embedded in URL
- **Dashboard**: Live user count, message stats, encryption status per room
- **UI**: Cyberpunk dark theme, matrix rain toggle, sound effects, glassmorphism panels

## User preferences

_Populate as you build — explicit user instructions worth remembering across sessions._

## Gotchas

- WebSocket path must be listed in `artifact.toml` paths: `/api/ws` is registered
- After modifying the API server, always rebuild (the dev script does build + start)
- crypto-js types need `@types/crypto-js` installed as devDependency on the frontend

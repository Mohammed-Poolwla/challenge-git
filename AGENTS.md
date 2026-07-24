# AGENTS.md

## Cursor Cloud specific instructions

`example-server` is a single Node.js HTTP JSON API (not a monorepo). Entry point `index.js` -> `lib/server.js` (router/CORS/logging) -> `lib/api.js` handlers -> `lib/models/things.js` -> `lib/db.js` (LevelUP). Standard commands live in `package.json` `scripts`.

### Datastore selection is driven by `NODE_ENV` (non-obvious)
`lib/db.js` picks the LevelUP backend from `NODE_ENV`:
- `test` -> `memdown` (in-memory; auth is also stubbed in this mode)
- `development` -> `leveldown` (on-disk LevelDB at `DB_PATH` or `./db`)
- `production` -> `mongodown` (external MongoDB)

Run the dev server with `NODE_ENV=development` explicitly (e.g. `NODE_ENV=development PORT=5000 npm run dev`). If `NODE_ENV` is unset the backend engine resolves to `undefined`. No external DB is needed for `development`/`test`; MongoDB is only for `production`.

### Auth on `/things/*` routes
`GET/POST /things/*` are wrapped by `lib/authify.js` (Authentic, `AUTHENTIC_HOST`) and require a valid `@lincx.la`/`@interlincx.com` JWT. Without a token they return 401 (expected). Auth is bypassed only under `NODE_ENV=test`, so the automated tests exercise the storage flow without an external auth server. Testing the real auth flow locally requires a reachable Authentic server + valid token.

### Tests / lint
- `NODE_ENV=test node test/index.js` runs the tape suite (24 assertions, all pass).
- `npm test` runs the suite, then `npm run deps` (passes), then `standard` lint. Lint currently FAILS on pre-existing extra-semicolon style errors in `test/routes.js`, so `npm test` exits non-zero even though every test passes. Treat that lint failure as pre-existing (do not "fix" unrelated code just to make `npm test` green).

Free endpoints for a quick smoke test: `/health`, `/echo?a=1`, `/reverse/<string>`.

# AGENTS.md

## Cursor Cloud specific instructions

### Overview

This is a lightweight UE5 Pixel Streaming signaling server and browser SDK. The main service is `signal.js` (Node.js WebSocket/HTTP server). There is no `package.json`, build system, linter, or test framework.

### Running the signaling server

```bash
sudo /home/ubuntu/.nvm/versions/node/v22.22.2/bin/node signal.js
```

Port 88 (configured in `signal.json`) requires root/sudo. Since Node.js is installed via nvm under the `ubuntu` user, you must use the full path to `node` when running with `sudo`.

### Key caveats

- **No package.json**: Dependencies are managed manually. The only dependency is `ws` (WebSocket library) in `node_modules/`.
- **No linter/tests/build**: This project has no automated testing, linting, or build pipeline.
- **Port 88 requires sudo**: The default port is privileged. Use `sudo` with the full node path.
- **signal.json is mutable at runtime**: The server's POST `/signal` endpoint modifies `signal.json` on disk. Be aware of this when testing configuration changes.
- **Full E2E streaming requires UE5 + GPU**: The signaling server can be tested independently (HTTP serving, WebSocket connections, player/engine pairing logic), but actual video streaming requires an Unreal Engine 5 instance with a GPU.
- **The `start` command on line 611 of signal.js**: This attempts to open a browser on startup (Windows-specific). It will fail silently on Linux — this is expected.

### Testing the server

1. Start server: `sudo /home/ubuntu/.nvm/versions/node/v22.22.2/bin/node signal.js`
2. HTTP test: `curl http://localhost:88/signal.html` (should return HTML)
3. WebSocket test: Connect to `ws://localhost:88/` with sub-protocol `peer-stream` to simulate a player
4. Admin GUI: Open `http://localhost:88/signal.html` in a browser
